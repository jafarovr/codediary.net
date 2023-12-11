---
layout: post
status: publish
published: true
title: Access MySQL Without Password
date: '2017-06-11 02:20:22 +0100'
date_gmt: '2017-06-10 22:20:22 +0100'
permalink: /posts/access-mysql-without-password/
tags:
- Linux
- Ubuntu
- MySQL
---
For MySQL you can specify your user and password in local config file (`.my.cnf`). This file should be in your home directory (i.e. ~/.my.cnf).

```
[mysql]
user=user
password=password
```