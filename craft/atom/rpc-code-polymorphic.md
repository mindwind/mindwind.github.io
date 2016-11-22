{% highlight java %}
// Invoke another implementor which rpcId is 'demo2'
DemoService demo = client.refer(DemoService.class);
RpcContext.getContext().setRpcId("demo2");
demo.echo("hi");
{% endhighlight %}
