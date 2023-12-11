---
layout: post
status: publish
published: true
title: Remove outdated installed versions of Homebrew packages
date: '2017-04-17 10:19:21 +0100'
date_gmt: '2017-04-17 06:19:21 +0100'
permalink: /posts/remove-outdated-installed-versions-of-homebrew-packages/
tags:
- Random
- macOS
- Homebrew
---
The [cleanup](https://github.com/Homebrew/brew/blob/master/docs/FAQ.md#how-do-i-uninstall-old-versions-of-a-formula) (`brew cleanup`) command will remove outdated installed package versions. 

To affect a particular package/formula, you may supply a formula name like so: `brew cleanup $FORMULA`. To simulate cleanup, i.e. see what would be removed, you may use the `-n` option: `brew cleanup -n`.
