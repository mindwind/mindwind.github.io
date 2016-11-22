{% highlight java %}
// Set invocation of this time is oneway
RpcContext ctx = RpcContext.getContext();
ctx.setOneway(true);
String r = ds.echo("hi");

// Return null for oneway invocation.
assert r == null;

// Oneway invocation, no future object return.
Future<String> future = ctx.getFuture();
assert future == null;
{% endhighlight %}
