---
layout: post
status: publish
published: true
title: Give directories and files privileges
date: '2016-12-30 12:15:31 +0000'
date_gmt: '2016-12-30 08:15:31 +0000'
permalink: /posts/give-directories-and-files-privileges/
categories:
- Linux
tags: []
---
To recursively give **directories** read & execute privileges:

```bash
find /path/to/base/dir -type d -exec chmod 755 {} +
```

To recursively give **files** read privileges:

```bash
find /path/to/base/dir -type f -exec chmod 644 {} +
```