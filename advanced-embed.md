---
layout: default
title: Megaplaya Advanced Embed -
---


## Megaplaya Advanced Embed

<script type="text/javascript">
  $(document).ready(
    function() {
      $('#vhx_megaplaya').flash({
        swf: 'http://vhx.tv/embed/megaplaya.swf',
        width: 850,
        allowFullScreen: true,
        allowScriptAccess: "always",
        height: 480
      });
    }
  );

  // Megaplaya calls this function when it's ready
  var megaplaya = false;
  function megaplaya_loaded()
  {
    megaplaya = $('#vhx_megaplaya').children()[0];

    megaplaya_addListeners();

    load_videos();
  }

  function megaplaya_call(method)
  {
    // "pause" => megaplaya.api_pause();
    (megaplaya["api_" + method])();
  }

  function megaplaya_addListeners()
  {
    var events = ['onVideoFinish', 'onVideoLoad', 'onError', 'onPause', 'onPlay', 'onFullscreen', 'onPlaybarShow', 'onPlaybarHide', 'onKeyboardDown'];

    // Loop through and add in call the callback methods. Flash will automatically call megaplaya_callback
    $.each(events, function(index, value) {
      megaplaya.api_addListener(value, "function() { megaplaya_callback('" + value + "', arguments); }")
    });
  }

  function megaplaya_callback(event_name, args)
  {
    var pretty_args = '';

    pretty_args += args[0] || ''
    if (args[1]) pretty_args += ', ' + (args[1] || '')
    if (args[2]) pretty_args += ', ' + (args[2] || '')

    $('#megaplaya_log')[0].innerHTML = event_name + "(" + pretty_args + ")<br />" + $('#megaplaya_log')[0].innerHTML;
  }

  function load_videos()
  {
    $.ajax({
      type: "GET",
      url: "http://vimeo.com/api/v2/vhx/videos.json",
      dataType: "jsonp",
      success: function(videos, status, ajax) {
        if (videos) {
          megaplaya.api_playQueue(videos);
        }
      }
     });
  }
</script>

<div id="vhx_megaplaya">Loading...</div>
<div id="controls">
  <input class="btn" onclick="megaplaya_call('playVideo')" value="Play" />
  <input class="btn" onclick="megaplaya_call('pauseVideo')" value="Pause" />
  <input class="btn" onclick="megaplaya_call('prevVideo')" value="Prev video" />
  <input class="btn" onclick="megaplaya_call('nextVideo')" value="Next video" />
  <input class="btn" onclick="megaplaya_call('disablePlaybar')" value="Disable playbar" />
  <input class="btn" onclick="megaplaya_call('enablePlaybar')" value="Enable playbar" />
  <input class="btn" onclick="megaplaya_call('zoomify')" value="Zoom-in" />
  <input class="btn" onclick="megaplaya_call('unzoomify')" value="Zoom-out" />
</div>
<div class="clear"></div>
Megaplaya log
<div id="megaplaya_log">
  &nbsp;
</div>
<div class="clear">
  &nbsp;
</div>

The code
<div class="gist">
  <script src="https://gist.github.com/1217547.js?file=megaplaya-advanced-embed.html"></script>
</div>
