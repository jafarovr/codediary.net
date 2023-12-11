---
layout: post
status: publish
published: true
title: Setting file dates from EXIF data for JPEG files
date: '2016-12-30 14:07:38 +0000'
date_gmt: '2016-12-30 10:07:38 +0000'
permalink: /posts/setting-file-dates-from-exif-data-for-jpeg-files/
tags:
- Random
- Linux
---
[ExifTool](http://www.sno.phy.queensu.ca/~phil/exiftool/) (which is free software written by Phil Harvey) lets you set the last modified date of the file system to the date the picture was taken, which is stored in the EXIF data of the JPEG file by virtually any digital camera. Simply create a folder with all the JPEGs and run this command:

```
exiftool "-DateTimeOriginal>FileModifyDate" myjpgfolder
```
This also allows you to set the file date to the exact second. Typically memory cards for digital cameras are formatted as FAT which truncates creation times to even seconds, as it's one bit short for storing the time more precisely than at 2 second intervals.
