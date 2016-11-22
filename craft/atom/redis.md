---
layout    : craft-post
title     : atom-redis
date      : 2014-10-31
author    : mindwind
permalink : /craft/atom/redis/
---


## Introduction
A easy use redis java client base on [jedis](https://github.com/xetorthio/jedis).


## What It Provides and Not Provides?

  1. [redis commands](http://redis.io/commands)  
     `Now it is fully compatible with all commands supported by redis 2.8.5.`
  2. [redis transactions](http://redis.io/topics/transactions)  
     `A simple and elegant batch execution mechanism.`
  3. [redis pub/sub](http://redis.io/topics/pubsub)  
     `A simple publish/subscribe messaging paradigm.`
  4. [redis sharding](https://github.com/mindwind/craft-atom/blob/master/craft-atom-redis/src/main/java/io/craft/atom/redis/api/ShardedRedis.java)  
     `Default murmur hash strategy, and support extended hash strategy implementor.`
  5. [<s>redis pipelining</s>](http://redis.io/topics/pipelining)  
     `Make a very difficult decision to give up redis pipelinling.`


## Getting Started
By default we support four type redis deployment architect.

### Type 1 - singleton redis
Just a singleton redis server instance, figure 1 is the deployment architect diagram.
A typical usage idiom for this type as follows:
{% include_relative redis-code-1.md %}
![redis singleton](/images/doc-craft-atom-redis-singleton.png)


### Type 2 - sharded redis
Sharded multiple redis server instances, figure 2 is the deployment architect diagram.
A typical usage idiom for this type as follows:
{% include_relative redis-code-2.md %}
![redis sharded](/images/doc-craft-atom-redis-sharded.png)


### Type 3 - master slave redis
Master slave redis server instances, include one master and multiple slaves,
figure 3 is the deployment architect diagram.
A typical usage idiom for this type as follows:
{% include_relative redis-code-3.md %}
![redis master slave](/images/doc-craft-atom-redis-master-slave.png)


### Type 4 - master slave sharded redis
Master slave sharded redis server instances, include multiple sharded redis instance
and each shard has one or more slaves, figure 4 is the deployment architect diagram.
A typical usage idiom for this type as follows:
{% include_relative redis-code-4.md %}
![redis master slave sharded](/images/doc-craft-atom-redis-master-slave-sharded.png)
