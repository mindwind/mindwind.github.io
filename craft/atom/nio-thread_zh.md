---
layout    : craft-post-zh
title     : atom-nio 线程模型说明
date      : 2014-10-28
author    : mindwind
permalink : /craft/atom/nio/thread/zh/
---


在我们开始说明 [atom-nio](/craft/atom/nio/zh/) 线程模型之前，
想想线程模型自身到底意味着什么？ 它意味着线程协作机制和线程数量决策。

## 单一线程做所有事情
![Single Thread](/images/doc-craft-atom-nio-thread-model-1.png)

如图1所示，仅有一个线程做所有事情，包括：

* 接受或发起连接请求。
* 读写数据。
* 执行业务操作。

这是最简单的线程模型，同时也显得太简单太天真了。 因为单线程十分低效，任意时刻只有一个任务能被处理。

## 按责任区分线程
![Thread Division](/images/doc-craft-atom-nio-thread-model-2.png)

另一种线程模型如图2所示，我们按责任区分了线程，例如：

* 一个线程负责接受或发起连接请求。
* 少量线程负责读写数据。
* 一组线程负责执行业务操作。

是的，这个线程模型十分适合 [atom-io](/craft/atom/io/zh/) API 模型。
`IoAcceptor` 或 `IoConnector` 持有一个线程负责接受或发起连接请求，这个任务很简单，所以一个线程足已。
`IoProcessor` 保有少量线程负责数据的读写，这类任务通常是 CPU 密集型的，所以默认的线程数量是 CPU 核数。

最后我们是否需要一组独立的线程来负责执行业务操作，它依赖于业务操作的负载。
因为线程切换存在固有成本，加入业务操作的负载是远远大于线程切换的成本，
那么你应当使用一组独立的线程来执行业务操作以获得更高的吞吐能力。
