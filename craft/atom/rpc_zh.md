---
layout    : craft-post-zh
title     : craft-atom-rpc
date      : 2014-10-31
author    : mindwind
permalink : /craft/atom/rpc/zh/
---


## 简介
高效且易于使用的 java RPC 组件。 它提供：

  * 同步远程调用。
  * 异步远程调用。


## 入门
使用该 RPC 组件包括下面 2 个步骤：

  1. 对于服务端，导出远程 API。
  2. 对于客户端，导入远程 API 并调用它。


### 导出远程 API
只有导出的 API 能够从远程被调用，代码示例如下：
{% include_relative rpc-code-export-1.md %}

只导出部分方法而不是整个接口。
{% include_relative rpc-code-export-2.md %}

对于面向对象编程的多态性，假如一个接口有多个实现者，如何导出呢？调用哪个实现？
{% include_relative rpc-code-export-3.md %}


### 导入远程 API
{% include_relative rpc-code-import.md %}


### 多态调用
`DemoService` 还有另外一个实现并且以 ‘demo2’ 作为 rpc 标识导出。
{% include_relative rpc-code-polymorphic.md %}


### 异步调用
默认的远程调用都是同步的，发起一个异步调用的代码示例如下：
{% include_relative rpc-code-asynchronous.md %}


### 单向调用
单向调用是一种特殊类型的异步调用，意味着客户端对本次调用不期待服务端的响应结果。
实际上服务端对于单向调用请求也不会作出响应。对于特定场景单向调用性能更好，但并不那么可靠。
代码示例如下：
{% include_relative rpc-code-oneway.md %}
