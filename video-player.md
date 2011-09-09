---
layout: default
title: VHX API docs (beta)
---


<script type="text/javascript">
  $(document).ready(
    function() {
      $('#megaplaya').flash({
        swf: 'http://vhx.tv/embed/megaplaya',
        width: 500,
        allowFullScreen: true,
        allowScriptAccess: "always",
        height: 375
      });
    }
  );

  // Megaplaya calls this function when it's ready
  var megaplaya = false;
  function megaplaya_loaded()
  {
    megaplaya = $('#megaplaya').children()[0];
  }

  function load_videos()
  {
    $.ajax({
      type: "GET",
      url: $('#vimeo_videos_url')[0].value,
      //url: '/js/bumpers.json',
      dataType: "jsonp",
      success: function(videos, status, ajax) {
        if (videos) {
          megaplaya.api_playQueue(videos);
        }
      }
     });
  }

</script>

This VHX's video player we call <b>Megaplaya</b>. Why? You can load in a playlist/queue of YouTube, Vimeo and raw video files using just Javascript. You can even create your own JS controls for Megaplaya.

Below is an example that loads in all our VHX bumpers from [vimeo.com/vhx/videos](http://vimeo.com/vhx/videos)

<div id="megaplaya" markdown="1">
Loading...
</div>

<div style="margin-left: 170px;" markdown="1">
<input id="vimeo_videos_url" type="text" value="http://vimeo.com/api/v2/vhx/videos.json" />
<div class="btn" onclick="load_videos()" style="width: 140px;">Load videos</div>
</div>

### [Javascript API](#javascript-api)

Below is the list of available JS api hooks. Please view the source of this page to see how we setup the above player using jQuery.

    api_isLoaded():Boolean

    api_addEventListener(event_name:String, callback_js_fn:String):void
      onVideoFinish
      onVideoLoad
      onKeyboardDown
      onError
      onPause
      onPlay
      onFullscreen
      onNextVideo
      onPrevVideo

    api_growl():void
    api_warn():void

    api_loadQueue(videos:Array):void
    api_setQueueAt(index:int):void

    api_playQueue(videos:Array):void
    api_playQueueAt(index:int):void

    api_nextVideo():void
    api_prevVideo():void

    api_setVolume(val:Number):void

    api_toggle():void
    api_playVideo():void
    api_pause():void

    api_goFullscreen():void
    api_exitFullscreen():void

    api_seek(position:Number):void

    api_disable():void
    api_enable():void

    api_zoomify():void
    api_unzoomify():void

    api_copyVideoLink():void
    api_visitVideoLink():void
    api_copyEmbedCode():void

    api_getCurPlayingVideo():Object
    api_getCurrentTime():Number
