---
layout    : craft-post
title     : Atom
date      : 2014-10-22
author    : mindwind
permalink : /craft/atom/
---

## Usage
Using maven import.

{% include_relative atom-code-maven.md %}


## Capability
A crafted and elegant atomic component library for java.
It is a maven multi-module java projects,
each module is a crafted and atomic java library for specific feature.  
The module list as follows:

  - [atom-io](/craft/atom/io/)  
    `A reactor io api model definition.`
  - [atom-nio](/craft/atom/nio/)  
    `An asynchronous event-driven net io component base on java NIO.`
  - [atom-rpc](/craft/atom/rpc/)  
    `An easy use and efficient java rpc component.`
  - [atom-lock](/craft/atom/lock/)  
    `A kind of distributed lock base on redis.`
  - [atom-util](/craft/atom/util/)  
    `A utility component with some useful tools.`
  - [atom-test](/craft/atom/test/)  
    `A unit test supported component used by other atom component.`
  - [atom-redis](/craft/atom/redis/)  
    `An easy use redis java client base on jedis.`
  - [atom-protocol](/craft/atom/protocol/)  
    `A base protocol codec api definition.`
  - [atom-protocol-rpc](/craft/atom/protocol/rpc/)  
    `A simple custom rpc protocol implementor.`
  - [atom-protocol-ssl](/craft/atom/protocol/ssl/)  
    `A simple ssl protocol implementor.`
  - [atom-protocol-http](/craft/atom/protocol/http/)  
    `A simplified http protocol implementor.`
  - [atom-protocol-textline](/craft/atom/protocol/textline)  
    `A simple textline protocol implementor.`


## Contract
  - All atomic component libraries has same version.  
    `Any one atomic component changes concludes global version upgrade.`
  - Any atomic component library has simple and single feature.  
    `Avoid feature inflation.`

## Version
  - 3.1.0  
    `Add some commands for atom-redis.`
  - 3.0.1  
    `Fix a bug of atom-util.`
  - 3.0.0  
    `Refactor version base on 2.x and rename package name and group id.`
  - <s>2.x</s>
    `Deprecated`
  - <s>1.x</s>
    `Deprecated`
