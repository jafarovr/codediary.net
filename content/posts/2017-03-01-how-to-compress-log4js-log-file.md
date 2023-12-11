---
layout: post
status: publish
published: true
title: How to Compress Log4J's Log File
date: '2017-03-01 15:24:32 +0000'
date_gmt: '2017-03-01 11:24:32 +0000'
permalink: /posts/how-to-compress-log4js-log-file/
tags:
- Random
- Coding
- Java
- Maven
---
I'm using log4j's DailyRollingFileAppender which would create a new log file everyday. Each file's size is around 100 to 200MB, and I see the log files folder is growing bigger and bigger every day.

My solution to them is to compress everyday's log into gzip, gz or perhaps into zip file. Basically, im using one of [log4j's companion](http://logging.apache.org/log4j/companions/extras/).

So here is my solution, first a simple pom.xml to download the libraries needed.

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
 
    <groupId>net.codediary</groupId>
    <artifactId>Log</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
 
    <name>Log</name>
 
    <dependencies>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>apache-log4j-extras</artifactId>
            <version>1.0</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>                       
    </dependencies>
</project>
```

And then my log4j.properties file, please take a look at line 7. It will create a new log file for every second, change it into `yyyyMMdd` if you want to create a daily appender.

```
log4j.rootLogger=DEBUG, request
 
log4j.appender.request=org.apache.log4j.rolling.RollingFileAppender
log4j.appender.request.File=msg.log
log4j.appender.request.RollingPolicy=org.apache.log4j.rolling.TimeBasedRollingPolicy
log4j.appender.request.RollingPolicy.ActiveFileName =msg.log
log4j.appender.request.RollingPolicy.FileNamePattern=msg.%d{yyyyMMdd.HHmmss}.gz
log4j.appender.request.layout = org.apache.log4j.PatternLayout
log4j.appender.request.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
```