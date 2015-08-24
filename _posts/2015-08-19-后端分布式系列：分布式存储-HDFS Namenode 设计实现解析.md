---
layout    : post
title     : 后端分布式系列：分布式存储－HDFS NameNode 设计实现解析
date      : 2015-08-19
author    : mindwind
categories: blog
tags      : 分布式存储 HDFS NameNode
image     : /assets/article_images/2015-08-19.jpg
---


接前文 [分布式存储-HDFS 架构解析]({% post_url 2015-08-18-后端分布式系列：分布式存储-HDFS 架构解析 %})，
我们总体分析了 HDFS 架构的主要构成组件包括：NameNode、DataNode 和 Client。
本文首先进一步解析 HDFS NameNode 的设计和实现要点。


## 元数据持久化
NameNode 将所有元信息以特定的数据结构组织存放在内存中，对于 namespace 和 replication factor 的信息会进行持久化，
而映射关系则不会持久化。因为映射关系是通过 DataNode 启动后定时汇报上来，
即使 NameNode 重启后内存信息丢失也可以通过 DataNode 重新汇报获得，而其他元信息则必须通过读取持久化存储来重建内存数据结构。
出于性能原因，所有元信息的读取直接从内存中获得，而增删改操作则借鉴来数据库的事务日志技术。
每次只变更内存数据结构并记录操作日志，将随机写变为顺序写来提高吞吐能力。

NameNode 使用一个事务日志文件 EditLog 来持久化记录针对文件系统元数据的每一次操作变更。
而整个 file system namesapce 文件、文件块和数据节点的映射关系被存储在另一个 FsImage 文件中。
当 NameNode 启动时，它读取 FsImage 文件构建元数据的内存数据结构，接着读取 EditLog 文件应用所有的操作事务到内存数据结构中。
接着基于最新的内存数据结构重新写一个新的 FsImage 文件到磁盘上，然后清除 EditLog 的内容，
因为它上面记录的所有操作日志都已经反应到 FsImage 上了。这个过程称为建立了一个检查点(checkpoint)，当然检查点技术也是数据库里最常用的了。
启动过程中 NameNode 会进入一种特殊状态，称为安全模式（Safemode state），
它等待 DataNode 报告文件块及其副本的数量并确定哪些文件的副本数量是不足的。

所有上述启动过程完成后，NameNode 才能对外提供服务。


## 数据分布策略
除了文件系统元数据管理外，NameNode 另一个重要作用是对写入的文件块选择合适 DataNode 来存放，称为 block placement policy.
在通常情况下，replication factor（副本数）默认为 3， HDFS 的数据分布策略是将两份副本放在本地机架的两个不同 DataNode 上，
最后一个副本放在另外一个机架的一个 DataNode 上。

NameNode 基于机架感知的数据分布策略并未考虑 DataNode 的磁盘空间利用率。
这有效避免了将新的文件数据集中放置在一组拥有大量剩余空间的 DataNodes 上，比如新扩容的 DataNodes。
这又带来另外一个问题，就是扩容新 DataNode 时导致数据分布的严重不平衡。

为了应对此类情况，HDFS 引入了一个平衡器工具（Balancer），在整个集群平衡磁盘空间利用率。
这个工具通过引入设置一个在 0 和 1 之间的阈值来判断整个集群是否达到平衡。

HDFS 对于磁盘均衡的定义如下：

  > 对于每一个 DataNode 其磁盘利用率与整个集群的平均利用率相比不超过设置的阈值偏差。

平衡器工作过程中确保不会减少副本总数，及其分布的机架数。
也就是原来三个副本分布在两个机架，被平衡后也还是三个副本分布在两个机架，但可能移动到不同的 DataNode 上了。
为了减少网络带宽占用，会尽可能避免机架间拷贝数据。
例如：副本 A1 和 A2 在 Rack1，A3 在 Rack2，若平衡器认为 A1 所在 DataNode 磁盘利用率过高，需要移动 A1。
在 Rack1 内找不到磁盘利用率低的其他 DataNode，则需要将 A1 移动到 Rack2。
实际做法是直接从 Rack2 的 A3 复制一份得到 A4，并删除 Rack1 上的 A1，这样就避免了机架间复制。


## 复制管理
NameNode 努力确保每一个副本符合配置的 replication factor。
假如 DataNode 宕机发生导致一些文件的 block 副本数变少，Namdenode 会发出复制命令给 DataNode，复制出新的副本以保持与配置的副本数一致。
当宕机的 DataNode 恢复后重新加入集群后会导致一些文件的副本数超出配置数，NameNode 会检测到并删除多余的副本以节省存储空间。
NameNode 维护一个复制优先级队列，对于副本不足的文件 block 按优先级排序，仅剩下一个副本的文件 block 享有最高的复制优先级。


## 性能设计
除了 NameNode 的单点和重启过程影响可用性外，另一个担忧因素是性能。
读操作基于内存访问还好，写操作中磁盘是一个瓶颈点。
而 NameNode 支持大量 Client 的并发读写，对于大量的并发写操作 NameNode 进行了优化。
多线程情况下，当一个线程为保存操作事务日志发起一个 flush-and-sync 到磁盘文件的操作，其他线程只能等待。
为了优化此类情况，NameNode 将随机写转换为批量写操作。
当一个 NameNode 的线程初始化了一个 flush-and-sync 操作，所有当时的事务操作日志被批量写入文件。
其余的线程只需要检查它们的事务是否被保存到了文件而不再需要再发起 flush-and-sync 操作。


## 总结
上面描述了 NameNode 提供的功能及其设计实现要点，最后我们简单点评下它在架构设计上的权衡考量。
首先 NameNode 作为中心节点简化了整体设计，但很显然它也是个单点，会影响可用性。
其次 NameNode 将所有元数据存储在内存中，内存的容量决定了整个分布式文件系统能支持文件数量，如果是大量的小文件场景也是个问题。
而且 HDFS 基于 java 实现，java 针对大堆内存的 GC 优化也是个麻烦事。
再次上面描述的 NameNode 的启动过程看起来就很耗时，特别是在 FsImage 和 EditLog 都很大的情况下。
而且在 NameNode 的启动完成前整个 HDFS 是不可用的，所以 NameNode 即使是重启也对整体的可用性有很大影响。


## 参考
[1] Hadoop Documentation. [HDFS Architecture](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html).  
[2] Robert Chansler, Hairong Kuang, Sanjay Radia, Konstantin Shvachko, and Suresh Srinivas. [The Hadoop Distributed File System](http://www.aosabook.org/en/hdfs.html)
