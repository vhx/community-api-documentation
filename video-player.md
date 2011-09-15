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
    // You can now load videos using megaplaya.api_playQueue()!
  }

  function load_videos()
  {
    $.ajax({
      type: "GET",
      url: $('#vimeo_videos_url')[0].value,
      dataType: "jsonp",
      success: function(videos, status, ajax) {
        // Vimeo's videos contain a "url" property that Megaplaya reads,
        // so we can just pass the response array in directly
        if (videos) {
          megaplaya.api_playQueue(videos);
        }
      }
     });
  }
</script>

Welcome to <b markdown="1">Megaplaya</b>, VHX's video player. You can load in a list of YouTube, Vimeo and raw video URLs using simple Javascript calls. You can even hide the VHX UI and create your own JS controls.

To see demos of Megaplaya in the wild, visit [showmenonstop.com](http://showmenonstop.com), [musicvideogenome.com](http://musicvideogenome.com) or jump to all the [sample code](#sample_code) and [featured apps](/featured-apps.html)

### [Basic video playback](#basic_video_loading)

The below demo loads all the videos from [vimeo.com/vhx](http://vimeo.com/vhx) into a Megaplaya when you click "Load videos.".

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
      swf: 'http://vhx.tv/swf/megaplaya.swf',
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

### [Sample code](#sample_code)

* [Simple megaplaya + jQuery demo app](https://gist.github.com/1215779)

* [Advanced megaplaya demo](/advanced-embed.html)

* [NONSTOPTV app](http://github.com/jamiew/nonstoptv) -- a styled Megaplaya fed by YouTube searches


### [Three ways to embed Megaplaya](#flavors)

We have many variations of embedding Megaplaya that should suit all your needs.

<table>
  <tr>
    <td class="head">Simple</td>
    <td>Specify a list of video URLs</td>
    <td class="code">http://vhx.tv/embed/megaplaya?videos=http://vimeo.com/25584378,http://www.youtube.com/watch?v=btV6M2xDe38</td>
  </tr>
  <tr>
    <td class="head">Smart</td>
    <td>Specify a VHX URL to pull videos from. The embed will always play the latest videos in that channel, e.g. <b>/casey/shared</b></td>
    <td class="code">http://vhx.tv/embed/megaplaya?url=/casey/shared</td>
  </tr>
  <tr>
    <td class="head">Advanced</td>
    <td>Total control via Javascript allowing player customization and response to events.<br /><a href="/advanced-embed.html">Click here to see a demo</a></td>
    <td class="code" style="background: url('http://sht.tl/i2OD') 0 -38px;">&nbsp;</td>
  </tr>
</table>


### [Player setup using jQuery](#setup)

For this demo we're using jQuery, but you can use whatever you want. [jquery.swfobject](http://jquery.thewikies.com/swfobject/) is used for injecting the SWF in a cross-browser, standards-compliant manner, but you could use vanilla [SWFObject](http://code.google.com/p/swfobject/) as well.

    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.6.3/jquery.min.js"></script>
    <script type="text/javascript" src="http://vhx.tv/javascripts/jquery.swfobject-1.1.1.js"></script>

This is the Megaplaya SWF URL:

    http://vhx.tv/swf/megaplaya.swf

Now add a #vhx_megaplaya div and inject the SWF into your page. This example uses [jquery-swfobject](http://jquery.thewikies.com/swfobject/) to inject the SWF:

    <div id="vhx_megaplaya"></div>

    <script>
      $(document).ready(function(){
        $('#vhx_megaplaya').flash({
          swf: 'http://vhx.tv/swf/megaplaya.swf',
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
      // You should save the reference to the newly created SWF object
      megaplaya = $('#vhx_megaplaya').children()[0];
      // Now you can call megaplaya.api_playQueue()!
    }

### [Let's play some videos](#example)

Simply pass an array of objects that each contain a video url to megaplaya, either inside of megaplay_loaded() or after it's fully loaded.

    megaplaya.api_playQueue([
      { url: 'http://www.youtube.com/watch?v=sgKUS9AFemc' },
      { url: 'http://vimeo.com/25584378' },
      { url: 'http://www.flashcomguru.com/flash/bbc_reel_m420pVP6_768K.flv' }
    ])

Each object must contain at least the URL of the video. You may pass as many properties as you'd like though, and these will be accessible when you grab the current video from Megaplaya. If you pass an id, megaplaya will pass it to all the event listeners.

    megaplaya.api_playQueue([
      {
        id: 9,
        url: 'http://vimeo.com/25584378',
        hello: "world",
        thumbnail_url: "url of image here",
        title: "Title of video"
      }
    ])

PROTIP: a lot of video websites (e.g. Vimeo) contain a "url" field in their JSONp responses, meaning you can just pass them straight into megaplaya without any parsing.

### [Event listeners](#listen)

You can hook into megaplaya to know when events occur. In this example, whenever a new video is loaded a javascript alert will popup.

    function video_load(id, url)
    {
      alert(url + ' is now playing!');
    }

    megaplaya.api_addListener("onVideoLoad", "video_load");

### [Javascript API](#javascript-api)

Below is the list of available JS api hooks. Please don't hesitate to [contact us](mailto:dev@vhx.tv) with bug reports and feedback!

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
    megaplaya.api_playQueue(myvideos);

    // Load in a new set of videos. Note that megaplaya will continue playing the current video
    megaplaya.api_updateQueue(mynewvideos);

    // Just make sure when you update the queue, you update the position of the current playing video
    // so megaplaya knows where it should be playing in the queue
    megaplaya.api_setQueueAt(index);

__What video services do you support?__

* Vimeo
* YouTube
* Raw video files (h264 and FLV)

Support for more sites coming soon.
