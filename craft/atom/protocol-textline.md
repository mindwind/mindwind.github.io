---
layout    : craft-post
title     : atom-protocol-textline
date      : 2014-10-29
author    : mindwind
permalink : /craft/atom/protocol/textline/
---


## Introduction
A simple textline protocol implementor, which implements
[atom-protocol](/craft/atom/protocol/) API.


## Getting Started
First of all, we look at an example of textline decode as follows:

{% include_relative protocol-textline-code-decode.md %}

Above we read some bytes from network, and decode by textline decoder with delimiter character `\n`.
So simple, for the textline encoder it is more simple than decoder,
so there is no need to describe it, spend ten seconds to see the source code
you can completely understand it.
