---
layout: post
status: publish
published: true
title: Send emails stored in /var/spool/mail/root to a Gmail Inbox
date: '2016-12-30 13:39:17 +0000'
date_gmt: '2016-12-30 09:39:17 +0000'
permalink: /posts/send-emails-stored-in-varspoolmailroot-to-a-gmail-inbox/
categories:
- Tutorial
- Linux
tags: []
---
Typically a lot of scripts, cron jobs and such will generate output that gets send by email to the operator. The only guaranteed operator account to exists on all Linux/Unix boxes is root, so that becomes the default mail recipient.

The same holds for email that bounces and can't get delivered.

Typically the administrator will configure the system to forward mail addressed to root to an user acount (locally or remote). The default is via `/etc/aliases`
```
# Basic system aliases -- these MUST be present.
mailer-daemon:  postmaster
postmaster:     root
# Forward all mail to root to the email
root:           your@email.com
```
If your VPS comes with sendmail you need to run `newaliases` to activate the changes.

To route email you need DNS so start by confuguring DNS. Edit `/etc/resolv.conf` and add the following lines:
```
# /etc/resolv.conf
# Use Google's public DNS servers
nameserver 8.8.4.4
nameserver 8.8.8.8
```
Often this is already sufficient to send email.
