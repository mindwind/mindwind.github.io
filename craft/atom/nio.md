---
layout    : craft-post
title     : craft-atom-nio
date      : 2014-10-24
author    : mindwind
permalink : /craft/atom/nio/
---


## Introduction
An asynchronous event-driven net I/O component base on java NIO,
it implements [atom-io](/craft/atom/io/)
API model definition.

## Getting Started
Before getting started, you should understand the reactor I/O model,
defined in the [atom-io](/craft/atom/io/).
Then we get started with a simple example,
and you will learn how to write a client and server on top of
[atom-nio](#).

### Writing an Echo Server
In order to use [atom-nio](#)
the only thing to do is implements `IoHandler` which handles I/O events.
An example as follows:

{% include_relative nio-code-server-1.md %}

So far so good. We have implemented the first part of the Echo Server.
What's left now is to write the `main()` method which starts the server with the `NioEchoServerHandler`,
as follows:

{% include_relative nio-code-server-2.md %}

Congratulations! Our Echo Server done.


### Writing an Echo Client
It is similar to Echo Server, first we also need implement a client handler, as follows:

{% include_relative nio-code-client-1.md %}

An client with `main()` method as follows:

{% include_relative nio-code-client-2.md %}


## Advanced
Actually above Echo Server and Client example lack a important part in network programming.
The missing part is protocol codec, because they don't need it,
but a complete client and server must handle protocol codec.

### Protocol Codec
For simplicity and atomicity [atom-nio](#) is not responsible for protocol codec.
The responsibility of protocol codec belong to [atom-protocol](/craft/atom/protocol/)
series components.


### Thread Model
[Thread Model Illustration](/craft/atom/nio/thread/): How to choose thread model？


### Event Model
[Event Model Illustration](/craft/atom/nio/event/): Understand event definition and how to handle.


### Performance
[Benchmark and Comparison with mina and netty](/craft/atom/nio/benchmark/): How fast?
