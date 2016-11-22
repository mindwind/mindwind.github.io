---
layout    : craft-post-zh
title     : atom-protocol-ssl
date      : 2014-10-29
author    : mindwind
permalink : /craft/atom/protocol/ssl/zh/
---


## 简介
一个简单的 SSL 协议实现，它通过封装来自 JDK 的 `SSLEngine` 类以让其更容易使用。


## 入门
首先，我们看一个 SSL 解码的示例如下：

{% include_relative protocol-ssl-code-codec.md %}

`SslCodec` 需要一个 `SslHandshakeHandler` 实现来处理 SSL 握手过程。
因为 SSL 握手与网络传输实现相关，下面 `NioSslHandshakeHandler`
是一个使用 I/O `channel` 作为传输通道的实现示例。

{% include_relative protocol-ssl-code-handshake.md %}
