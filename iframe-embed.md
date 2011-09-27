---
layout: default
title: Universal Embed -
---

## Universal Embed

Use this iframe embed and it will work on almost all platforms. (Currently all browsers, iPhone, iPad, iPod and Android). Please note that there is no support for Javascript control over an iframe, yet.

Iframes use the below URL:

    http://vhx.tv/embed/megaplaya.html

<div id="inject">


</div>


This player pulls videos from Staff's shared videos.

    http://vhx.tv/embed/megaplaya.html?url=/staff/shared

And here's the HTML below for easy copy and pasting.

<script type="text/javascript">
  $(document).ready(function() {
    var iframe = "<iframe width='850' height='480' src='http://vhx.tv/embed/megaplaya.html?url=/staff/shared' frameborder='0' allowfullscreen='1'></iframe>".replace(/'/g, '"').replace(/="1"/g, '');
    $('#embed_code')[0].innerHTML = iframe;
    $('#inject')[0].innerHTML = iframe;
  })
</script>
<textarea class="field" id="embed_code" onclick="this.select()">

</textarea>