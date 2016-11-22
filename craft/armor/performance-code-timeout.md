{% highlight java %}
for (int i = 0; i < iterations; i++) {
  try {
    sum += is.normal(i);
    is.timeout(timeout);
  } catch (Exception e) {}
}
{% endhighlight %}
