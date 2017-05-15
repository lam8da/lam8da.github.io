<section class="comment">

<div id="disqus_thread"></div>
<script type="text/javascript">
  /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
  var disqus_shortname = 'lambda'; // required: replace example with your forum shortname
  var disqus_url = '{{ site.url }}{{ page.url | remove:'index.html' }}';
  /* * * DON'T EDIT BELOW THIS LINE * * */
  (function() {
      var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
      dsq.src = 'http://' + disqus_shortname + '.disqus.com/embed.js';
      (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
  })();
</script>
<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

<div id="gitment_thread"></div>
<link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
<script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
<script>
  var gitment = new Gitment({
    id: 'location.href', // 可选。默认为 location.href
    owner: 'lam8da',
    repo: 'lam8da.github.io.comments',
    oauth: {
      client_id: '4ec8d7487458f49ba8f2',
      client_secret: 'dbd58db32dee8f9a954f35b6f1ecf931a0a4edfc',
    },
  })
  gitment.render('gitment_thread')
</script>

</section>
