---
layout    : craft-post-zh
title     : Armor 性能
date      : 2015-04-28
author    : mindwind
permalink : /craft/armor/performance/zh/
---


我认为你会关心使用 [Armor](/craft/armor/zh/) 后带来的性能开销。
我们做了一些性能基准比较测试，如下。


## Armor 的性能开销
基准测试的细节请参见测试源代码 [BenchmarkArmor](https://github.com/mindwind/craft-armor/blob/master/src/test/java/io/craft/armor/BenchmarkArmor.java)。
执行这个测试会分别调用一百万次基于 Armor 增强的或没有 Armor 的方法，然后算出一个平均每次调用的耗时。

| Without Armor | With Armor |
|----------------------------|
|     2.08 ns   | 16379.3 ns |

上面的结果比较看起来差距很大，但实际上它的绝对值很小。
因为 `1毫秒 == 1000000纳秒`，在真实世界的业务处理中 `16379.3 纳秒` 是非常小的。
所以不用担心 [Armor](/craft/armor/zh/) 的性能开销，只要你不是到处的滥用它。


## Armor 的线程隔离会带来额外的性能开销么？
[Armor](/craft/armor/zh/) 提供了一种线程隔离的能力。隔离线程执行的相互性能影响如何？
基准测试的细节请参见测试源代码 [BenchmarkArmor](https://github.com/mindwind/craft-armor/blob/master/src/test/java/io/craft/armor/BenchmarkForIsolationArmor.java)。
我们看下面两个测试案例。


### 超时案例（Timeout）
代码片段如下。
{% include_relative performance-code-timeout.md %}

![](/craft/armor/performance-timeout.png)

如上图所示，这是一个很有趣的例子，当超时发生时它的线程池将变慢而阻塞住。
方法 `normal` 的线程池则会拥有更多的 CPU 时间去执行，因此结果是超时发生后每毫秒的迭代次数反而增加了。


### 错误案例（Error）
代码片段如下。
{% include_relative performance-code-error.md %}

![](/craft/armor/performance-error.png)

如上图所示，当错误发生时性能会有微微的下降。我认为这归因于错误处理本身的开销。
