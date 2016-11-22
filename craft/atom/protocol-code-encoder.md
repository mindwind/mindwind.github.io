{% highlight java %}
public interface ProtocolEncoder<P> {

    /**
     * Encodes higher-level protocol objects into binary data.
     *
     * @param protocolObject
     * @return byte array
     * @throws ProtocolException
     */
    byte[] encode(P protocolObject) throws ProtocolException;
}
{% endhighlight %}
