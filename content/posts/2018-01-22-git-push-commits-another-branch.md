---
layout: post
status: publish
published: true
title: Git Push commits to another branch
date: '2018-01-22 17:14:40 +0000'
date_gmt: '2018-01-22 13:14:40 +0000'
permalink: /posts/git-push-commits-another-branch/
tags:
- Git
- VCS
---

When pushing to a non-default branch, you need to specify the source ref and the target ref:
```
git push origin branch1:branch2
```

Or
```
git push <remote> <branch with new changes>:<branch you are pushing to>
```
