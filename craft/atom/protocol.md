---
layout    : craft-post
title     : atom-protocol
date      : 2014-10-28
author    : mindwind
permalink : /craft/atom/protocol/
---


## Introduction
A base protocol codec API definition, other component,
like [atom-protocol-textline](/craft/atom/protocol/textline/) implements it.
Protocol codec contains two parts: decoding and encoding.

## Protocol Decode
Protocol decode handles the binary data from network,
decodes bytes into higher-level protocol objects.
Protocol decode API definition as follows:

{% include_relative protocol-code-decoder.md %}

The incoming bytes may be streaming based,
so the protocol decoder implementor should save temporary decoding state.


## Protocol Encode
Protocol encode handles higher-level protocol objects,
transform them to binary data to transport through network.
Protocol encode API definition as follows:

{% include_relative protocol-code-encoder.md %}


## Implementor
A protocol codec implementor usually be used together with [craft-atom-nio](/craft/atom/nio/).
Now we have some implementors as follows:

  * [craft-atom-protocol-rpc](/craft/atom/protocol/rpc/)
  * [craft-atom-protocol-ssl](/craft/atom/protocol/ssl/)
  * [craft-atom-protocol-http](/craft/atom/protocol/http/)
  * [craft-atom-protocol-textline](/craft/atom/protocol/textline/)
