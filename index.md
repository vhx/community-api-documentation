---
layout: default
title: VHX API docs (beta)
---

Greetings, programs.
<img src="http://vhx.tv/images/spacecat-cropped.png" alt="Spacecat" title="Spacecat" style="float: left; height: 100px; padding-right: 12px; margin-top: -20px;" />
<br style="clear: both;" />

## [Getting Started](#getting-started)

The VHX API and this documentation are currently _Hella Beta_ and actively being improved because of your feedback. Please don't hesitate to contact us [via email](mailto:dev@vhx.tv) or through our [contact form](http://vhx.tv/feedback).

Join the [VHXdev mailing list](https://groups.google.com/group/vhx-api) for announcements and general discussion.


### [General API info](#general-api-info)

* Response formats: __.json__ and __.xml__
* JSON requests allow a __callback__ parameter to specify a javascript callback (JSONp)

* Common parameters:
  *
  * __email__ -- authenticating user's email (jamie@vhx.tv) or username (jamiew)
  * __api_token__ -- authenticating user's API token
  * __page__ -- currently all paged result sets contain 50 results
  * __url__ -- all video-adding methods (share, queue, like, etc.) accept either an __?id__ parameter or a __?url__ paramter. This lets you easily specify a native VHX video ID or an arbitrary YouTube or Vimeo URL

### [User Authentication](#user-authentication)

We currently use simple API token auth. Specify "email" and "api_token" parameters with your request:

* __email__ -- can be the user's email address (jamie@vhx.tv) or username (jamiew)
* __api_token__ -- can be found on <http://vhx.tv/settings>

### [App Registration](#app-registration)

We don't currently require you to formally register your app, but you are required to specify an identifying __app_id__ parameter with your requests

    curl http://api.vhx.tv/jamiew.json?app_id=testbed_app

A sample VHX API call to queue <http://vimeo.com/123456>, authenticating as __@jamiew__:

    curl -d app_id=test \
      -d url=http://vimeo.com/123456 \
      -d email=jamiew \
      -d api_token=123xyz \
      http://vhx.tv/videos/queue.json



## [API Methods](#api-methods)

### [Video Actions](#video-actions)

*auth required

      POST http://vhx.tv/videos/queue.json?url=http://vimeo.com/2
    DELETE http://vhx.tv/videos/unqueue.json?url=http://vimeo.com/2

      POST http://vhx.tv/videos/share.json?url=http://vimeo.com/2&comment=Yay+this+is+great
    DELETE http://vhx.tv/videos/unshare.json?url=http://vimeo.com/2

      POST http://vhx.tv/videos/like.json?url=http://vimeo.com/2
    DELETE http://vhx.tv/videos/unlike.json?url=http://vimeo.com/2

### [Your Videos](#your-videos)

*auth required

    GET http://vhx.tv/shared.json
    GET http://vhx.tv/queued.json
    GET http://vhx.tv/liked.json
    GET http://vhx.tv/watched.json

### [Other People's Videos (public)](#other-peoples-videos)

    GET http://vhx.tv/jamiew.json
    GET http://vhx.tv/jamiew/shared.json
    GET http://vhx.tv/jamiew/queued.json
    GET http://vhx.tv/jamiew/liked.json

(everything except 'watched')

### [Playlists](#playlists)

    GET http://vhx.tv/jamiew/awesome-music-videos.json

### [User info & Followers](#user-info-followers)

    Public:

    GET http://vhx.tv/jamiew.json
    GET http://vhx.tv/jamiew/followers.json
    GET http://vhx.tv/jamiew/following.json

    Auth required:

      POST http://vhx.tv/jamiew/follow.json
    DELETE http://vhx.tv/jamiew/unfollow.json


### [Service Streams](#service-streams)

If user has linked their Facebook, Tumblr or Twitter accounts VHX will import videos shared by their friends:

*auth required

    GET http://vhx.tv/facebook.json
    GET http://vhx.tv/twitter.json
    GET http://vhx.tv/tumblr.json

## [More info](#more-info)

### [Rate Limits](#rate-limits)

Currently we allow 86k API requests per (app + IP address) combo per day (the "request window"), which is 1 request per second. This is _not_ a rolling window and expires at midnight UTC each day.

If you've exceeded the rate limits you'll receive a __403 Forbidden__ response code.

Every API response contains the following HTTP headers to let you know about rate limits, following Twitter's API conventions:

- X-RateLimit-Limit -- number of requests allowed during window
- X-RateLimit-Remaining -- number of requests left in the current window
- X-RateLimit-Reset -- unixtime when your request window we be reset

If you are rocking out and need your limits bumped don't hesitate to [contact us](mailto:team@vhx.tv?subject=I+need+more+API+requests).

### [CHANGELOG](#changelog)

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

We have near-term plans to add support for several other video hosting sites as well as raw video files. [Contact us](mailto:dev@vhx.tv) if you'd like us to support your site! Flash/AS3-friendly video player APIs preferred.


### [Legal](#legal)

By using the VHX API, you agree to the [VHX API Terms of Service](/tos.html).