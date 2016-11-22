{% highlight java %}
server.export(DemoService.class,
              "echo",
              new Class<?>[] { String.class },
              new DemoServiceImpl1(),
              new RpcParameter(10, 100));
{% endhighlight %}
