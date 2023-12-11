---
layout: post
status: publish
published: true
title: Java Dynamic Property Configuration
date: '2017-01-05 16:14:12 +0000'
date_gmt: '2017-01-05 12:14:12 +0000'
permalink: /posts/java-dynamic-property-configuration/
tags:
- Coding
- Java
---

Recently, I had the need of changing `config.properties` (predefined application configuration) in Java without restarting the application. I found out a very nice library from Netflix which constantly looks for the changes in your config file and applies these variables runtime.


I am adding maven repository of the library and an example code. Full documentation is available at: [github.com/Netflix/archaius](https://github.com/Netflix/archaius)

```
<dependency>
  <groupId>com.netflix.archaius</groupId>
  <artifactId>archaius-core</artifactId>
  <version>0.6.0</version>
</dependency>
```

```
import com.google.common.util.concurrent.RateLimiter;
import com.mysql.jdbc.jdbc2.optional.MysqlDataSource;
import com.netflix.config.*;
import com.netflix.config.sources.JDBCConfigurationSource;
import com.netflix.config.sources.URLConfigurationSource;

import java.io.File;

public class DynamicConfigTest {

  public static void main(String... args) throws Exception {

    MysqlDataSource ds = new MysqlDataSource();
    ds.setServerName("localhost");
    ds.setPortNumber(3306);
    ds.setDatabaseName("config");
    ds.setUser("root");
    ds.setPassword("password");

    // config to read values from database
    JDBCConfigurationSource jdbcConfigurationSource = new JDBCConfigurationSource(ds, "SELECT * FROM params", "key", "value");

    // config to read values from file.
    URLConfigurationSource urlConfigurationSource = new URLConfigurationSource(new File(System.getProperty("user.dir") + "/config.properties").toURI().toURL());

    // following FixedDelayPollingScheduler define how often it should read the file or database.
    ConfigurationManager.install(new DynamicConfiguration(urlConfigurationSource, new FixedDelayPollingScheduler(1000, 1000, false)));

    DynamicIntProperty test = DynamicPropertyFactory.getInstance().getIntProperty("key", 10000);

    System.out.println(test);

    test.addCallback(new Runnable() {
      @Override
      public void run() {
        System.out.println("value changed to: " + test.get());
      }
    });
  }
}
```
```
# config.properties
key=1
```
I found this library very useful, often I needed to change logging level from INFO to DEBUG to debug my application in runtime. This library helped me to do it, hope you'd find it helpful too.
