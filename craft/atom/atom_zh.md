---
layout    : craft-post-zh
title     : Atom 概述
date      : 2014-10-22
author    : mindwind
permalink : /craft/atom/zh/
---

## 用法
使用 maven 引入。

{% include_relative atom-code-maven.md %}


## 能力
一个精心制作的 java 原子组件库。
基于 maven 的多模块项目，每个模块都是精心制作的提供特定功能的 java 原子库。  
模块列表如下：

  - [atom-io](/craft/atom/io/zh)  
    `reactor io 模型 api 定义。`
  - [atom-nio](/craft/atom/nio/zh)  
    `基于 java NIO 的异步事件驱动网络 io 组件。`
  - [atom-rpc](/craft/atom/rpc/zh)  
    `高效且易于使用的 java rpc 组件。`
  - [atom-lock](/craft/atom/lock/zh)  
    `基于 redis 实现的一种分布式锁。`
  - [atom-util](/craft/atom/util/zh)  
    `有用的工具类组件。`
  - [atom-test](/craft/atom/test/zh)  
    `为其他原子组件库的单元测试提供支持的组件。`
  - [atom-redis](/craft/atom/redis/zh)  
    `基于 jedis 的易于使用的 java redis 客户端。`
  - [atom-protocol](/craft/atom/protocol/zh)  
    `基础的协议编解码 api 定义。`
  - [atom-protocol-rpc](/craft/atom/protocol/rpc/zh)  
    `简单自定义的 rpc 协议实现者。`
  - [atom-protocol-ssl](/craft/atom/protocol/ssl/zh)  
    `简单的 ssl 协议实现者。`
  - [atom-protocol-http](/craft/atom/protocol/http/zh)  
    `简化的 http 协议实现者。`
  - [atom-protocol-textline](/craft/atom/protocol/textline/zh)  
    `简单的 textline 协议实现者。`


## 契约
  - 所有原子组件库保持一致版本。  
    `任一原子组件发生改变导致整体版本升级。`
  - 任一原子组件库保持简单单一的功能。  
    `避免功能膨胀。`


## 版本
  - 3.1.0  
  `新增了一些 atom-redis 的命令支持。`
  - 3.0.1  
  `修复了一个 atom-util 的 bug。`
  - 3.0.0  
  `基于 2.x 的重构版本，修改了包名和组 id。`
  - <s>2.x</s>  
  `过期的`
  - <s>1.x</s>  
  `过期的`
