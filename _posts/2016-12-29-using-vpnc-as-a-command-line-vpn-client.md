---
layout: post
status: publish
published: true
title: Using vpnc as a Command Line VPN Client
date: '2016-12-29 23:42:31 +0000'
date_gmt: '2016-12-29 19:42:31 +0000'
permalink: /posts/using-vpnc-as-a-command-line-vpn-client/
categories:
- Tutorial
- Security
tags: []
---
In many enterprises, Cisco VPNs are used to give remote developers controlled access to production web servers. This allows machines in far-flung locations to operate as if they're on the same controlled network, making security and management much easier for the network administrators. For the remote developers, on the other hand, things aren't always so smooth. VPN clients are built into most desktop operating systems, making manual connections relatively simple. However, the GUI software for connecting to a VPN can sometimes be buggy or difficult to use. In addition, it's often useful to connect remote servers to a VPN; since they rarely have a GUI installed, the familiar VPN connection tools are missing.

The command-line VPN client [vpnc](http://www.unix-ag.uni-kl.de/~massar/vpnc/") is a great solution to both problems. With it, you can quickly and easily establish a VPN connection, bypassing the GUI entirely.

## Installing vpnc
First, we need to install the vpnc client using the package manager for our operating system. Here are a few examples:
### RED HAT / CENTOS
```bash
yum install vpnc
```
### DEBIAN / UBUNTU
For Ubuntu you should prefix the command with "sudo" to execute it as root.
```bash
apt-get install vpnc
```

## Configuring vpnc
Once vpnc is installed, we need to create a configuration file for each VPN we'll be connecting to. Start by copying `/etc/vpnc/default.conf` to a file for your specific VPN, such as `lullavpn.conf`. Edit the new file in the text editor of your choice.

The default configuration file should contain something like this:
```
IPSec gateway <gateway> 
IPSec ID <group-id> 
IPSec secret <group-psk> 
IKE Authmode hybrid 
Xauth username <username> 
Xauth password <password>
```

Fill in each line of information as needed for your VPN. If you want to be asked for your password each time you connect, comment out or remove the "Xauth password" line. Here's an example of what a vpnc configuration might look like after being set up:

```
IPSec gateway 123.234.123 
IPSec ID lullavpn 
IPSec secret my-super-secret-shared-key 

# This VPN uses just a pre-shared key and no certificate, so set IKE Authmode to "psk".
IKE Authmode psk 
Xauth username robotic.coder 
Xauth password my-really-super-secret-password 

# By specifying local port as 0, we use a random source port for each 
# VPN connection, allowing multiple VPNs to be run at once. local port 0
```

As this file contains your credentials in cleartext, it's important to make sure that it's only readable by the root user. Use `chown root *.conf` to set the configuration files to all be owned by root and `chmod go-rw *.conf` to remove permissions from group and other. You can verify the file permissions by running `ls -la *.conf`.

## Connecting and Disconnecting a VPN

Connecting a specific VPN is a single command away. Note that you do not have to be in the /etc/vpnc directory to run this command:

```bash
vpnc lullavpn.conf
```

To disconnect the VPN client:
```
vpnc-disconnect
```
Remember on Ubuntu to use `sudo` to run vpnc as root.

If you have multiple VPN clients running, the disconnect command will *only* affect the most recently established connection. To kill all currently active vpnc connections, use the command: `killall vpnc`.

VPNs can be an annoyance in day-to-day work, but they're a fact of life when managing complex hosting environments and working with most corporate networks. vpnc's simplicity and flexibility can help smooth out (or automate away) some of the rough edges; with it, working with servers on a VPN can even be a pleasure!