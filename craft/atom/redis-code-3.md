{% highlight java %}

// masterslavestring format string
// e.g. localhost:6379-localhost:6380-localhost:6381 the first is master, others are slaves.
MasterSlaveRedis masterSlaveRedis = RedisFactory.newMasterSlaveRedisBuilder(masterslavestring)
                                                .timeoutInMillis(1000)
                                                .build();
{% endhighlight %}
