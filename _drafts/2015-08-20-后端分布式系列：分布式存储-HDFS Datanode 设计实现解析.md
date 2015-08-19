---
layout    : post
title     : 后端分布式系列：分布式存储-HDFS Datanode 设计实现解析
date      : 2015-08-20
author    : mindwind
categories: blog
tags      : 分布式存储 HDFS Datanode
image     : /assets/article_images/2015-08-20.jpg
---


前文分析了 Namenode，本文进一步解析 HDFS Datanode 的设计和实现要点。


## 文件存储
Datanode 正如其名是负责存储文件数据的节点。
HDFS 中文件的存储方式是将文件按块（block）切分，默认一个 block 64MB。
若文件大小超过一个 block 的容量可能会被切分为很多个 block，并存储在不同的 Datanode 上。
若文件大小小于一个 block 的容量，则文件只有一个 block，实际占用的存储空间为文件大小容量加上一点额外的校验数据。
也可以这么说一个文件至少由一个或多个 block 组成，而一个 block 仅属于一个文件。

block 是一个逻辑概念对象，由 Datanode 基于本地文件系统来实现。
每个 block 在本地文件系统中由两个文件组成，第一个文件包含文件数据本身，第二个文件则记录 block 的元信息（metadata）如：数据校验和（checksum）。
所以每一个 block 对象实际物理对应两个文件，但 Datanode 不会将文件创建在同一个目录下。
因为本机文件系统可能不能高效的支持单目录下的大量文件，Datanode 会使用启发式方法决定单个目录下存放多少文件合适并在适当时候创建子目录。

文件数据存储的可靠性依赖多副本保障，对于单一 Datanode 节点而言只需保证自己存储的 block 是完整并无损坏的。
Datanode 会主动周期性的运行一个 block 扫描器（scanner）通过比对 checksum 来检查 block 是否损坏。
另外还有一种被动的检查方式，就是当读取时检查。


## 文件操作


## 生命周期


## 总结


## 参考
[1] Hadoop Documentation. [HDFS Architecture](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html).
[2] Robert Chansler, Hairong Kuang, Sanjay Radia, Konstantin Shvachko, and Suresh Srinivas. [The Hadoop Distributed File System](http://www.aosabook.org/en/hdfs.html)
