---
layout: post
status: publish
published: true
title: Encode or Decode base64 from the Command Line
date: '2017-11-01 20:12:56 +0000'
date_gmt: '2017-11-01 16:12:56 +0000'
permalink: /posts/encode-decode-base64-command-line/
categories:
- Linux
- Ubuntu
- macOS
tags:
- linux
- base64
- encode
- decode
- ubuntu
---
If you have ever needed to quickly decode or encode base64, Linux has a command line utility called base64 that works great. I'll show you how it works!

To encode text to base64, use the following syntax:
```bash
$ echo -n 'codediary.net rocks' | base64
Y29kZWRpYXJ5Lm5ldCByb2Nrcw==
```
To decode, use base64 -d. To decode base64, use a syntax like the following:
```bash
$ echo -n Y29kZWRpYXJ5Lm5ldCByb2Nrcw== | base64 -d
codediary.net rocks
```
Note: if on OS X, use capital D:
```bash
echo -nY29kZWRpYXJ5Lm5ldCByb2Nrcw== | base64 -D
```
