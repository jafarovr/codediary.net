---
layout: post
status: publish
published: true
title: Setting the NLS_LANG Environment Variable for Oracle Databases
date: '2017-03-02 14:48:27 +0000'
date_gmt: '2017-03-02 10:48:27 +0000'
permalink: /posts/setting-the-nls_lang-environment-variable-for-oracle-databases/
categories:
- Linux
- Ubuntu
- Windows
- Oracle
tags: []
---
Setting the NLS_LANG Environment Variable for Oracle Databases:

On Window:
```shell
set NLS_LANG=AMERICAN_AMERICA.UTF8
```
On Unix (Solaris and Linux, CentOS, Ubuntu etc.)
```bash
export NLS_LANG=AMERICAN_AMERICA.UTF8
```
It would also be advisable to set env variable in your `.bash_profile` [on start up script]. This is the place where other ORACLE env variables (ORACLE_SID, ORACLE_HOME) are usually set.
