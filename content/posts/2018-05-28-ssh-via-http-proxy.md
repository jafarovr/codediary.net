---
layout: post
status: publish
published: true
title: SSH via HTTP proxy
date: '2018-05-28 09:46:01 +0100'
date_gmt: '2018-05-28 09:46:01 +0100'
permalink: /posts/ssh-via-http-proxy/
tags:
- Security
- Linux
- Ubuntu
---
If you happen to be stuck behind a corporate firewall with only HTTP proxies for external access, you might still be able to SSH out through them using the built-in `nc` on *nix systems.

First, hope that the proxies haven't disabled the `CONNECT` method, then simply add a section to your `.ssh/config` like this:


```
Host foobar.example.com
    ProxyCommand          nc -X connect -x proxyhost:proxyport %h %p
    ServerAliveInterval   10
```

This will tunnel the connection through the HTTP proxy to the remote server. The `ServerAliveInterval` setting is required as most proxies will drop the connection after a period of inactivity.

To avoid issues with trying to connect to the host when not behind the corporate firewall, replace the above with a fake entry for the proxy method like this:

```
Host foobar-proxy.example.com
    HostName              foobar.example.com
    ProxyCommand          nc -X connect -x proxyhost:proxyport %h %p
    ServerAliveInterval   10
```

Then use

```
$ ssh foobar-proxy.example.com
```

when inside the firewall, and

```
$ ssh foobar.example.com
```

when outside.

Simples, no external software required.

