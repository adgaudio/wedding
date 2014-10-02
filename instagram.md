---
layout: default
title: Our instagram photos
site_classes: gray_background
---


Our Instagram Feed!
==============

Please help us document our wedding by sharing your photos on Instagram.


Use our hashtag:  #alexdillemma
---


<div id="instagram_authorize_oauth">
<!--A link to let Instagram authorize this webpage-->
</div>


<div id="instafeed">
<!--Instagram photos inserted here-->
</div>

<pre>
  <script
    type="text/javascript"
    src="{{ site.baseurl }}/js/instafeed.min.js">
  </script>
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
  <script type="text/javascript">
    // Setup vars for an instagram feed using instafeedjs
    {% raw %}
    var data = {
        get: 'tagged',
        tagName: 'alexdillemma',
        resolution: 'low_resolution',
        sortBy: 'most-recent',
        template: '<a href="{{link}}"><div class="instagram_image_holder"><div style="background-image:url({{image}})" class="instagram_image"></div></div></a>'
    };
    {% endraw %}

    // Add client_id
    {% comment %}
    // hack: instagram doesn't let you have multiple referers for oauth,
    // so I've instead created an extra client id for local development
    {% endcomment %}
    {% if site.baseurl != 'local' %}
      var clientId = '{{ site.instagram.client_id }}';
      var redirect_uri = '{{ site.instagram.redirect_uri }}';
    {% else %}
      var clientId = '{{ site.instagram.client_id_localhost }}';
      var redirect_uri = '{{ site.instagram.redirect_uri_localhost }}';
    {% endif %}
    data['clientId'] = clientId;

    // Try to add access token from url or session cache if possible
    // hack: instagram returns access token as a hash, not as a query param
    var accessToken = location.hash.split('=')[1];
    var cache_key = redirect_uri + '--instagram_access_token--';
    if (!accessToken) {
      accessToken = window.sessionStorage.getItem(cache_key);
    }
    if (accessToken) {
      data['accessToken'] = accessToken;
      window.sessionStorage.setItem(cache_key, accessToken);
    }

    // add pagination
    data['after'] = function() {
      var images = $("#instafeed").find('a');
      $.each(images, function(index, image) {
        var delay = (index * 75) + 'ms';
        $(image).css('-webkit-animation-delay', delay);
        $(image).css('-moz-animation-delay', delay);
        $(image).css('-ms-animation-delay', delay);
        $(image).css('-o-animation-delay', delay);
        $(image).css('animation-delay', delay);
        $(image).addClass('animated flipInX');
      });
    // disable button if no more results to load
    // if (!this.hasNext()) {
    //  loadButton.setAttribute('disabled', 'disabled');
    // }
    }
    var lastScrollTop = 0;
    $(window).scroll(function(event){
       var st = $(this).scrollTop();
       if (st > lastScrollTop){
         // downscroll code
         feed.next();
       } else {
         // upscroll code
       }
       lastScrollTop = st;
    });

    // run the instagram feed
    var feed = new Instafeed(data);
    feed.run();

    // hack: add a link enabling people to view photos within their network
    // if instagram let me have multiple referers, I wouldn't do it this way
    var aTag = document.createElement('a');
    aTag.setAttribute('href', 'https://instagram.com/oauth/authorize/?' + 
      'client_id=' + clientId + '&redirect_uri=' + redirect_uri
      + '&response_type=token');
    aTag.innerHTML = "Login to Instagram to see more photos.";
    document.getElementById("instagram_authorize_oauth").appendChild(aTag);

  </script>
</pre>
