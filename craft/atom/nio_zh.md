---
layout    : craft-post-zh
title     : atom-nio
date      : 2014-10-24
author    : mindwind
permalink : /craft/atom/nio/zh/
---


## 简介
基于 java NIO 的异步事件驱动网络 I/O 组件，实现了 [atom-io](/craft/atom/io/zh/) API 模型定义。

## 入门
在开始之前，你应当理解在 [atom-io](/craft/atom/io/zh/) 中定义的 reactor I/O 模型。
然后我们以一个简单的例子开始学习如何基于 [atom-nio](#) 写一个 client 和 server 程序。

### 编写一个 Echo Server
为了使用 [atom-nio](#) 唯一需要做的就是实现 `IoHandler` 接口来处理 I/O 事件。 示例如下：

{% include_relative nio-code-server-1.md %}

到目前为止一切顺利，我们已经实现了 Echo Server 第一部分。
目前剩下的是写一个 `main()` 方法来启动 server，如下所示：

{% include_relative nio-code-server-2.md %}

恭喜！我们的 Echo Server 完成了。

### 编写一个 Echo Client
这类似于 Echo Server，首先我们也需要实现一个 client 的 handler 接口，如下：

{% include_relative nio-code-client-1.md %}

一个包含 main() 方法的 client 如下所示：

{% include_relative nio-code-client-2.md %}

## 进阶
实际上以上 Echo Server 和 Client 的示例还缺少一个在网络编程中重要的部分，那就是协议解析。
因为以上例子中并不需要协议解析，但一个完整的 client 和 server 是必须处理协议解析的。


### 协议解析
出于简洁和原子化的设计考虑，[atom-nio](#) 并不负责协议解析。
协议解析的职责属于 [atom-protocol](/craft/atom/protocol/zh/) 这一系列组件。

### 线程模型
[线程模型说明](/craft/atom/nio/thread/zh/)：如何使用线程模型？

### 事件模型
[事件模型说明](/craft/atom/nio/event/zh/): 理解事件的定义与处理。

### 性能
[性能基准测试及与 mina 和 netty 的对比](/craft/atom/nio/benchmark/zh/): 有多快？
