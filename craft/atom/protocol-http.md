---
layout    : post
title     : craft-atom-protocol-http
date      : 2014-10-29
author    : mindwind
permalink : /craft/atom/protocol/http/
---


## Introduction
A simple http protocol implementor, which implements
[atom-protocol](/craft/atom/protocol/) API definition.


## Getting Started
First of all, we look at an example of http request decode and response encode as follows:

{% include_relative protocol-http-code-codec.md %}

But now we just implement http request decoder and response encoder,
so you can only use it to implement your own http server.
