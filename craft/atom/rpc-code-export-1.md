{% highlight java %}
RpcServer server = RpcFactory.newRpcServer(port);
server.export(DemoService.class, new DemoServiceImpl(), new RpcParameter(10, 100));
server.open();
{% endhighlight %}
