{% highlight java %}
public class RpcServerIoHandler extends AbstractIoHandler {

    @Override
    public void channelOpened(Channel<byte[]> channel) {
        ProtocolDecoder<RpcMessage> decoder = RpcCodecFactory.newRpcDecoder();
        channel.setAttribute("decoder", decoder);
    }

    @Override
    public void channelRead(Channel<byte[]> channel, byte[] bytes) {
        ProtocolEncoder<RpcMessage> decoder = (ProtocolEncoder<RpcMessage>) channel.getAttribute("decoder");
        List<RpcMessage> msgs = decoder.decode(bytes);

        // After handle the request, we should produce a response and send back to client.
        RpcMessage rsp = ...;
        ProtocolEncoder<RpcMessage> encoder = RpcCodecFactory.newRpcEncoder();
        byte[] bytes = encoder.encode(rsp);
        channel.write(bytes);
    }
}
{% endhighlight%}
