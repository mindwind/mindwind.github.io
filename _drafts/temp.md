---
layout    : post
title     : Temp
date      : 2015-09-24
author    : mindwind
categories: blog
tags      : 技能
image     : /assets/article_images/2015-09-24.jpg
elapse    :
---

## 参考
[1] antirez. [Redis Cluster Specification](http://redis.io/topics/cluster-spec)  
Very high performance and scalability while preserving weak but reasonable forms of data safety and availability is the main goal of Redis Cluster.

In Redis Cluster nodes don't proxy commands to the right node in charge for a given key, but instead they redirect clients to the right nodes serving a given portion of the key space.

The key space is split into 16384 slots, effectively setting an upper limit for the cluster size of 16384 master nodes (however the suggested max size of nodes is in the order of ~ 1000 nodes).

Redis Cluster is a full mesh where every node is connected with every other node using a TCP connection.
