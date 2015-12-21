---
layout    : post
title     : 阅读的姿势
date      : 2015-09-24
author    : mindwind
categories: blog
tags      : 技能
image     : /assets/article_images/2015-09-24.jpg
elapse    :
---

## 参考
[1] antirez. [Redis Documentation](http://redis.io/documentation)  
As a rule of thumb, an instance with 30000 connections can only process half the throughput achievable with 100 connections.


[2] antirez. [Clarifications about Redis and Memcached](http://antirez.com/news/94)
Memcached multi threading is still an advantage since it makes things simpler to use and administer, but I think it is not a crucial part.


[3] antirez. [Lazy Redis is better Redis](http://antirez.com/news/93)
Now that values of aggregated data types are fully unshared, and client output buffers don’t contain shared objects as well, there is a lot to exploit. For example it is finally possible to implement threaded I/O in Redis, so that different clients are served by different threads. This means that we’ll have a global lock only when accessing the database, but the clients read/write syscalls and even the parsing of the command the client is sending, can happen in different threads. This is a design similar to memcached, and one I look forward to implement and test.


[4] antirez. [On Redis, Memcached, Speed, Benchmarks and The Toilet](http://oldblog.antirez.com/post/redis-memcached-benchmark.html)
Even if Redis provides much more features than memcached, including persistence, complex data types, replication, and so forth, it's easy to say that it is an almost strict superset of memcached.

This is a limited test that can't be interpreted as Redis is faster. It is surely faster under the tested conditions, but different real world work loads can lead to different results. For sure after this tests I've the impressions that the two systems are well designed, and that in Redis we are not overlooking any obvious optimization.

[5] antirez, [An update on the Memcached/Redis benchmark](http://oldblog.antirez.com/post/update-on-memcached-redis-benchmark.html)
is a multi threaded implementation worth it?
This is a matter of design, tastes, and facts all mixed together. With a single instance using multiple threads you have a few advantages:
  - No sharding if you want to use a single server.
  - If your application performs a lot of GET operations with multiple keys per time, a single instance does not force you to take multiple connections and to send more requests in parallel, that is less straightforward.
There are also disadvantages:
  - Slower development speed to achieve the same features. Multi thread programming is hard.
  - It's harder to fix bugs. The only place of the Redis code base where we experienced hard to fix bugs was the Virtual Memory, that is threaded (because it is the only way to do it well).
  - Not as scalable.
  - If you are dealing with complex atomic operations like Redis does, it can become a nightmare.
  - Once Redis 2.2 will be stable we'll focus on Redis Cluster, that will mitigate the pain of running a cluster of instances. This is a non issue with memcached mostly as it's used for caching and   client-side sharding is perfectly fine for this application.

[6] dormando. [Redis VS Memcached (slightly better bench)](http://dormando.livejournal.com/525147.html)
memcached is multi-threaded and has a very, very high performance ceiling. redis is single-threaded and is very performant on its own.
Computers are absolutely trending toward more cores and not toward higher clocks. Threading is how we will scale single instances.
Understand what your app needs feature-wise, scale-wise, and performance-wise, then use the right tool for the damn job. Don't just read benchmarks and flip around ignorantly, please :)

[7] Mike Perham. [Storing Data with Redis](http://www.mikeperham.com/2015/09/24/storing-data-with-redis/)
At the same time I see lots of people using Redis in the most naive way possible: put everything into one database.

Redis is single-threaded and can perform X ops/sec so consider that your performance “budget”. Datasets in the same Redis instance will share that budget. What happens when your traffic spikes and the cache data uses the entire budget? Now your job queue slows to a crawl.

I recommend memcached because it is designed for caching: it performs no disk I/O at all and is multithreaded so it can scale across all cores, handling 100,000s of requests per second. Redis is limited to a single core so it will hit a scalability limit before memcached. Using Redis for caching is totally reasonable if you want to stick with one tool and are comfortable with the necessary configuration and lower scalability limit per process. Redis does have a nice advantage that it can persist the cache, making it much faster to warm up upon restart.


[8] 温柔一刀. [Redis 常见的性能问题和解决方法](http://zhupan.iteye.com/blog/1576108).
Master写内存快照，save命令调度rdbSave函数，会阻塞主线程的工作，当快照比较大时对性能影响是非常大的，会间断性暂停服务，所以Master最好不要写内存快照。
虽然Redis宣称主从复制无阻塞，但由于磁盘io的限制，如果Master快照文件比较大，那么dump会耗费比较长的时间，这个过程中Master可能无法响应请求，也就是说服务会中断。
