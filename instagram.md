---
layout: default
title: Our instagram photos
site_classes: gray_background
---


Please help us document our wedding by sharing your photos on Instagram!
-----


Use our hashtag!  #alexdillemma
------

<pre>
  <div id="instafeed"></div>
  <!--Javascript-->
  <script
    type="text/javascript"
    src="{{ site.baseurl }}/js/instafeed.min.js">
  </script>

  {% raw %}
    <script type="text/javascript">
        var feed = new Instafeed({
            get: 'tagged',
            tagName: 'alexdillemma',
            clientId: 'd5a94fab34bd49cba895b1bc1b26a854',
            resolution: 'low_resolution',
            sortBy: 'most-recent',
            template: '<a href="{{link}}"><div class="instagram_image_holder"><div style="background-image:url({{image}})" class="instagram_image" /></div></a>'
        });
        feed.run();
    </script>
  {% endraw %}
</pre>
