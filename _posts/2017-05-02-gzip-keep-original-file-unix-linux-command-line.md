---
layout: post
status: publish
published: true
title: How to gzip and keep original file on Unix or Linux command line
date: '2017-05-02 13:36:56 +0100'
date_gmt: '2017-05-02 09:36:56 +0100'
permalink: /posts/gzip-keep-original-file-unix-linux-command-line/
categories:
- Linux
- Ubuntu
tags:
- gzipp
- gunzip
---
I would like to compress a log file using gzip Unix command line utility, and I would also like to keep the original file. However, when I use the gzip my-app.log command, results in modifying my log file and renaming it my-app.log.gz. How do I force the gzip command to keep original file while maintaining the original file on Linux or Unix-like system?

The gzip program compresses and decompresses files on Unix like system. You need to pass the `-c` or `--stdout`, or `--to-stdout` option to the gzip command. This option specifies that output will go to the standard output stream, leaving original files intact.

### Syntax: To keep original file while using gzip
The options are as follows:
```bash
gzip -c input.file > output.file.gz
```
If no files are specified and in direction used, gzip will compress from standard input, or decompress to standard output. So one can use the following syntax:
```bash
gzip < input.file > output.file.gz
```
Or pass the `-k`/`--keep` to the gzip command to leep (don't delete) input files during compression or decompression:
```bash
gzip -k input.file
gzip --keep input.file
```
### Examples
Let us tell gzip command to keep original file called Friday-Comic.jpg :
```bash
$ ls -lh Friday-Comic.jpg
```
Gzip and create a new file called Friday-Comic-1.jpg.gz:
```bash
$ gzip -c Friday-Comic.jpg > Friday-Comic-1.jpg.gz
$ ls -lh Friday-Comic-1.jpg.gz
```
Gzip and create a new file called Friday-Comic-1.jpg.gz using shell redirection:
```bash
$ gzip < Friday-Comic.jpg > Friday-Comic-2.jpg.gz
$ ls -lh Friday-Comic*
```
