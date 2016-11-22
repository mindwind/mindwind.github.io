{% highlight java %}
public class NioTextlineServerIoHandler extends AbstractIoHandler {

    @Override
    public void channelOpened(Channel<byte[]> channel) {
        ProtocolDecoder<String> decoder = TextLineCodecFactory.newTextLineDecoderBuilder(UTF_8, "\n").build();
        channel.setAttribute("decoder", decoder);
    }

    @Override
    public void channelRead(Channel<byte[]> channel, byte[] bytes) {
        ProtocolDecoder<String> decoder = (ProtocolDecoder<String>) channel.getAttribute("decoder");
        List<String> lines = decoder.decode(bytes);
        // do something others
    }
}
{% endhighlight%}
