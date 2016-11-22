{% highlight java %}
public class NioEchoServerHandler extends AbstractIoHandler {
    @Override
    public void channelRead(Channel<byte[]> channel, byte[] bytes) {
        channel.write(bytes);
    }
  }
{% endhighlight %}
