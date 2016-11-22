---
layout    : craft-post
title     : craft-atom-rpc
date      : 2014-10-31
author    : mindwind
permalink : /craft/atom/rpc/
---


## Introduction
An easy use and efficient java RPC component.
It provides:

  * Synchronous remote invocation.
  * Asynchronous remote invocation.


## Getting Started
Using this RPC component include two steps as follows:

  1. Export remote API for server side.
  2. Import remote API and invoke it for client side.


### Export remote API
Only exported API can be invoked from remote peer, the code example as follows:
{% include_relative rpc-code-export-1.md %}

Not export entire interface just some methods.
{% include_relative rpc-code-export-2.md %}

For polymorphism of OOP, if a interface has more than one implementor, how to export?
Which to be invoked?
{% include_relative rpc-code-export-3.md %}


### Import remote API
{% include_relative rpc-code-import.md %}


### Polymorphic invocation
`DemoService` has another implementor and be exported with 'demo2' as its rpc id.
{% include_relative rpc-code-polymorphic.md %}


### Asynchronous invocation
The default remote invocation is synchronous, launch asynchronous invocation code example as follows:
{% include_relative rpc-code-asynchronous.md %}


### Oneway invocation
Oneway is a special asynchronous invocation,
which means the client does not expect the server response for this invocation.
Actually server also never response oneway request.
For some specific scene, the oneway invocation which performance is better but not so reliable.
The code example as follows:
{% include_relative rpc-code-oneway.md %}
