{% highlight java %}
public class NioSslHandshakeHandler implements SslHandshakeHandler {

    private Channel<byte[]> channel;

    public NioSslHandshakeHandler(Channel<byte[]> channel) {
        this.channel = channel;
    }

    @Override
    public void needWrite(byte[] bytes) {
        channel.write(bytes);
    }
}
{% endhighlight%}
