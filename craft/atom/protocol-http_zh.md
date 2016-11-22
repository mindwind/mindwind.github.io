---
layout    : craft-post-zh
title     : atom-protocol-http
date      : 2014-10-29
author    : mindwind
permalink : /craft/atom/protocol/http/zh/
---


## 简介
一个简单 http 协议实现，它实现了
[atom-protocol](/craft/atom/protocol/zh/) 定义的 API。


## 入门
首先，我们看一个关于 http 请求解码和响应器编码的例子，如下：

{% include_relative protocol-http-code-codec.md %}

到目前我们仅仅实现了 http 请求解码和响应编码器，所以你只能用它来实现一个简单的 http 服务。
