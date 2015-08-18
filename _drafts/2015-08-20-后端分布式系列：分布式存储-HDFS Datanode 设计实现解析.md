---
layout    : post
title     : 后端分布式系列：分布式存储-HDFS Datanode 设计实现解析
date      : 2015-08-20
author    : mindwind
categories: blog
tags      : 分布式存储 HDFS Datanode
image     : /assets/article_images/2015-08-20.jpg
---




HDFS 的文件存储方式是将文件按块（block）切分，默认一个 block 64MB。
若文件大小超过一个 block 的容量可能会被切分为很多个 block，存储在不同的 Datanode 上。
若文件大小小于一个 block 的容量，则文件只有一个 block，实际占用的存储空间为文件大小容量加上一点额外的校验数据。



## 参考
[1] Hadoop Documentation. [HDFS Architecture](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html).
[2] Robert Chansler, Hairong Kuang, Sanjay Radia, Konstantin Shvachko, and Suresh Srinivas. [The Hadoop Distributed File System](http://www.aosabook.org/en/hdfs.html)
