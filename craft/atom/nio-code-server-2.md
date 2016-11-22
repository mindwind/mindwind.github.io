{% highlight java %}
public class NioEchoServer {

    private int port;

    public NioEchoServer(int port) {
        this.port = port;
    }

    public void start() throws IOException {
        IoAcceptor acceptor = NioFactory.newTcpAcceptorBuilder(new NioEchoServerHandler()).build();
        acceptor.bind(port);
    }

    public static void main(String[] args) throws IOException {
        new NioEchoServer(1314).start();
    }
}
{% endhighlight %}
