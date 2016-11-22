{% highlight java %}
// Set 'rpcId' to bind a specific interface implementor
String rpcId = "demo2";
server.export(rpcId, DemoService.class, new DemoServiceImpl2(), new RpcParameter());
{% endhighlight %}
