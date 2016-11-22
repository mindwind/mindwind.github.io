{% highlight java %}
DLock dlock = ...;
if (dlock.tryLock("lockKey", 10, TimeUnit.SECONDS)) {
    try {
        // manipulate protected state
    } finally {
        dlock.unlock();
    }
}
{% endhighlight %}
