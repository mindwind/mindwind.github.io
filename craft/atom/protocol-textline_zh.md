---
layout    : craft-post-zh
title     : atom-protocol-textline
date      : 2014-10-29
author    : mindwind
permalink : /craft/atom/protocol/textline/zh/
---


## 简介
一个简单文本换行协议实现，它实现了
[atom-protocol](/craft/atom/protocol/zh/) 定义的 API。


## 入门
首先，我们看一个关于 text-line 请求解码的例子，如下：

{% include_relative protocol-textline-code-decode.md %}

如上我们从网络读取了一些字节，并基于换行符 `\n` 解码。
如此简单，对于 textline 编码则更简单了，所以没必要再详细描述了，花上 10 秒钟看看源码你就能完全理解它。
