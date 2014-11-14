---
layout    : post
title     : netty与mina性能对比测评分析
date      : 2013-02-28
author    : mindwind
categories: 2013
tags      : netty mina 性能
image     : /assets/article_images/2013-02-28.jpg
---


_「netty 和 mina 都是 java nio 网络框架著名的开源项目，如何在二者间进行选择曾经一直困扰着我，
本文以性能为切入点，对比分析下 netty 和 mina 在实现上的一些细微差异」_


## 测试方法
采用 mina 和 netty 各实现一个 基于 nio 的EchoServer，测试在不同大小网络报文下的性能表现。

# 测试环境
<pre>
客户端-服务端：
     model name: Intel(R) Core(TM) i5-2320 CPU @ 3.00GHz
     cache size: 6144 KB
     cpu cores:  4
     jdk:        1.6.0_30-b12
     network:    1000Mb
     memory:     -Xms256m -Xmx256m
     Linux:      centos 5.7, kernel 2.6.18-274.el5

测试工具：
     jmeter v2.4

版本：
     mina            2.0.7
     netty           3.6.2.Final

配置：
     mina
          io-processor cpu 核数
          executor     cpu 核数
          buffer       初始 buffer 大小，设置为 2048（2k）
     netty
          boss         netty 默认配置 1
          worker       cpu 核数
          executor     cpu 核数

     mina  线程池设置
          io processor：
          IoAcceptor acceptor = new NioSocketAcceptor(Integer.parseInt(ioPool));
          executor：
          acceptor.getFilterChain().addLast("threadPool",
                  new ExecutorFilter(Integer.parseInt(executorPool)));
     netty 线程池设置
          io worker:
          new NioWorkerPool(Executors.newCachedThreadPool(),Integer.parseInt(ioPool))
          executor:
          new OrderedMemoryAwareThreadPoolExecutor(Integer.parseInt(executorPool), 0, 0)
</pre>


## 测试结果
<pre>
mina        tps        cpu        network io      art(average response time)      90%rt
1k          45024/sec  150%        50MB/sec               < 1ms                    1ms
5k          10155/sec  90%         55MB/sec               3 ms                     1ms
10k         8740/sec   137%        98MB/sec               3 ms                     4ms
50k         1873/sec   128%        100MB/sec              16ms                     19ms
100k        949/sec    128%        100MB/sec              33ms                     43ms
</pre>

<pre>
netty        tps        cpu        network io      art(average response time)     90%rt
1k          44653/sec   155%        50MB/sec              < 1ms                    1ms
2k          35580/sec   175%        81MB/sec              < 1ms                    1ms
5k          17971/sec   195%        98MB/sec              1ms                      2ms
10k         8806/sec    195%        98MB/sec              3ms                      4ms
50k         1909/sec    197%        100MB/sec             16ms                     18ms
100k        964/sec     197%        100MB/sec             32ms                     45ms
</pre>


## 测试点评
mina 和 netty 在 1k、2k、10k、50k、100k 报文大小时 tps 接近。
mina 在 5k 报文时有个明显的异常（红色标注），tps 较低，网络 io 吞吐较低，比较 netty 在 5k 报文的网络 io 吞吐 98MB/sec（基本接近前兆网卡极限）。
5k 报文以上基本都能压满网络 io，瓶颈在 io，所以 tps 和 响应时间基本相差不大。
疑问，为什么 mina 会在 5k 报文时 io 吞吐出现明显降低？


## 测试分析
通过分析 mina 和 netty 的源码，发现在处理 io 读事件时 buffer 的分配策略上，两个框架有一些区别。
在网络 io 处理上，程序每次调用 socket api 从 tcp buffer 读取的字节数是随时变化的，它会受到报文大小，操作系统 tcp buffer 大小、tcp 协议算法实现、网络链路带宽等各种因素影响。
因此 nio 框架在处理每个读事件时，也需要每次动态分配一个 buffer 来临时存放读到的字节，buffer 分配的效能是影响网络 io 框架程序性能表现的关键因素。
下面分别分析下 mina 和 netty buffer 的动态分配实现。

### mina

  * buffer 分配方式：  
    默认实现采用了 HeapByteBuffer，每次都是直接调用 ByteBuffer.allocate(capacity) 直接分配。
  * buffer 分配大小预测：  
    根据每次读事件实际读到的字节数计算分配 buffer 的大小，若实际读到的字节将 ByteBuffer 装满，说明来自网络的数据量可能较大而分配 buffer。   容量不足，则扩大 buffer 一倍。
    若连续 2 次读到的实际字节数小于 buffer 容量的一半，则缩小 buffer 为原来的一半。

### netty

  * buffer 分配方式：  
    默认实现采用了 DirectByteBuffer，并且实现了 buffer cache，只要 buffer 大小不改变就会重复利用已经分配的 buffer。
  * buffer 分配大小预测：  
    初始化了一张 buffer size 静态分配表如下（截取部分），假如当前默认 buffer 为 2048。

<pre>
[1024, 1152, 1280, 1408, 1536, 1664, 1792, 1920, 2048, 2304, 2560, 2816, 3072, 3328, 3584]
                                           |      |    |                       |
                                           A      D    B                       C

</pre>

根据每次读事件实际读到的字节数，进行预测下一次应该分配的 buffer 大小。
若实际读到的字节数大于等于当前 buffer（上图 B 位置） 则将当前 buffer 增大到上图示 C 位置，每次增大步进为 4
若连续 2 次实际读到的字节数小于等于 A 位置指示的 buffer 大小，则缩小 buffer 到 D 位置
从上面的对比分析可以看出，mina 采用了相对简单的 buffer 分配和预测方式，buffer 的增长和缩小比例相同。
而 netty 采用了一种相对复杂点的 buffer 分配方式，buffer increment faster decrement slower。
事实证明 netty 的分配方式更有效的避免的 buffer 分配中的抖动问题（忽大忽小），而 buffer 分配抖动正是影响 io 吞吐的罪魁祸首。
mina 测试中 5k 报文正好引发了 buffer 的分配抖动，导致 io 吞吐大受影响，通过修改 mina 源码采用固定 buffer 大小来测试则有效避免了 buffer 抖动，io 吞吐也恢复正常。
但实际情况下，固定 buffer 肯定不是有效的方式，不能很好的适应各种网络环境的复杂性，但采用动态 buffer 分配时算法需首要考虑避免抖动。
另外可以看出 netty 的 cpu 消耗明显高出 mina 不少，怀疑 netty 采用的 executor 实现（OrderedMemoryAwareThreadPoolExecutor）存在比较多的锁竞争和线程上下文切换。

下面是一组不使用 executor 时 netty 的测试数据，其他指标都差不多但 cpu 明显下降不少

<pre>
netty			cpu
1k				75%
2k				125%
5k				126%
10k				126%
50k				118%
100k			116%
</pre>

## 测试总结
mina: 需进一步优化其 buffer 分配，避免分配抖动导致的 io 吞吐波动。
netty： 需进一步优化默认 executor 的实现，降低 cpu 消耗。

其实 netty2 和 mina 都出自同一人 Trustin Lee，从使用者感受上来说 mina 的 API 接口设计明显比 netty 更优雅。
