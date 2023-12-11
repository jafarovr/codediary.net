---
layout: post
status: publish
published: true
title: Logging uncaught exception (redirecting from sysout to logger)
date: '2016-12-30 12:28:03 +0000'
date_gmt: '2016-12-30 08:28:03 +0000'
permalink: /posts/logging-uncaught-exception-redirecting-from-sysout-to-logger/
tags:
- Coding
- Java
---
Uncaught exceptions in Java are handled by the System. The JVM usually logs the exception into the System.err and then shuts down. But in case of web applications its different. It logs the printstack trace to server console and then continues. Even though most of the time System.err is redirected to a file system, I wanted a better way of handling it just like any other exception stacktrace. I wanted logger (you can use *log4j* too) to handle it, so I it can help maintainers better.

Java gives `Thread.UncaughtExceptionHandler` to handle any such exceptions. I implemented my own.

```
import java.util.logging.*;

public class TestUncaughtExceptionHandling {
    public static void main(String[] args) {
        //This registers the exception 
        //handler for every thread
        Thread.setDefaultUncaughtExceptionHandler(new MyUncaughtExceptionHandler());

        //raise ArithmeticException which is 
        //runtime exception and uncaught
        //this will be handled by MyExceptionHandler
        int r = 5 / 0;

    }
}

//simple UncaughtExceptionHandler which logs
class MyUncaughtExceptionHandler implements Thread.UncaughtExceptionHandler {
    //Get a logger. You can use LogConfigurator/Manager
    //This logger will conatin the stacktrace of all 
    //the uncaught exceptions in the world.
    Logger log = Logger.getLogger("UncaughtExceptionHandler");

    public MyUncaughtExceptionHandler() {
        try {
            //All this is done by LogConfigurator. 
            //Dont worry much.
            FileHandler handler = new FileHandler("c:\\uncaught_exception%u.log", true);
            handler.setFormatter(new SimpleFormatter());
            log.addHandler(handler);
            log.setUseParentHandlers(false);
        } catch (Exception e) {}
    }
    //Implement your own way of logging here
    public void uncaughtException(final Thread t, final Throwable e) {
        String msg = String.format("Thread %s: %s", t.getName(), e.getMessage());
        LogRecord lr = new LogRecord(Level.SEVERE, msg);
        lr.setThrown(e);
        log.log(lr);
    }
}
```