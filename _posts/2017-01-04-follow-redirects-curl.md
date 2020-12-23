---
layout: post
status: publish
published: true
title: Follow Redirects with cURL
date: '2017-01-04 01:06:48 +0000'
date_gmt: '2017-01-03 21:06:48 +0000'
permalink: /posts/follow-redirects-curl/
categories:
- Linux
tags: []
---
The `-L` flag instructs cURL to follow any redirect so that you reach the eventual endpoint.
```bash
curl -L google.com
```
