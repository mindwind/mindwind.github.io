{% highlight java %}
public class NioHttpServerIoHandler extends AbstractIoHandler {

    @Override
    public void channelOpened(Channel<byte[]> channel) {
        ProtocolDecoder<HttpRequest> decoder = HttpCodecFactory.newHttpRequestDecoder();
        channel.setAttribute("decoder", decoder);
    }

    @Override
    public void channelRead(Channel<byte[]> channel, byte[] bytes) {
        ProtocolDecoder<HttpRequest> decoder = (ProtocolDecoder<HttpRequest>) channel.getAttribute("decoder");
        List<HttpRequest> requests = decoder.decode(bytes);

        // After handle the request, we should produce a response and send back to client.
        HttpResponse response = HttpResponses.new200Response("response content");
        ProtocolEncoder<HttpResponse> encoder = HttpCodecFactory.newHttpResponseEncoder(Charset.forName("utf-8"));
        byte[] bytes = encoder.encode(response);
        channel.write(bytes);
    }
}
{% endhighlight%}
