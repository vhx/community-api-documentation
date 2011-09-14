---
layout: default
title: Megaplaya -
---

## Megaplaya

<script type="text/javascript">
  $(document).ready(function(){
    $('#vhx_megaplaya').flash({
      swf: 'http://vhx.tv/embed/megaplaya',
      width: 450,
      allowFullScreen: true,
      allowScriptAccess: "always",
      height: 337
    });
  });

  // Megaplaya calls this function when it's ready
  var megaplaya = false;
  function megaplaya_loaded()
  {
    megaplaya = $('#vhx_megaplaya').children()[0];
  }

  function load_videos()
  {
    $.ajax({
      type: "GET",
      url: $('#vimeo_videos_url')[0].value,
      dataType: "jsonp",
      success: function(videos, status, ajax) {
        if (videos) {
          megaplaya.api_playQueue(videos);
        }
      }
     });
  }
</script>

Welcome to <b markdown="1">Megaplaya</b>, VHX's video player. You can load in a list of YouTube, Vimeo and raw video URLs using just Javascript. You can even create your own JS controls for Megaplaya.

The below example loads all our VHX fan-made bumper videos from [vimeo.com/vhx/videos](http://vimeo.com/vhx/videos) when you click "Load videos.".

<div id="player_demo">
  <div markdown="1">
    <input id="vimeo_videos_url" class="field" type="text" value="http://vimeo.com/api/v2/vhx/videos.json" style="width:280px;" />
    <input type="button" class="btn" onclick="load_videos()" style="width: 140px;" value="Load videos" />
  </div>
  <div id="vhx_megaplaya">Loading...</div>
</div>

<div id="player_demo_code">
<pre>
$(document).ready(function(){
    $('#vhx_megaplaya').flash({
      swf: 'http://vhx.tv/embed/megaplaya',
      width: 450,
      allowFullScreen: true,
      allowScriptAccess: "always",
      height: 337
    });
  }
);

// Megaplaya calls this function when it's ready
var megaplaya = false;
function megaplaya_loaded() {
  megaplaya = $('#vhx_megaplaya').children()[0];
}

// Load videos from Vimeo using JSONp callback
function load_videos() {
  $.ajax({
    type: "GET",
    url: $('#vimeo_videos_url')[0].value,
    dataType: "jsonp",
    success: function(videos, status, ajax) {
      if (videos) {
        megaplaya.api_playQueue(videos);
      }
    }
  });
}
</pre>
</div>

<div class="clear"><br /></div>

### [Player setup using jQuery](#setup)

For this demo, we're using jQuery, but you can use whatever you want. jquery.swfobject is used for .flash() but you could inject it manually as well

    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.6.3/jquery.min.js"></script>
    <script type="text/javascript" src="http://vhx.tv/javascripts/jquery.swfobject-1.1.1.js"></script>

This is the Megaplaya SWF URL:

    http://vhx.tv/embed/megaplaya

Now add a #vhx_megaplaya div and inject the SWF into your page.

    <div id="vhx_megaplaya"></div>

    <script>
      $(document).ready(function(){
        $('#vhx_megaplaya').flash({
          swf: 'http://vhx.tv/embed/megaplaya',
          width: 500,
          height: 375
          allowFullScreen: true,
          allowScriptAccess: "always",
        });
      });
    </script>

### [Megaplaya loaded](#megaplaya-loaded)

Megaplaya automatically calls **megaplaya_loaded()** when it's ready. You may also call **api_isLoaded()** to check if the player has loaded already. You must wait until the player is loaded before you can make any API calls.

    var megaplaya = false;
    function megaplaya_loaded()
    {
      // You need save the reference to the newly created SWF object
      megaplaya = $('#vhx_megaplaya').children()[0];
    }

### [Let's play some videos](#example)

Simply pass an array of objects that each contain a video url to megaplaya, either inside of megaplay_loaded() or after it's fully loaded.

    megaplaya.api_playQueue([
      { url: 'http://www.youtube.com/watch?v=sgKUS9AFemc' },
      { url: 'http://vimeo.com/25584378' },
      { url: 'http://www.flashcomguru.com/flash/bbc_reel_m420pVP6_768K.flv' }
    ])

Each object must contain at least the URL of the video. You may pass as many properties as you'd like though. If you pass an id, megaplaya will pass it to all the event listeners.

    megaplaya.api_playQueue([
      {
        id: 9,
        url: 'http://vimeo.com/25584378',
        hello: "world",
        thumbnail_url: "url of image here",
        title: "Title of video"
      }
    ])

### [Event listeners](#listen)

You can hook into megaplaya to know when events occur. In this example, whenever a new video is loaded a javascript alert will popup.

    function video_load(id, url)
    {
      alert(url + ' is now playing!');
    }

    megaplaya.api_addListener("onVideoLoad", "video_load");

### [Javascript API](#javascript-api)

Below is the list of available JS api hooks. Please don't hesitate to send us bug reports and feedback!

    // call this to check if player is loaded. Megaplaya also calls megaplaya_loaded()
    api_isLoaded():Boolean

    api_addListener(event_name:String, callback_js_fn:String):void
      onVideoFinish
      onVideoLoad
      onKeyboardDown // Use this to implement your own keyboard controls
      onError
      onPause
      onPlay
      onFullscreen
      onPlaybarShow   // triggers when the VHX playbar shows itself
      onPlaybarHide   // triggers when the VHX playbar hides itself

    api_growl():void  // A simple growl message that appears at the top of the player
    api_warn():void   // same as above, but in red

    api_loadQueue(videos:Array):void // Clears out current queue, and loads the next. Doesn't play.
    api_setQueueAt(index:int):void   // Updates the player's reference point in the queue. Doesn't play.

    api_playQueue(videos:Array):void // Clears out current queue, and loads the next. Autoplays.
    api_playQueueAt(index:int):void  // Updates the player's reference point in the queue. Autoplays.

    api_nextVideo():void
    api_prevVideo():void

    api_setVolume(val:Number):void // Value between 0 and 1

    api_toggle():void     // toggle pause/play
    api_playVideo():void  // play the video
    api_pause():void      // pause the video

    // Not gauranteed to work in all browsers. Usually requires SWF object focus first
    api_goFullscreen():void
    api_exitFullscreen():void

    api_seek(position:Number):void    // Value between 0 and 1

    api_disable():void  // hides VHX's player controls
    api_enable():void   // shows VHX's player controls

    api_zoomify():void    // FX: Zooms into the video. Same as VHX.tv splash page background
    api_unzoomify():void  // FX: Zooms back out

    api_copyVideoLink():void
    api_visitVideoLink():void
    api_copyEmbedCode():void  // Requres embed paramter to be passed along with video.

    api_getCurPlayingVideo():Object  // Returns current playing video boject
    api_getCurrentTime():Number      // Current time in seconds

## [FAQ](#faq)

__Can I update the queue without restarting it?__

Yes you can. We have no built-in methods for this, but you can simply do something like this:

    // load some videos in
    player.api_playQueue(myvideos);

    // Load in a new set of videos. Note that megaplaya will continue playing the current video
    player.api_updateQueue(mynewvideos);

    // Just make sure when you update the queue, you update the position of the current playing video
    // so megaplaya knows where it should be playing in the queue
    player.api_setQueueAt(index);

__What video services do you support?__

* Vimeo
* YouTube
* Video files

Support for TED, Blip.tv and DailyMotion coming soon!
