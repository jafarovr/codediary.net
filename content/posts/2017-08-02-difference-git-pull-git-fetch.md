---
layout: post
status: publish
published: true
title: Difference between 'git pull' and 'git fetch'?
date: '2017-08-02 15:21:40 +0100'
date_gmt: '2017-08-02 11:21:40 +0100'
permalink: /posts/difference-git-pull-git-fetch/
tags:
- Random
---
In the simplest terms, `git pull` does a `git fetch` followed by a `git merge`.

You can do a `git fetch` at any time to update your remote-tracking branches under `refs/remotes/<remote>/`.

This operation never changes any of your own local branches under `refs/heads`, and is safe to do without changing your working copy. I have even heard of people running `git fetch`periodically in a cron job in the background (although I wouldn't recommend doing this).

A `git pull` is what you would do to bring a local branch up-to-date with its remote version, while also updating your other remote-tracking branches.

Git documentation: [git pull](http://git-scm.com/docs/git-pull)
