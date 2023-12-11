---
layout: post
status: publish
published: true
title: How To Enable HTTP/2 in Apache
date: '2017-04-14 13:22:13 +0100'
date_gmt: '2017-04-14 09:22:13 +0100'
permalink: /posts/how-to-enable-http2-in-apache-on-ubuntu-16-04/
tags:
- Linux
- Apache
- Ubuntu
---
Here's a simple guide showing how you can enable HTTP/2 in Apache on Ubuntu.

Today's internet connections are amazingly fast. You younglings might not believe this, but there was a time when we actually had to sit and wait for a website to appear. If you want to experience the internet speeds of the past, give [56k Emulator](http://www.56k-emulator.co.uk/) a try. It will give you the basic idea. And keep in mind that 56K modems were freaking fast when they became available.

Even though today
s internet connections are fast, the technology used to push propaganda around inside the tubes is old and slow. HTTP/1.1 was never intended to be used with the kind of content-heavy website we have today. Thankfully, there's a new option available, the marvelous RFC-7540. Or HTTP/2, if you will.

HTTP/2 is a major revision of HTTP/1.1. Its main goal is to make web sites appear in your browser quicker, and with the need to send less data than with HTTP/1.1. The 
number one HTTP server on the internet", Apache 2 only has [experimental support](https://httpd.apache.org/docs/2.4/mod/mod_http2.html) for HTTP/2. This means that it's not available in the version Ubuntu 16.04 includes by default.

Once again, we have to turn to our PPA packaging hero [Ondřej Surý](https://www.sury.org/) for support. Not only does he maintain packages for the latest and greatest version of PHP, he also makes sure Ubuntu users can be on the bleeding edge of Apache goodness.

### Getting on the HTTP/2 Highway

These instructions assume that you've already got Apache2 installed, and that the sites you want to enable HTTP/2 for has TLS (HTTPS) enabled. If you don&rsquo;t, and have no idea how to do this, please consult the internet. Log in to your Ubuntu box. Then, run the following commands:

```
# Install add-apt-repository. 
sudo apt-get install software-properties-common 

# Add Apache2 PPA repository. 
sudo add-apt-repository ppa:ondrej/apache2 

# Update your local package list. 
sudo apt-get update 

# Upgrade Apache2 with the new packages from Sur&yacute;. 
sudo apt-get upgrade
```
Next, enable HTTP/2.
```
sudo a2enmod http2
```
Then, add the Protocols directive to the VirtualHost configuration of the sites you want to server over HTTP/2:
```
<VirtualHost *:443> 
  ServerName your-awesome-site.com 
  Protocols h2 http/1.1 
</VirtualHost>
```
Finally, restart Apache2.
```
# Restart Apache2. 
sudo service apache2 restart
```
All done. Your site is now being server using HTTP/2. Hooray!
