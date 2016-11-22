---
layout    : craft-post-zh
title     : atom-protocol-rpc
date      : 2014-10-29
author    : mindwind
permalink : /craft/atom/protocol/rpc/zh/
---


## 简介
一个简单自定义的 RPC 协议实现，它实现了
[atom-protocol](/craft/atom/protocol/zh/) 定义的 API。
[atom-rpc](/craft/atom/rpc/zh/) 使用它作为默认的协议编解码实现。


## 入门
对 RPC 请求进行解码和响应编码的代码示例如下：

{% include_relative protocol-rpc-code-codec.md %}
