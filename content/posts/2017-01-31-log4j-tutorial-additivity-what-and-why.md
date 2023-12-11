---
layout: post
status: publish
published: true
title: 'Log4j Tutorial: Additivity; what and why?'
date: '2017-01-31 02:50:12 +0000'
date_gmt: '2017-01-30 22:50:12 +0000'
permalink: /posts/log4j-tutorial-additivity-what-and-why/
tags:
- Tutorial
- Coding
- Java
---
You've configured a total of *three appenders* in your application. One for the package *com.demo.moduleone* and one for *com.demo.moduletwo* and one root logger for *com.demo*. The log4j configuration will look something like this (showing only the appender configuration, excluding other details)


```
log4j.category.com.demo.moduleone = INFO, moduleOneFileAppender
log4j.category.com.demo.moduletwo = INFO, moduleTwoFileAppender
log4j.rootLogger = INFO, rootFileAppender
```

The Log4j loggers are following hierarchies. ie, A log4j logger is said to be an *ancestor* of another logger if its name followed by a dot is a prefix of the *descendant* logger name. A log4j logger is said to be a *parent* of a *child* logger if there are no ancestors between itself and the descendant logger.

So, as per the hierarchy, our rootFileAppender is the *parent* appender for both moduleOneFileAppender and moduleTwoAppender. So, all the log messages that are coming to the child appenders will be propagated to the parent appenders too. So, in our scenario, the log messages from the package *com.demo.moduleone* will be sent to the *moduleOneFileAppender* plus the *rootFileAppender*. The same applies to the *com.demo.moduletwo* also. This leads to write the same log message in two different location.
>

### How to avoid this redundancy?
In order to avoid this redundancy, we can use **Log4j additivity**. Just set the *additivity* property of an Log4j logger to **false** and then the log messages which are coming to that logger will **not** be propagated to its parent loggers. So, our new Log4j configuration file would be:

```
log4j.category.com.demo.moduleone = INFO, moduleOneFileAppender
log4j.additivity.com.demo.moduleone = false

log4j.category.com.demo.moduletwo = INFO, moduleTwoFileAppender
log4j.additivity.com.demo.moduletwo = false

log4j.rootLogger = INFO, rootFileAppender
```

With the above configuration, the log messages from the *com.demo.moduleone* will go to the moduleOneAppender only and the rest of the log messages will go to the rootFileAppender.

Source: [http://veerasundar.com/blog/2009/08/log4j-tutorial-additivity-what-and-why/](http://veerasundar.com/blog/2009/08/log4j-tutorial-additivity-what-and-why/)
