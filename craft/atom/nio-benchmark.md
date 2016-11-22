---
layout    : craft-post
title     : Benchmark and Comparison with Mina and Netty
date      : 2014-10-29
author    : mindwind
permalink : /craft/atom/nio/benchmark/
---


## Test Environment

The test environment as follows:

| Hardware                              | Software                                         |
|---------------------------------------|--------------------------------------------------|
| CPU model : Intel(R) Core(TM) i5-2320 | Linux         : CentOS 5.7, kernel 2.6.18-274.el5
| CPU cores : 4                         | JDK           : 1.6.0_30-b12
| CPU speed : 3.00GHz                   | JMeter        : 2.4
| Cache size: 6144 KB                   | Mina          : 2.0.7
| Network   : 1000 Mbps                 | Netty         : 3.6.2.Final
| Memory    : -Xms256m -Xmx256m         | atom-nio      : 2.0.0


## Test Way
Build an Echo Server independently using [Mina](http://mina.apache.org),
[Netty](http://netty.io) and [atom-nio](/craft/atom/nio/).
Thread and buffer config as follows:

| atom-nio            | Mina                | Netty               |
|---------------------|---------------------|---------------------|
| acceptor : 1 thread | acceptor : 1 thread | boss    : 1 thread
| processor: 4 thread | processor: 4 thread | worker  : 4 thread
| executor : 4 thread | executor : 4 thread | executor: 4 thread
| buffer   : 2k       | buffer   : 2k       | buffer  : 2k


## Test Indicator

| TPS                    | CPU             | ART                   | 90% RT            |
|------------------------|-----------------|-----------------------|-------------------|
| Transaction Per Second | CPU utilization | Average Response Time | 90% Response Time |


## Test Result

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
