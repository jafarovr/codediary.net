---
layout: post
status: publish
published: true
title: How to automatically save screenshots to Google Drive on macOS
date: '2017-04-26 14:21:05 +0100'
date_gmt: '2017-04-26 10:21:05 +0100'
permalink: /posts/automatically-save-screenshots-google-drive-macos/
tags:
- macOS
- Google Drive
- Cloud
- Screenshots
---
### Saving screenshots to Drive automatically

* Create a new folder called "Screenshots" in your Google Drive.
* Open Terminal.
* Run the following commands:

```
defaults write com.apple.screencapture location ~/Google\ Drive/Screenshots/
killall SystemUIServer
```
Now screenshots will be saved to the Screenshots folder in your Google Drive, not the desktop.
