---
layout    : craft-post-zh
title     : atom-redis
date      : 2014-10-31
author    : mindwind
permalink : /craft/atom/redis/zh/
---


## 简介
基于 [jedis](https://github.com/xetorthio/jedis) 的易于使用的 java redis client


## 它支持与不支持的？

  1. [redis commands](http://redis.io/commands)  
     `目前它完全兼容 redis 2.8.5 所支持的命令。`
  2. [redis transactions](http://redis.io/topics/transactions)  
     `一种简化且优雅的批量执行机制。`
  3. [redis pub/sub](http://redis.io/topics/pubsub)  
     `一种发布/订阅消息模式。`
  4. [redis sharding](https://github.com/mindwind/craft-atom/blob/master/craft-atom-redis/src/main/java/io/craft/atom/redis/api/ShardedRedis.java)  
     `默认的 murmur hash 策略，并且支持自定义 hash 策略扩展。`
  5. [<s>redis pipelining</s>](http://redis.io/topics/pipelining)  
     `做出一个艰难的决定放弃了对 redis pipelinling 的支持。`


## 入门
默认我们支持 4 种 redis 部署架构类型。

### 类型 1 - 单实例 redis
仅有一个单实例 redis 服务，部署架构如图1。
针对此类的典型用法如下：
{% include_relative redis-code-1.md %}
![redis singleton](/images/doc-craft-atom-redis-singleton.png)


### 类型 2 - 分片 redis
分片多实例 redis 服务，部署架构如图2。
针对此类的典型用法如下：
{% include_relative redis-code-2.md %}
![redis sharded](/images/doc-craft-atom-redis-sharded.png)


### 类型 3 - 主从 redis
主从 redis 服务，一主多从，部署架构如图3。
针对此类的典型用法如下：
{% include_relative redis-code-3.md %}
![redis master slave](/images/doc-craft-atom-redis-master-slave.png)


### 类型 4 - 主从分片式 redis
主从分片式 redis 服务，包含多个分片 redis 实例同时每个分片至少有 1 个或多个从节点，部署架构如图4。
针对此类的典型用法如下：
{% include_relative redis-code-4.md %}
![redis master slave sharded](/images/doc-craft-atom-redis-master-slave-sharded.png)
