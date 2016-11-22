{% highlight java %}
// Set invocation of this time is asynchronous
RpcContext ctx = RpcContext.getContext();
ctx.setAsync(true);
String r = ds.echo("hi");

// Return null for asynchronous invocation.
assert r == null;

// Do something other ... and get the invocation result.
Future<String> future = ctx.getFuture();
r = future.get(2, TimeUnit.SECONDS);
assert r != null;
{% endhighlight %}
