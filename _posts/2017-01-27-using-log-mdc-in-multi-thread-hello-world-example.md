---
layout: post
status: publish
published: true
title: Using Log MDC in multi-thread - Hello World example
date: '2017-01-27 00:16:15 +0000'
date_gmt: '2017-01-26 20:16:15 +0000'
permalink: /posts/using-log-mdc-in-multi-thread-hello-world-example/
categories:
- Tutorial
- Coding
- Java
tags: []
---
The slf4j has MDC which will delegate to underlying logging system's MDC implementation. The most popular logging systems are log4j and logback. They both support MDC. So the most common way to use MDC (or logging system) is through slf4j API. In this demo, logback is used as underline logging system. **What can MDC (Mapped Diagnostic Context) do for me?** MDC is a map like structure, it can save information you want to output to the log, so you don't need add that information to every logger.info(&hellip;) as parameter. For example, in an web application, you want every log output contain http request source IP. The IP string don't need to be added to every logger call , or passed back and forth between controller and service layers. You should save that IP in the MDC, and use `%X{IP}` in log configuration to add that value to every line of your log output.

BTW, MDC is thread-safe. No worrying for concurrency. 

**MDC in multi-thread environment**

In multi-thread environment, if the child is create with the basic Thread + Runnable way, the child thread will automatically inherit parent's MDC. But if using executor as thread management. The MDC inheritance is not warranted, as the logback document said:

> A copy of the mapped diagnostic context can not always be inherited by worker threads from the initiating thread. This is the case when `java.util.concurrent.Executors` is used for thread management.

In this hello world demo, there are 2 threads. The main thread will create a child thread using package java.utils.concurrent instead of the old style Thread way to show how to keep MDC context inheritance.
### 0. What you need

* JDK 1.7
* Maven 3.2.1
* SLF4J 1.7.7
* Logback 1.1.2

### 1. Configure the maven pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                        http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>net.codediary.demo</groupId>
  <artifactId>slf4j-logback-mdc</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>
 
  <name>slf4j-logback-mdc</name>
  <url>http://maven.apache.org</url>
 
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
 
  <dependencies>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>1.7.7</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.1.2</version>
      <scope>runtime</scope>
    </dependency>
  </dependencies>
</project>
```
Add dependencies for slf4j and logback in pom.xml

### 2. Define the Java class

There are 2 classes in this demo. First the Child.java.

```java
package net.codediary.demo;
 
import java.util.Map;
 
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.slf4j.MDC;
 
public class Child implements Runnable {
  private Logger logger = LoggerFactory.getLogger(Child.class);
   
  // contextMap is set when new Child() is called
  private Map<String,String> contextMap = MDC.getCopyOfContextMap();
   
  public void run() {
    MDC.setContextMap(contextMap);  // set contextMap when thread start
    logger.info("Running in the child thread");
  }
}
```

The Child.java is very simple, just implements the Runnable interface. One thing need to mention is how the MDC context is passed from parent thread to child thread. The Child object instance is created in** parent thread**, when* new Child()* is call immediately before executor create new thread. So the parent's MDC context is duplicated to a variable called *contextMap, *then set back to MDC in child thread at the begin of* run()*method.
The Parent.java with main()

```java
package net.codediary.demo;
 
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;
 
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.slf4j.MDC;
 
public class Parent {
  private Logger logger = LoggerFactory.getLogger(Parent.class);
  private ExecutorService executorService = Executors.newCachedThreadPool();
   
  public Parent() {
    // Mimic Web app, save common info in MDC
    MDC.put("IP", "192.168.1.1");
  }
 
  public void runMultiThreadByExecutor() throws InterruptedException {
    logger.info("Before start child thread");
     
    executorService.submit(new Child());
    logger.info("After start child thread");
     
    executorService.shutdown();
    executorService.awaitTermination(1, TimeUnit.SECONDS);
    logger.info("ExecutorService is over");
  }
 
  public static void main(String[] args) throws InterruptedException {
    Parent parent = new Parent();
    parent.runMultiThreadByExecutor();  //MDC OK
  }
}
```

In Parent class, ExecutorService from java.util.concurrency package is used to create a new child thread. In parent's constructor , String "192.168.1.1" has been put in MDC with key "IP". The key/value can be any String. MDC works just like an ordinary `Map<String,String>`. It will be used in logback's patten layout shown below.

### 3. Configure logging system to use values in MDC
In logback, the value can be get using `%X{KEY}` in appender's pattern. So in this demo, it should be `%X{IP}`. Here is the logback.xml for this demo.
```xml
<configuration>
  <!-- to screen -->
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <!-- screen output pattern -->
      <pattern>%d{HH:mm:ss.SSS} [%X{IP}] [%thread] %-5level [%F:%L] - %msg %n</pattern>
    </encoder>
  </appender>
  
  <root level="INFO">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```