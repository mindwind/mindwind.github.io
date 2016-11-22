{% highlight java %}
DLock dlock = ...;
// base on redis 2.4+
DLock dlock = DLockFactory.newRedis24DLock(...);

// base on redis 2.6.12+
DLock dlock = DLockFactory.newRedis26DLock(...);
{% endhighlight %}
