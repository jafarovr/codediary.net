---
layout: post
status: publish
published: true
title: Installing Oracle XE with Docker
date: '2017-01-09 19:40:21 +0000'
date_gmt: '2017-01-09 15:40:21 +0000'
permalink: /posts/installing-oracle-xe-docker/
categories:
- Linux
- Docker
- Databases
- Windows
- Oracle
tags:
- databases
- docker
- linux
---
Nicest way to install Oracle XE on any operating system (for development) is Docker. [Docker](https://www.docker.io/) is an application container for Mac, Linux and Windows. It is based on [LXC](https://linuxcontainers.org/) and gives you the ability to package complete application including their dependencies to a self-containing file (called an *image*). These images can be exchanged and run on every Linux machine where Docker is installed! Awesome!

Docker images are also shared around the community on [index.docker.io](https://index.docker.io/). And this is where this two guys come into play:

* [Wei-Ming Wu](https://github.com/wnameless) made a Docker image containing Oracle XE. You can find it [here](https://index.docker.io/u/wnameless/oracle-xe-11g/).
* [Alexei Ledenev](https://github.com/alexei-led) extended Wei-Ming Wu's image to also use the Oracle web console (APEX). You can find it [here](https://index.docker.io/u/alexeiled/docker-oracle-xe-11g/)

Both projects work the same:
* Install Docker on your machine. You can find instructions for that at [docs.docker.com/docker-for-mac](https://docs.docker.com/docker-for-mac/). (For other operating systems please see Docker's official website)
* Pull the image to your machine:
  
```bash
docker pull alexeiled/docker-oracle-xe-11g
```
Run the image:
```bash
docker run -d --shm-size=1g -p 8080:8080 -p 1521:1521 -v /local-initdb:/etc/entrypoint-initdb.d alexeiled/docker-oracle-xe-11g
```
* That's it. Absolutely simple.


Additionally, if you want to <code>ssh</code> to your&nbsp;docker container, you can run following commands:

To get `container_id`:
```bash
docker ps
```
SSH into your container:
```
docker exec -i -t [container_id] /bin/bash
```
The great thing is that those images are not as big as a virtual machine. They only contain the actual application and its environment. But they are packed in a way that they can be executed everywhere right out of the box but are still isolated. You can also make changes to the images and create another image containing your changed system.
