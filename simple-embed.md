---
layout: default
title: Simple Megaplaya Embed -
---

## Simple Megaplaya Embed

This is an embed using no javascript. Scroll down to look at the code.

<object width="850" height="480"><param name="movie" value="http://vhx.tv/embed/megaplaya?videos=http://vimeo.com/25584378,http://www.youtube.com/watch?v=btV6M2xDe38"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://vhx.tv/embed/megaplaya?videos=http://vimeo.com/25584378,http://www.youtube.com/watch?v=btV6M2xDe38" type="application/x-shockwave-flash" width="850" height="480" allowscriptaccess="always" allowfullscreen="true"></embed></object>

This player adds comma-separated list of video URLs to the player.

    http://vhx.tv/embed/megaplaya?videos=http://vimeo.com/25584378,http://www.youtube.com/watch?v=btV6M2xDe38

And here's the actual HTML we used. When you update the code, be sure to update the url in _both_ places.

<script>
  $(document).ready(function() {
    $('#embed_code')[0].innerHTML = "<object width='640' height='480'><param name='movie' value='http://vhx.tv/embed/megaplaya?videos=http://vimeo.com/25584378,http://www.youtube.com/watch?v=btV6M2xDe38'></param><param name='allowFullScreen' value='true'></param><param name='allowscriptaccess' value='always'></param><embed src='http://vhx.tv/embed/megaplaya?videos=http://vimeo.com/25584378,http://www.youtube.com/watch?v=btV6M2xDe38' type='application/x-shockwave-flash' width='640' height='480' allowscriptaccess='always' allowfullscreen='true'></embed></object>".replace(/'/g, '"');
  })
</script>
<textarea class="field" id="embed_code" onclick="this.select()">

</textarea>