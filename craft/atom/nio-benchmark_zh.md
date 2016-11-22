---
layout    : craft-post-zh
title     : 性能基准测试及与 mina 和 netty 的对比
date      : 2014-10-29
author    : mindwind
permalink : /craft/atom/nio/benchmark/zh/
---


## 测试环境

测试软硬件环境如下:

| 硬件                                   | 软件                                              |
|---------------------------------------|--------------------------------------------------|
| CPU model : Intel(R) Core(TM) i5-2320 | Linux         : CentOS 5.7, kernel 2.6.18-274.el5
| CPU cores : 4                         | JDK           : 1.6.0_30-b12
| CPU speed : 3.00GHz                   | JMeter        : 2.4
| Cache size: 6144 KB                   | Mina          : 2.0.7
| Network   : 1000 Mbps                 | Netty         : 3.6.2.Final
| Memory    : -Xms256m -Xmx256m         | atom-nio      : 2.0.0


## 测试方法
使用 [Mina](http://mina.apache.org)，[Netty](http://netty.io) 和 [atom-nio](/craft/atom/nio/zh/) 各自独立实现一个 Echo Server。
线程与缓冲区配置如下：

| atom-nio            | Mina                | Netty               |
|---------------------|---------------------|---------------------|
| acceptor : 1 thread | acceptor : 1 thread | boss    : 1 thread
| processor: 4 thread | processor: 4 thread | worker  : 4 thread
| executor : 4 thread | executor : 4 thread | executor: 4 thread
| buffer   : 2k       | buffer   : 2k       | buffer  : 2k


## 测试指标

| TPS                    | CPU             | ART                   | 90% RT            |
|------------------------|-----------------|-----------------------|-------------------|
| 每秒事务数               | CPU 利用率       | 平均响应时间            | 90% 响应时间        |


## 测试结果

  * Data Size - 1k  
    ![1k](/images/doc-craft-atom-nio-benchmark-1k.jpg)
  * Data Size - 2k  
    ![2k](/images/doc-craft-atom-nio-benchmark-2k.jpg)
  * Data Size - 5k  
    ![5k](/images/doc-craft-atom-nio-benchmark-5k.jpg)
  * Data Size - 10k  
    ![10k](/images/doc-craft-atom-nio-benchmark-10k.jpg)
  * Data Size - 50k  
    ![50k](/images/doc-craft-atom-nio-benchmark-50k.jpg)
  * Data Size - 100k  
    ![100k](/images/doc-craft-atom-nio-benchmark-100k.jpg)
