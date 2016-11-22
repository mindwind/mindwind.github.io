---
layout    : craft-post-zh
title     : craft-atom-lock
date      : 2014-10-31
author    : mindwind
permalink : /craft/atom/lock/zh/
---


## 简介
基于 [redis](http://redis.io) 实现的一种分布式锁。


## 入门
该组件的一种典型使用场景如下：
{% include_relative lock-code-1.md %}

如何获得一个 `DLock` 实例
{% include_relative lock-code-2.md %}
