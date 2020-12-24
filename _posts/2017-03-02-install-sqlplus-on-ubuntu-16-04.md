---
layout: post
status: publish
published: true
title: Install SQL*Plus on Ubuntu 16.04
date: '2017-03-02 12:41:49 +0000'
date_gmt: '2017-03-02 08:41:49 +0000'
permalink: /posts/install-sqlplus-on-ubuntu-16-04/
categories:
- Linux
- Ubuntu
- Databases
- Oracle
tags: []
---
First of all you need to download [Instant Client Downloads](http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html). Install alien software so you can install rpm packages by typing following command in terminal.
```bash
sudo apt-get install alien
```
Once that is done, go to the folder where the rpm files are located and execute the following:
<!--more-->
```bash
sudo alien -i oracle-instantclient*-basic*.rpm
sudo alien -i oracle-instantclient*-sqlplus*.rpm
sudo alien -i oracle-instantclient*-devel*.rpm
```
You need to install `libaio.so`. Type following command to do it:
```bash
sudo apt-get install libaio1
```
Create Oracle configuration file:
```bash
sudo vi /etc/ld.so.conf.d/oracle.conf
```
Put this line in that file:
```bash
/usr/lib/oracle/<your version>/client/lib/
```
Note - for 64-bit installations, the path will be:
```bash
/usr/lib/oracle/<your version>/client64/lib/
```
Update the configuration by running following command:
```bash
sudo ldconfig
```
Try to connect using:
```bash
sqlplus username/password@//dbhost:1521/SID
```
or:
```bash
sqlplus testuser/password
```
Note that if you installed the 64-bit version, the client is called `sqlplus64`.

Source: [http://askubuntu.com/questions/159939/how-to-install-sqlplus](http://askubuntu.com/questions/159939/how-to-install-sqlplus)
