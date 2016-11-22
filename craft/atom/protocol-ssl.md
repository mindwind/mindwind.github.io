---
layout    : craft-post
title     : atom-protocol-ssl
date      : 2014-10-29
author    : mindwind
permalink : /craft/atom/protocol/ssl/
---


## Introduction
A simple SSL protocol implementor, it wraps `SSLEngine` class which from JDK
to make it use easily.


## Getting Started
First of all, we look at an example of SSL decode as follows:

{% include_relative protocol-ssl-code-codec.md %}

`SslCodec` need a `SslHandshakeHandler` implementor to handle the SSL handshake.
Because SSL handshake is related to transport implementation,
here `NioSslHandshakeHandler` is a example using I/O `channel` as the transport.

{% include_relative protocol-ssl-code-handshake.md %}
