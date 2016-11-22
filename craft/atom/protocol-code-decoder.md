{% highlight java %}
public interface ProtocolDecoder<P> {

    /**
     * Decodes binary data into higher-level protocol objects.
     *
     * @param bytes
     * @return higher-level protocol objects, an empty list returned if these bytes are incomplete for protocol definition.
     * @throws ProtocolException
     */
    List<P> decode(byte[] bytes) throws ProtocolException;

}
{% endhighlight %}
