---
layout: default
title: Introduction
---

<p>VHX's API provides access to almost all the features of the vhx.tv website.</p>

###  Getting Started

Hi hi. Here's some stuff for you to get started with.

### Currently supported video sites

* YouTube
* Vimeo

We have near-term plans to add support for Blip.tv, TED and raw video files.

### General Paramaters

_page_

Actions that return lists of objects are paged, with a few exceptions. Currently all pages return 50 results (subject to change)

_url_

All video-adding methods (share, queue, like, etc.) accept either an id parameter or a ?url= paramter. This lets you easily specify a native VHX video ID or an arbitrary YouTube or Vimeo URL


### Rate Limits

Rate limits are subject to change. Currently, we allow 86k requests per day (the "request window"), which is about 1 request per second. Every API response contains the following HTTP headers (identical to Twitter's API):

- X-RateLimit-Limit -- number of requests allowed during window
- X-RateLimit-Remaining -- number of requests left in the current window
- X-RateLimit-Reset -- unixtime when your request window we be reset


## Legal

By using the VHX API, you agree to the [VHX API Terms of Service](/tos.html).
