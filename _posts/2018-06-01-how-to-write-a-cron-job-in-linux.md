---
layout: post
status: publish
published: true
title: How to write a Cron Job in Linux?
date: '2018-06-01 10:06:16 +0100'
date_gmt: '2018-06-01 10:06:16 +0100'
permalink: posts/how-to-write-a-cron-job-in-linux/
categories:
- Linux
- Ubuntu
tags:
- linux
- ubuntu
- cronjob
- scheduledjobs
- schedule
- automate
- jobs
---

### What is a Cron Job?


Cron is a Linux utility where you can setup a task in your machine to run automatically, if required in a repetitive manner at a specific time and date. Such a task that you schedule is called a Cron Job.


Here we'll look into how we can setup a cron job in a Linux machine.

Follow the steps bellow.

### Step 1
Place the script you want to schedule as a Cron Job to one of the following directories in your machine depending on how often you need to repeat the execution of the script.

```
/etc/cron.hourly
/etc/cron.daily
/etc/cron.weekly
/etc/cron.monthly
```

For example, if you want to schedule your task daily, place the script in the `/etc/cron.daily` directory.

### Step 2

Then give correct permissions to the script as follows. Assume `script.sh` is the script that you need to schedule.

```bash
cd /etc/cron.daily
chmod 755 script.sh
```

### Step 3
Then you should add a new Cron Job to crontab.
```bash
crontab -e
```
### Step 4

Then you will be prompted with your vi editor in the terminal. Type in the following into the editor and to save it hit ESC key, and then type `:w` followed by `:q` to exit.

```
0 0 * * * /etc/cron.daily/script.sh
```

This command will make the Cron Job run every night.

**Note**: Look at the following to identify the different ways of customizing the command given in Step 4.

![](/uploads/cron-job.png)
