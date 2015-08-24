---
layout    : post
title     : 后端分布式系列：分布式存储-HDFS Client 设计实现解析
date      : 2015-08-27
author    : mindwind
categories: blog
tags      : 分布式存储 HDFS Client
image     : /assets/article_images/2015-08-27.jpg
---


前面对 HDFS NameNode 和 DataNode 的架构设计实现要点做了介绍，
本文对最后一个主要构成组件 HDFS Client 做进一步解析。


## 流式读取
HDFS Client 为客户端应用提供一种流式读取模型，就像访问本机文件系统一样来访问 HDFS。
将复杂的分布式文件系统读取细节隐藏，简化了上层应用的使用难度。
写过读取过本机文件的程序想必都很熟悉流式读取的编程模型，就不多说了。


## 错误处理
相比读取本机文件系统，从分布式文件系统读取出错概率会更高。
因此 HDFS Client 提供了一些附加功能来提升分布式文件系统读取访问的可用性。
在从某个 DataNode 读取数据的过程中若发生错误异常，Client 会透明的转移到距离第二接近的 DataNode 上，
并记住第一个 DataNode 读取失败，后续的 blocks 读取将不再尝试该 DataNode。
除此之外 Client 对于读到的每个 block 进行 checksum 校验，若读到损坏的 block，则向 NameNode 汇报，
并尝试从其他副本重新读取。


## 缓冲写入
创建文件并写入数据的操作并不是直接连到 DataNode 同步远程写入的，而是通过写入本地的一个临时文件来做缓冲？
我们写本地文件也经常使用一种 BufferedWriter 来提高写入吞吐能力。
本质上都是为了解决数据生产端和数据接收端处理能力的差异，在单机情况下磁盘操作慢，所以用内存 buffer 来缓冲。
在分布式环境下，不仅要考虑磁盘还要考虑网络，所以用本地内存加上本地磁盘文件来做缓冲。

应用写 HDFS 的操作被透明的转移到写入本地文件，当本地文件积累的数据超过一个 HDFS block 的大小后，
Client 再请求 NameNode 分配 DataNodes，Client 再将本地文件的数据一次性的发送到对应的 DataNodes 流水线处理。
这个过程实际将同步写转为了移步写过程，提高了写入吞吐性能。

当文件被关闭后，在 Client 端临时文件中剩下的数据将被传输给 DataNode。
然后 Client 告知 NameNode 文件已关闭，写入完成。
NameNode 此时才将新写入的文件持久化，若在文件关闭前 NameNode 宕机，则正在写入的文件算作丢失了。


## 总结
Client 在 HDFS 的三个主要部件中相对简单，在设计实现时更多考虑易用性、容错和性能。
至此，对 HDFS 的三个主要部件 NameNode、DataNode 和 Client 的设计实现要点进行了讲述，
后续会以主题文章对其中一些关键的技术点做进一步剖析。


## 参考
[1] Hadoop Documentation. [HDFS Architecture](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html).
[2] Robert Chansler, Hairong Kuang, Sanjay Radia, Konstantin Shvachko, and Suresh Srinivas. [The Hadoop Distributed File System](http://www.aosabook.org/en/hdfs.html)
[3] Tom White. [Hadoop: The Definitive Guide](http://book.douban.com/subject/10464777/). O'Reilly Media(2012-05), pp 94-96
