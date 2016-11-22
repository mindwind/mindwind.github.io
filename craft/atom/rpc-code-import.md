{% highlight java %}
client = RpcFactory.newRpcClient(host, port);
client.open();
DemoService demo = client.refer(DemoService.class);
demo.echo("hi");
{% endhighlight %}
