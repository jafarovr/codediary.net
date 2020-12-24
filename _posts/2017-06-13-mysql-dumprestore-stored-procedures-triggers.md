---
layout: post
status: publish
published: true
title: MySQL Dump/Restore Stored Procedures and Triggers
date: '2017-06-13 17:37:44 +0100'
date_gmt: '2017-06-13 13:37:44 +0100'
permalink: /posts/mysql-dumprestore-stored-procedures-triggers/
categories:
- MySQL
tags: []
---
`mysqldump` will backup by default all the triggers but NOT the stored procedures/functions. There are 2 mysqldump parameters that control this behavior:
* routines -  FALSE by default
* triggers -  TRUE by default

This means that if you want to include in an existing backup script also the triggers and stored procedures you only need to add the * routines command line parameter:
```sql
mysqldump <other mysqldump options> --routines outputfile.sql
```
Let's assume we want to backup ONLY the stored procedures and triggers and not the mysql tables and data (this can be useful to import these in another db/server that has already the data but not the stored procedures and/or triggers), then we should run something like:
```sql
mysqldump --routines --no-create-info --no-data --no-create-db --skip-opt <database> > outputfile.sql
```
and this will save only the procedures/functions/triggers of the . If you need to import them to another db/server you will have to run something like:
```sql
mysql <database> < outputfile.sql
```
