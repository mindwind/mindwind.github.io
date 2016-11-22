{% highlight java %}
Redis redis = RedisFactory.newRedisBuilder(host, port)
                          .timeoutInMillis(1000)
                          .build();
{% endhighlight %}
