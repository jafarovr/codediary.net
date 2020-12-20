---
layout: post
status: publish
published: true
title: Uploading files with Curl
date: '2016-12-30 12:12:31 +0000'
date_gmt: '2016-12-30 08:12:31 +0000'
permalink: /posts/uploading-files-with-curl/
categories:
- Tutorial
- Linux
tags: []
---
I've always trouble uploading files with Curl. Somehow the syntax for that command won't stick, so I post it here for future reference.

What I want to do is perform a normal `POST`, including a file and some other variables to a remote server. This is it:
```bash
curl -i -F name=test -F filedata=@localfile.jpg http://example.org/upload
```

You can add as many `-F` as you want. The `-i` option tells curl to show the response headers as well, which I find useful most of the time.
