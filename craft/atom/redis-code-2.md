{% highlight java %}
// shardstring format string e.g. localhost:6379,localhost:6380,localhost:6381
ShardedRedis shardedRedis = RedisFactory.newShardedRedisBuilder(shardstring)
                                        .timeoutInMillis(1000)
                                        .build();
{% endhighlight %}
