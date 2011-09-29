---
layout: default
title: OEmbed endpoint -
---

## [VHX OEmbed endpoint](/oembed.html)

VHX provides a [OEmbed](http://oembed.com/) support to let you easily fetch video or playlist information, e.g.:

[http://vhx.tv/services/oembed.json?url=http://vhx.tv/jamiew/shared/394462](http://vhx.tv/services/oembed.json?url=http://vhx.tv/jamiew/shared/394462)

### [Overview](#overview)

    URL scheme: http://vhx.tv/*
    Endpoint: http://vhx.tv/services/oembed.{format}

Pass a ?url=... param with either a full @http://vhx.tv/jamiew@ URI or just @/jamiew/@ path

{format} can be either xml or json

All responses contain an embeddable player in the @html@ field as well as typical metadata, e.g. @thumbnail_url@, @title@, @description@, @author_name, @author_url et al

Our OEmbed endpoint will intelligently determine if the passed ?url param is a video, playlist, or something invalid (e.g. /about). If the url param is invalid (like "/about") we will return a 404 status code and the "success" field will be false, like so:

[http://vhx.tv/oembed.json?url=/about](http://vhx.tv/oembed.json?url=/about)

    {
      "errors": "Record not found",
      "success": false
    }

Per the oembed spec the **type** field will always be "video". If you want to tell a video from playlist you can check for "video_id" vs "playlist_id" field.

### [Embed options](#embed_options)

You can specify a few parameters to customize the returned embed:

* **autoplay:** 1 or 0
* **width:** in pixels
* **height:** in pixels


### [JSONp callbacks](#jsonp_callbacks)

You can specify a ?callback param to wrap the JSON data in a javascript callback for easy injecting. If a ?callback param is specified the OEmbed endpoint will
always return a 200 OK status code (so your code actually fires), and you should determine if it was a valid object or not by checking for @success=true@


### [Sample response: video](#sample_response_video)

[http://vhx.tv/oembed.json?url=/123456](http://vhx.tv/oembed.json?url=/123456)

    {
      "type":"video",
      "html":"<iframe width=\"320\" height=\"240\" src=\"http://www.youtube.com/embed/W2ZgPcNS550\" frameborder=\"0\" allowfullscreen></iframe>",
      "title":"Monsters,300",
      "thumbnail_url":"http://img.youtube.com/vi/W2ZgPcNS550/0.jpg",
      "video_id":123456,
      "original_url":"http://www.youtube.com/watch?v=W2ZgPcNS550",
      "author_name":"SidelineFilms",
      "author_url":"http://www.youtube.com/user/SidelineFilms",
      "height":320,
      "width":320,
      "duration":152,
      "description":"Dave builds the 300 trailer with clips from Monsters,Inc.\n\nFACEBOOK GROUP FOR SIDELINE FILMS-\n\nhttp://hs.facebook.com/group.php?gid=10220866318&ref=mf",
      "provider_url":"http://vhx.tv",
      "provider_name":"VHX.TV",
      "version":"1.0",
      "cache_age":1800
    }


### [Sample response: playlist](#sample_response_playlist)

We also include an array with all the thumbnails for videos contained in the playlist.

[http://vhx.tv/oembed.json?url=/jamiew/internet-memes-101](http://vhx.tv/oembed.json?url=/jamiew/internet-memes-101)

    {
      "type":"video",
      "html":"<iframe src=\"http://vhx.tv/embed/megaplaya.html?url=%2Fjamiew%2Finternet-memes-101\" allowfullscreen frameborder=\"0\" height=\"480\" width=\"800\" />",
      "playlist_id":12,
      "title":"Internet Memes 101",
      "description":"Classic viral videos and Internet oddities. For more visit http://knowyourmeme.com and http://youshouldhaveseenthis.com",
      "thumbnail_url":"http://vhx.tv/assets/playlists/12/mudkip_large_medium.png",
      "author_name":"Jamie Dubs",
      "author_url":"/jamiew",
      "height":800,
      "width":800,
      "provider_url":"http://vhx.tv",
      "provider_name":"VHX.TV",
      "version":"1.0",
      "cache_age":1800,
      "video_thumbnails":
      [
        "http://img.youtube.com/vi/-5x5OXfe9KY/0.jpg",
        "http://img.youtube.com/vi/qItugh-fFgg/0.jpg",
        "http://img.youtube.com/vi/HPPj6viIBmU/0.jpg",
        "http://img.youtube.com/vi/KmtzQCSh6xk/0.jpg",
        "http://img.youtube.com/vi/a1Y73sPHKxw/0.jpg",
        "http://img.youtube.com/vi/aMS0O3kknvk/0.jpg",
        "http://img.youtube.com/vi/EwTZ2xpQwpA/0.jpg",
        "http://img.youtube.com/vi/Ll-lia-FEIY/0.jpg",
        "http://img.youtube.com/vi/gx-NLPH8JeM/0.jpg",
        "http://img.youtube.com/vi/CRbhE3GRiUE/0.jpg",
        "http://img.youtube.com/vi/nda_OSWeyn8/0.jpg",
        "http://img.youtube.com/vi/rW6M8D41ZWU/0.jpg",
        "http://img.youtube.com/vi/s8MDNFaGfT4/0.jpg",
        "http://img.youtube.com/vi/hKoB0MHVBvM/0.jpg",
        "http://img.youtube.com/vi/dQw4w9WgXcQ/0.jpg",
        "http://img.youtube.com/vi/Q5im0Ssyyus/0.jpg",
        "http://img.youtube.com/vi/_OBlgSz8sSM/0.jpg",
        null,
        "http://img.youtube.com/vi/L1BDM1oBRJ8/0.jpg",
        "http://img.youtube.com/vi/CQzUsTFqtW0/0.jpg",
        "http://img.youtube.com/vi/Nnzw_i4YmKk/0.jpg",
        "http://img.youtube.com/vi/zSWUWPx2VeQ/0.jpg",
        "http://img.youtube.com/vi/x9wDG48y9E0/0.jpg",
        "http://img.youtube.com/vi/txqiwrbYGrs/0.jpg",
        "http://img.youtube.com/vi/cvCjyWp3rEk/0.jpg",
        "http://img.youtube.com/vi/OQSNhk5ICTI/0.jpg",
        null,
        "http://img.youtube.com/vi/bNF_P281Uu4/0.jpg",
        "http://img.youtube.com/vi/W45DRy7M1no/0.jpg",
        "http://img.youtube.com/vi/cIwTYL1fwJk/0.jpg",
        "http://img.youtube.com/vi/bSuvOVH0aSQ/0.jpg",
        "http://img.youtube.com/vi/BEtIoGQxqQs/0.jpg",
        "http://img.youtube.com/vi/kHmvkRoEowc/0.jpg",
        "http://img.youtube.com/vi/FzRH3iTQPrk/0.jpg",
        "http://img.youtube.com/vi/RxPZh4AnWyk/0.jpg",
        "http://img.youtube.com/vi/6bVa6jn4rpE/0.jpg",
        "http://img.youtube.com/vi/Zll_jAKvarw/0.jpg",
        "http://img.youtube.com/vi/FJ3oHpup-pk/0.jpg",
        "http://img.youtube.com/vi/CMNry4PE93Y/0.jpg",
        "http://img.youtube.com/vi/2Z4m4lnjxkY/0.jpg",
        "http://img.youtube.com/vi/-mv9Hv9MeXQ/0.jpg",
        "http://img.youtube.com/vi/dMH0bHeiRNg/0.jpg",
        "http://img.youtube.com/vi/7QLSRMoKKS0/0.jpg",
        null,
        null,
        "http://img.youtube.com/vi/nx4UEe98EkY/0.jpg",
        "http://img.youtube.com/vi/PbcctWbC8Q0/0.jpg"
      ]
    }
