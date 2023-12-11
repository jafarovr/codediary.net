---
layout: post
status: publish
published: true
title: Plex Server Up and Running with Docker
date: '2019-03-09 12:28:42 +0000'
date_gmt: '2019-03-09 12:28:42 +0000'
permalink: /posts/plex-server-up-and-running-with-docker/
tags:
- Random
- Linux
- Docker
---
[Plex](https://plex.tv/) is a media server which aggregates your media and allows you to access it on your home network. Installing the server is relatively simple. Installing it with Docker is even simpler. Docker also makes it easy to define a single configuration which is reusable across environments.

I'm working off a Ubuntu installation, since that is where I'm setting up my home media centre, but once docker is installed, getting Plex going should be nearly identical in any OS.

The only requirement is that Docker is installed on your machine/vm:

```
$ sudo apt-get install docker.io
$ docker --version
Docker version 1.12.3, build 6b644ec
```

Next we pull the latest Plex Docker image:

```
$ sudo docker pull plexinc/pms-docker
Using default tag: latest
latest: Pulling from plexinc/pms-docker

8aec416115fd: Downloading 22.87 MB/50.31 MB
695f074e24e3: Download complete 
946d6c48c2a7: Download complete 
bc7277e579f0: Download complete 
2508cbcde94b: Download complete
8c4564719b44: Download complete 
58f7154b2281: Download complete 
8ceeaf7f0f36: Downloading 36.76 MB/101.7 MB
Digest: sha256:a31aada7c1ed938772c3742569edd47078ae53563b9d2c53f...
Status: Downloaded newer image for plexinc/pms-docker:latest
```

We also should create somewhere for Plex to store config and transcoded data (this is optional).

```
$ mkdir ~/.plex
$ mkdir ~/.plex/config
$ mkdir ~/.plex/transcode
```

And finally, we run a new container with mapped host ports to ports required by Plex within the container:

```
$ sudo docker run \
    -d \
    --name plex \
    -p 32400:32400/tcp \
    -p 3005:3005/tcp \
    -p 8324:8324/tcp \
    -p 32469:32469/tcp \
    -p 1900:1900/udp \
    -p 32410:32410/udp \
    -p 32412:32412/udp \
    -p 32413:32413/udp \
    -p 32414:32414/udp \
    -e TZ="YOUR-TIME-ZONE" \
    -e PLEX_CLAIM="YOUR-CLAIM" \
    -e ADVERTISE_IP="http://192.168.1.10:32400/" \
    -h 192.168.1.10 \
    -v ~/.plex/config:/config \
    -v ~/.plex/transcode:/transcode \
    -v ~/media:/data \
    plexinc/pms-docker
```

The first two `-v` flags are only necessary if you created the `config` and `transcode` directories.

The port mappings (`-p 32400:32400/tcp`) map a port from the host (left), to the container (right).

Simply replace the timezone with your timezone, and get a [Plex Claim](https://plex.tv/claim). The IP addresses must be the IP address of the *host *machine, not the IP allocated to container itself.

```
$ sudo docker ps
CONTAINER ID        IMAGE                COMMAND             CREATED             STATUS                    PORTS                                                                                                                                                                                        NAMES
86fba9199b7e        plexinc/pms-docker   "/init"             11 minutes ago      Up 2 minutes (healthy)    0.0.0.0:3005->3005/tcp, 0.0.0.0:8324->8324/tcp, 0.0.0.0:1900->1900/udp, 0.0.0.0:32410->32410/udp, 0.0.0.0:32400->32400/tcp, 0.0.0.0:32412-32414->32412-32414/udp, 0.0.0.0:32469->32469/tcp   plex
```

It takes a few minutes for Plex server to boot up. Once up, you should be able to browse to `http://<your-ip>:32400`. The app is available at `http://<your-ip>:32400/web`. Also, the server is now discoverable; if you have the Plex app installed on your iPhone/iPad, you should be able to see the server in the app.

If you are using Google Cloud Compute engine (or maybe AWS, an ecosystem that has enterprise-grade firewall setup)  to run Plex server, need to set ADVERTISE_IP as external IP of the server. 

```
$ curl ipinfo.io                                                                                                                                                                
{
   "ip": "xx.xxx.xx.xxx",
   "hostname": "xx.xxx.xx.xxx.bc.googleusercontent.com",
   "city": "",
   "region": "",
   "country": "US",
   "loc": "",
   "org": "Google LLC"
 }%
 ```

The server will be automatically detected by Plex and you are all set up. If not detected, you can manually log in the server and set it up. To do that you need to map your local 32400 to server and visit [http://localhost:32400/web/](http://localhost:32400/web/)

```
$ ssh -L 32400:localhost:32400 username@xx.xxx.xx.xxx
```

Note that, when you set external IP as host IP, Plex server will be accessible from public ([http://xx.xxx.xx.xxx:32400/web/](http://xx.xxx.xx.xxx:32400/web/)) on 32400, *ideally* you can just open Plex using external IP to complete the setup, but Plex doesn't let you set it up when you use external IP, that's why we do local port mapping.

![](/uploads/docker-plex-setup.png)

Have fun!

Official Docker repo: [https://github.com/plexinc/pms-docker](https://github.com/plexinc/pms-docker)

