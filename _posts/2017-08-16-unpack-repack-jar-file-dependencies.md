---
layout: post
status: publish
published: true
title: Unpack and repack a JAR file with all of its dependencies
date: '2017-08-16 17:08:43 +0100'
date_gmt: '2017-08-16 13:08:43 +0100'
permalink: /posts/unpack-repack-jar-file-dependencies/
categories:
- Random
- Linux
- Coding
- Java
tags: []
---
 Repacking an unpacked JAR is a little frustrating because of the folder structure 

 When unpacking with: 

```bash
jar xvf JAR_NAME.jar
```
you get a `JAR_NAME/` folder 

To repack the JAR: 

remove old jar 
```bash
rm JAR_NAME.jar
```
get inside the folder 
```bash
cd JAR_NAME
```
pack the jar referencing the parent folder 

```bash
jar cf ../JAR_NAME.jar *
```
and you will end up with the JAR_NAME.jar in the parent folder, where the original was unpacked from, without the first folder level you would get if you had packed the folder itself. 

