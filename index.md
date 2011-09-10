---
layout: default
title: VHX API docs (beta)
---

<div style="text-align:center;padding-top: 20px;">
  <img align="center" src="http://vhx.tv/images/spacecat-cropped.png" alt="Spacecat" title="Spacecat" style="text-align:center;height: 100px; padding-right: 12px; " />
</div>
<div style="text-align:center;font-size:18px;margin-bottom:40px;">Greetings, programs.</div>

<div class="col" markdown="1">
  <div class="title">Video metadata</div>
  Easily grab any type information about a video
</div>
<div class="col" markdown="1">
  <div class="title">Playlists</div>
  Easily make playlists of videos and watch them in a fun, mixtape-like experience. [Here's an example](http://vhx.tv/casey/music-videos)
</div>
<div class="col" markdown="1">
  <div class="title">Embeddable player</div>
  User our player to play any type of video on the fly in a seamless, back-to-back experience. [Try it out](/video-player.html)
</div>
<div class="clear">&nbsp;</div>

The VHX API and this documentation are currently _Hella Beta_. Please don't hesitate to contact us [via email](mailto:dev@vhx.tv) or through our [contact form](http://vhx.tv/feedback).

Join the [VHXdev mailing list](https://groups.google.com/group/vhx-api) for announcements and general discussion.

### [General API info](#general-api-info)

* Response formats: __.json__ and __.xml__
* JSON requests allow a __callback__ parameter to specify a javascript callback (JSONp)

* Common parameters:
  * __login__ -- authenticating user's username (jamiew) or email address (jamie@vhx.tv)
  * __api_token__ -- authenticating user's API token, found on <http://vhx.tv/settings>
  * __page__ -- currently all paged result sets contain 50 results
  * __url__ -- all video-adding methods (share, queue, like, etc.) accept either an __?id__ parameter or a __?url__ paramter. This lets you easily specify a native VHX video ID or an arbitrary YouTube or Vimeo URL

### [User Authentication](#user-authentication)

We currently use a simple API tokens to authenticate you as a user with plans to add full OAuth2 support. Just specify "login" and "api_token" parameters with your request:

* __login__ -- can be username (jamiew) or email address (jamie@vhx.tv)
* __api_token__ -- can be found on <http://vhx.tv/settings>

### [App Registration](#app-registration)

We don't currently require you to formally register your app, but you are required to specify an identifying __app_id__ parameter with your requests. e.g:

    curl http://api.vhx.tv/jamiew.json?app_id=testbed_app

### [curl samples](#curl-samples)

Get all the videos shared by [@staff](http://vhx.tv/staff/shared):

    curl http://api.vhx.tv/staff/shared.json?app_id=test


Get queued videos for the current user, authenticating as __@jamiew__:

    curl http://api.vhx.tv/queue.json?app_id=test&login=jamiew&api_token=[SECRET]


Add <http://vimeo.com/123456> to my queue, authenticating as __@jamiew__:

    curl -d app_id=test \
      -d url=http://vimeo.com/123456 \
      -d login=jamiew \
      -d api_token=[SECRET] \
      http://api.vhx.tv/videos/queue.json

Share that same video, with a simple urlencoded comment:

    curl -d app_id=test \
        -d url=http://vimeo.com/2 \
        -d login=jamiew \
        -d api_token=SECRET \
        -d comment=First+video+on+vimeo \
        http://api.vhx.tv/videos/share.json


## [API Methods](#api-methods)

### [Video Actions](#video-actions)

_auth required_

      POST http://api.vhx.tv/videos/queue.json?url=http://vimeo.com/2
    DELETE http://api.vhx.tv/videos/unqueue.json?url=http://vimeo.com/2

      POST http://api.vhx.tv/videos/share.json?url=http://vimeo.com/2&comment=Yay+this+is+great
    DELETE http://api.vhx.tv/videos/unshare.json?url=http://vimeo.com/2

      POST http://api.vhx.tv/videos/like.json?url=http://vimeo.com/2
    DELETE http://api.vhx.tv/videos/unlike.json?url=http://vimeo.com/2

### [Your Videos](#your-videos)

_auth required_

    GET http://api.vhx.tv/shared.json
    GET http://api.vhx.tv/queued.json
    GET http://api.vhx.tv/liked.json
    GET http://api.vhx.tv/watched.json

### [Other People's Videos](#other-peoples-videos)

_public_

    GET http://api.vhx.tv/jamiew.json
    GET http://api.vhx.tv/jamiew/shared.json
    GET http://api.vhx.tv/jamiew/queued.json
    GET http://api.vhx.tv/jamiew/liked.json

(everything except 'watched')

### [Playlists](#playlists)

_public_

    GET http://api.vhx.tv/playlists.json          -- all playlists
    GET http://api.vhx.tv/playlists/:id.json      -- playlist details
    GET http://api.vhx.tv/:user_id/:id.json       -- alternate URL for above (e.g. /jamiew/turntablism.json)

_auth required_

    POST http://api.vhx.tv/playlists.json         -- create playlist. attributes: title, description, image, color (#ff00ff)
    PUT http://api.vhx.tv/playlists/:id.json      -- update playlist's attributes

    POST http://api.vhx.tv/playlists/:id/videos   -- add a video to specified playlist (pass :url or :video_id)
    DELETE http://api.vhx.tv/playlists/:id/videos/:video_id   -- remove video from playlist
    PUT http://api.vhx.tv/playlists/:id/videos/:video_id/move -- specify :position argument (starts at index 0) to change video's order

    GET http://api.vhx.tv/playlists/mine.json     -- authenticating user's playlists
    GET http://api.vhx.tv/videos/:id/playlists.json       -- playlists said video is present in
    GET http://api.vhx.tv/videos/:id/playlists/mine.json  -- playlists by the authenticating user's that said video is in


### [User info & Followers](#user-info-followers)

_public_

    GET http://api.vhx.tv/jamiew.json
    GET http://api.vhx.tv/jamiew/followers.json
    GET http://api.vhx.tv/jamiew/following.json

_auth required_

      POST http://api.vhx.tv/jamiew/follow.json
    DELETE http://api.vhx.tv/jamiew/unfollow.json


### [Service Streams](#service-streams)

If user has linked their Facebook, Tumblr or Twitter accounts VHX will import videos shared by their friends:

*auth required

    GET http://api.vhx.tv/facebook.json
    GET http://api.vhx.tv/twitter.json
    GET http://api.vhx.tv/tumblr.json

## [More info](#more-info)

### [Rate Limits](#rate-limits)

Currently we allow 86k API requests per (app + IP address) combo per day, which is 1 request per second. The day window is not rolling, and expires at midnight UTC each day.

If you've exceeded the rate limits you'll receive a __403 Forbidden__ response code.

Every API response contains the following HTTP headers to let you know about rate limits, following Twitter's API conventions:

- X-RateLimit-Limit -- number of requests allowed during window
- X-RateLimit-Remaining -- number of requests left in the current window
- X-RateLimit-Reset -- unixtime when your request window we be reset

If you are rocking out and need your limits bumped don't hesitate to [contact us](mailto:team@vhx.tv?subject=I+need+more+API+requests).


### [CHANGELOG](#changelog)

* __r4__ [2011-09-08]: megaplaya documentation added
* __r3__ [2011-09-07]: documentation updates
* __r2__ [2011-09-01]: added playlists
* __r1__ [2011-07-25]: initial commit


### [Roadmap](#roadmap)

* OAuth2 support (we will grandfather in apps using api_tokens)
* Client libraries -- ruby, python, PHP, javascript (node.js)
* More sample code and example apps! [Send us yours](mailto:team@vhx.tv)


### [Video support](#video-support)

* YouTube
* Vimeo

We have near-term plans to add support for several other video hosting sites as well as raw video files. [Contact us](mailto:dev@vhx.tv) if you'd like us to support your site. Direct video file access preferred. Flash/AS3-friendly video player APIs also make us happy.


### [Legal](#legal)

By using the VHX API, you agree to the [VHX API Terms of Service](/tos.html).
