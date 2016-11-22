---
layout    : craft-post-zh
title     : atom-protocol
date      : 2014-10-28
author    : mindwind
permalink : /craft/atom/protocol/zh/
---


## 简介
一个基本协议解析 API 定义，其他组件例如
[atom-protocol-textline](/craft/atom/protocol/textline/zh/)
实现该 API 定义。 协议解析包含两个部分：解码和编码。


## 协议解码
协议解码处理来自网络的二进制数据，将子节解码为高级的协议对象。 协议解码 API 定义如下：

{% include_relative protocol-code-decoder.md %}

来自网络的字节是基于流的，因此协议解码器实现一般会保存临时的解码中间状态。


## 协议编码
协议编码处理高级的协议对象，将其转变为二进制数据以便通过网络传输。 协议编码 API 定义如下：

{% include_relative protocol-code-encoder.md %}

## 实现者
协议解析实现者通常会配合组件 [craft-atom-nio](/craft/atom/nio/zh/)
一起使用。 现在我们拥有的实现如下：

  * [atom-protocol-rpc](/craft/atom/protocol/rpc/zh/)
  * [atom-protocol-ssl](/craft/atom/protocol/ssl/zh/)
  * [atom-protocol-http](/craft/atom/protocol/http/zh/)
  * [atom-protocol-textline](/craft/atom/protocol/textline/zh/)
