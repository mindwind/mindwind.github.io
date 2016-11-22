{% highlight java %}
// masterslaveshardstring format string
// e.g. localhost:6379-localhost:6380,localhost:6389-localhost:6390
MasterSlaveShardedRedis masterSlaveShardedRedis
    = RedisFactory.newMasterSlaveShardedRedisBuilder(masterslaveshardstring)
                  .timeoutInMillis(1000)
                  .build();
{% endhighlight %}
