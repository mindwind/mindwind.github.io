{% highlight java %}
public class NioSslServerIoHandler extends AbstractIoHandler {

    @Override
    public void channelOpened(Channel<byte[]> channel) {
        SslCodec codec = new SslCodec(sslContext, new NioSslHandshakeHandler(channel));
        codec.init();
        channel.setAttribute("ssl_codec", codec);
    }

    @Override
    public void channelRead(Channel<byte[]> channel, byte[] bytes) {
        SslCodec codec = (SslCodec) channel.getAttribute("ssl_codec");
        byte[] decryptData = codec.decode(bytes);
        if (decryptData != null) {
            // here, ssl handshake complete and we got data after decrypt.
            // do something, then write back some encrypt data
            byte[] encryptData = sslCodec.encode("some data...".getBytes());
            channel.write(encryptData);
        }
    }
}
{% endhighlight%}
