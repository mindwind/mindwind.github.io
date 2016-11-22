---
layout    : craft-post
title     : Performance of Armor
date      : 2015-04-28
author    : mindwind
permalink : /craft/armor/performance/
---


I think you would care about the performance cost using [Armor](/craft/armor/).
We have some comparative benchmarks as follows.


## Performance Cost of Armor
Benchmark detail please see the test source code [BenchmarkArmor](https://github.com/mindwind/craft-armor/blob/master/src/test/java/io/craft/armor/BenchmarkArmor.java).
Run the benchmark test we execute 1 million invocation with or without Armor,
then we got the average elapse time of each invocation.

| Without Armor | With Armor |
|----------------------------|
|     2.08 ns   | 16379.3 ns |

Above comparison looks like has huge gap, but its absolute value is small.
Because `1ms == 1000000ns`, in a real world business process `16379.3ns` is very small.
So don't worry about performance of [Armor](/craft/armor/), if you don't abuse it everywhere.


## Thread Isolation of Armor Bring Extra Overhead?
[Armor](/craft/armor/) provide a kind of capability for thread isolation.
What about mutual performance influence of isolated thread execution?
Benchmark detail please see the test source code [BenchmarkForIsolationArmor](https://github.com/mindwind/craft-armor/blob/master/src/test/java/io/craft/armor/BenchmarkForIsolationArmor.java).
We see two cases as below.


### A Timeout case
Code snippet as below.
{% include_relative performance-code-timeout.md %}

![](/craft/armor/performance-timeout.png)

As shown above the figure, this is a interesting case while timeout occurs its thread pool would be slow and blocked. The method of `normal`, its thread pool has more CPU time to execute,
so it has more iterations per millisecond.


### An Error case
Code snippet as below.
{% include_relative performance-code-error.md %}

![](/craft/armor/performance-error.png)

As shown above the figure, while error occurs performance goes down slightly.
I think it is attribute to error handling overhead.
