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


## 总结



## 参考
[1] Hadoop Documentation. [HDFS Architecture](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html).
[2] Robert Chansler, Hairong Kuang, Sanjay Radia, Konstantin Shvachko, and Suresh Srinivas. [The Hadoop Distributed File System](http://www.aosabook.org/en/hdfs.html)
[3] Tom White. [Hadoop: The Definitive Guide](http://book.douban.com/subject/10464777/). O'Reilly Media(2012-05), pp 94-96
