---
layout: default
title:
---

## [Greetings, programs]()

<div style="text-align:center;margin-bottom: 40px;">
  <img align="center" src="http://vhx.tv/images/spacecat-cropped.png" alt="Spacecat" title="Spacecat" style="text-align:center;height: 100px; padding-right: 12px; " />
</div>

<div class="col" markdown="1" style="background: #1a1a1a url('http://sht.tl/ynSeC') 10px 135px no-repeat;" onclick="location.href='#general_api_info'">
  <div class="title">1) Videos!</div>
  Read and write videos to VHX accounts, including sharing, liking, queueing and more.
</div>
<div class="col col2" markdown="1" style="background: #1a1a1a url('http://sht.tl/aCSEr') -20px 0 no-repeat;" onclick="location.href='#playlists'">
  <div class="title">2) Make Playlists</div>
  Organize videos into fun, continous-play video mixtapes, with support for animated GIF album art. [Here's an example](http://vhx.tv/casey/music-videos)
</div>
<div class="col col3" markdown="1" style="background: #1a1a1a url('http://sht.tl/9VgT') -120px -40px no-repeat;" onclick="location.href='/video-player.html'">
  <div class="title">3) Embeddable Player</div>
  Just send our player a list of URLs for back-to-back playback experience. [Try it out](/video-player.html)
</div>
<div class="clear">&nbsp;</div>

The VHX API and this documentation are currently in _BETA_. Please contact us [via email](mailto:dev@vhx.tv) or through our [contact form](http://vhx.tv/feedback) with any issues or suggestions. OAuth2 support coming soon.

For some demos of what you can do with the VHX embeddable player and API visit [Music Video Genome](http://musicvideogenome.com) or [showmenonstop.com](http://showmenonstop.com)
<br />

* Join the [VHXdev mailing list](https://groups.google.com/group/vhx-api) for announcements and general discussion.
* Chat with us on IRC: [irc.freenode.net/#vhx](irc://irc.freenode.net/#vhx) -- and say hi to [our bot](http://github.com/jamiew/fatbot)
* Follow us on Twitter: [@vhxtv](http://twitter.com/vhxtv)
* Follow our Tumblr: [blog.vhx.tv](http://blog.vhx.tv)

## [Let's get started]()

### [Demo apps and code samples](#demo_apps_and_code_samples)

* [Complete megaplaya + jQuery demo](https://gist.github.com/1215779) -- use our embedable player to play videos from any site. Read more on the [video-player](/video-player.html) page
* [showmemenonstop.com source code](http://github.com/jamiew/nonstoptv) -- a custom-styled Megaplaya fed by YouTube searches


### [Just show me the 'curl' examples](#just_show_me_the_curl_examples)

Get all the videos shared by [@staff](http://vhx.tv/staff/shared):

    curl http://api.vhx.tv/staff/shared.json


Get queued videos for the current user, authenticating as __@jamiew__ -- api_token is available at [vhx.tv/settings](http://vhx.tv/settings)

    curl http://api.vhx.tv/queue.json?login=jamiew&api_token=[SECRET]


Add <http://vimeo.com/123456> to my queue, authenticating as __@jamiew__:

    curl -d url=http://vimeo.com/123456 \
      -d login=jamiew \
      -d api_token=[SECRET] \
      http://api.vhx.tv/videos/queue.json

Share that same video, with a simple urlencoded comment. First video posted on Vimeo!:

    curl -d url=http://vimeo.com/2 \
        -d login=jamiew \
        -d api_token=SECRET \
        -d comment=First+video+posted+on+Vimeo \
        http://api.vhx.tv/videos/share.json


### [General API info](#general_api_info)

* Response formats: __.json__ and __.xml__
* We follow REST conventions as much as possible, using nested resource URLs and HTTP verbs for actions:
  * __GET__ &rarr; fetch things
  * __POST__ &rarr; create things
  * __PUT__ &rarr; update things
  * __DELETE__ &rarr; remove things

* Common (but optional) parameters:
  * __url__ -- all video-adding methods (share, queue, like, etc.) accept either an __?id__ parameter or a __?url__ paramter. This lets you easily specify a native VHX video ID or an arbitrary YouTube or Vimeo URL
  * __page__ -- currently all paged result sets contain 50 results
  * __callback__ -- optional parameter for JSON requests to wrap the data in a javascript callback (JSONp)
  * __app_id__ -- a simple string identifying your application, e.g. "my_new_vidrecommender"

* Authentication parameters:
  * __login__ -- authenticating user's username (jamiew) or email address (jamie@vhx.tv)
  * __api_token__ -- authenticating user's API token, found on <http://vhx.tv/settings>


### [User Authentication](#user_authentication)

We currently use simple API tokens to authenticate you as a user. Just specify "login" and "api_token" parameters with your requests:

* __login__ -- can be username (jamiew) or email address (jamie@vhx.tv)
* __api_token__ -- can be found on <http://vhx.tv/settings>

We plan to add full OAuth2 support soon.

### [App Registration](#app_registration)

We don't currently require you to formally register your app, but request that you pass an identifying string as an __app_id__ parameter, especially if you are on a shared IP (e.g. a hackathon). In the future this will grant you increased rate limits as well.

    curl http://api.vhx.tv/jamiew.json?app_id=testbed_app


## [API Methods](#api_methods)

### [Video Actions](#video_actions)

_auth required_

      POST http://api.vhx.tv/videos/queue.json?url=http://vimeo.com/2
    DELETE http://api.vhx.tv/videos/unqueue.json?url=http://vimeo.com/2

      POST http://api.vhx.tv/videos/share.json?url=http://vimeo.com/2&comment=Yay+this+is+great
    DELETE http://api.vhx.tv/videos/unshare.json?url=http://vimeo.com/2

      POST http://api.vhx.tv/videos/like.json?url=http://vimeo.com/2
    DELETE http://api.vhx.tv/videos/unlike.json?url=http://vimeo.com/2

### [Your Videos](#your_videos)

_auth required_

    GET http://api.vhx.tv/shared.json
    GET http://api.vhx.tv/queued.json
    GET http://api.vhx.tv/liked.json
    GET http://api.vhx.tv/watched.json

### [Other People's Videos](#other_peoples_videos)

_public_

    GET http://api.vhx.tv/jamiew.json
    GET http://api.vhx.tv/jamiew/shared.json
    GET http://api.vhx.tv/jamiew/queued.json
    GET http://api.vhx.tv/jamiew/liked.json

(everything except 'watched')

### [Playlists](#playlists)

A video mixtape, composed of a list of videos with "position" attributes. Yes, we support [animated GIFs](http://vhx.tv/casey/robocop)!

_public_

    GET http://api.vhx.tv/playlists.json        -- all playlists
    GET http://api.vhx.tv/playlists/:id.json    -- playlist details
    GET http://api.vhx.tv/:user_id/:id.json     -- alternate URL for above (e.g. /jamiew/turntablism.json)

_auth required_

    POST http://api.vhx.tv/playlists.json       -- create playlist. attributes: title, description, image, color (ff00ff)
    PUT http://api.vhx.tv/playlists/:id.json    -- update playlist's attributes

    POST http://api.vhx.tv/playlists/:id/videos                 -- add a video to specified playlist (pass :url or :video_id)
    DELETE http://api.vhx.tv/playlists/:id/videos/:video_id     -- remove video from playlist
    PUT http://api.vhx.tv/playlists/:id/videos/:video_id/move   -- specify :position argument (starts at index 0) to change video's order

    GET http://api.vhx.tv/playlists/mine.json             -- authenticating user's playlists
    GET http://api.vhx.tv/videos/:id/playlists.json       -- playlists said video is present in
    GET http://api.vhx.tv/videos/:id/playlists/mine.json  -- playlists by the authenticating user's that said video is in

### [Basic playlist API example](#basic_playlist_api_example)

The rough order of operations to create a playlist are:

* POST /playlists -- with title and optional description and image
  * store the returned __id__ (e.g. 123) and __slug__ (url chunk parsed from title, e.g. "my-new-playlist")
* POST video URLs to /playlists/123
  * store the returned ids for these as well, since you can use them to delete or move the videos later
* PUT /playlists/123/videos/456789/move -- with a 'position' parameter
* DELETE /playlists/123/videos/456789 -- to remove it completely

The playlist is visible on vhx.tv at http://vhx.tv/{USERNAME}/{PLAYLIST_SLUG}

http://vhx.tv/playlists/123 is still a valid API resource endpoint, and if you visit it on the live site it will 301 Redirect to semantic URL.


### [User info & Followers](#user_info_followers)

_public_

    GET http://api.vhx.tv/jamiew.json
    GET http://api.vhx.tv/jamiew/followers.json
    GET http://api.vhx.tv/jamiew/following.json

_auth required_

      POST http://api.vhx.tv/jamiew/follow.json
    DELETE http://api.vhx.tv/jamiew/unfollow.json


### [Service Streams](#service_streams)

If user has linked their Facebook, Tumblr or Twitter accounts VHX will import videos shared by their friends:

_auth required_

    GET http://api.vhx.tv/facebook.json
    GET http://api.vhx.tv/twitter.json
    GET http://api.vhx.tv/tumblr.json

## [More info](#more_info)

### [Rate Limits](#rate_limits)

Currently we allow up to 86k API requests per IP per day (approximately 1 request per second). We also throttle clients that are making requests too quickly -- more than 2 per second. The "day" windowexpires at midnight UTC each day.

If you've exceeded the rate limits you'll receive a __403 Forbidden__ response code.

Every API response contains the following HTTP headers to let you know about rate limits, following Twitter's API conventions:

- X-RateLimit-Limit -- number of requests allowed during window
- X-RateLimit-Remaining -- number of requests left in the current window
- X-RateLimit-Reset -- unixtime when your request window we be reset

If you are rocking out and need your limits bumped don't hesitate to [contact us](mailto:team@vhx.tv?subject=I+need+more+API+requests).


### [Roadmap](#roadmap)

* OAuth2 support -- we will grandfather in apps using api_tokens
* Client libraries: Ruby, Python, PHP, Javascript (node.js)
* Interactive API demos, hurl-style
* More sample code and example apps! [Send us yours](mailto:team@vhx.tv)


### [Video support](#video_support)

* YouTube
* Vimeo
* Raw video files (h264 or FLV)

We have near-term plans to add support for several other major video hosting sites. [Contact us](mailto:dev@vhx.tv) if you'd like us to support your site. Direct video file access preferred. Flash/AS3-friendly video player APIs also make us happy.


### [Legal](#legal)

By using the VHX API, you agree to the [VHX API Terms of Service](/tos.html).
