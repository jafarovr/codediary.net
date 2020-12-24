---
layout: post
status: publish
published: true
title: How to debug integration tests in a Maven project in IntelliJ IDEA?
date: '2018-06-01 10:10:19 +0100'
date_gmt: '2018-06-01 10:10:19 +0100'
permalink: /posts/how-to-debug-integration-tests-in-a-maven-project-in-intellij-idea/
categories:
- Coding
- Java
- Maven
- Spring Boot
tags:
- Java
- debugging
- remote
- intelliij
- integration-tests
---
## What is debugging?

Debugging code is done in order to locate and fix bugs in it. This is an important step towards producing bug free code which in turn creates reliable software.

So now I will explain how to debug tests in IntelliJ IDEA IDE for a maven project in simple steps.

<!--more-->

Debugging integration tests.

### Step 1:

The maven surefire plugin will be used for this purpose. And the operating system referenced here is Ubuntu with regard to the commands.

First setup break points in the lines of code that you need to debug. For this, you need to simply click on the left corner of the lines in the code editing area where you need the tests to pause during debugging. A red dot will appear when you click.

### Step 2 :

Type the following command on the command line after moving into the directory containing your maven project's integration tests.

That is,
```bash
cd <path-to-the-directory-containing-your-maven-project's-integrationtests>
mvn clean install -Dmaven.surefire.debug
```
The tests will pause automatically and await a remote debugger on port 5005. (The port 5005 is set as default) You will see a statement in the command line notifying that it is listening in port 5005.

Listening for transport **dt_socket** at address: 5005

If you need to configure a different port, you can pass a more detailed value to the above command.

That is,

```bash
mvn clean install -Dmaven.surefire.debug="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=8000 -Xnoagent -Djava.compiler=NONE"
```
(Note : This is a single command)

This command will use port 8000 instead of 5005.

### Step 3 :

If you are running the debugger for the first time, you will have to edit the debugging configurations in the IntelliJ IDEA IDE. If you have already done the configuration and set the remote debugger port to attach to as 5005, there is no need to edit the configurations again.

The debug configurations can be edited as follows.

Go to Run --> Edit Configurations... in the IDE.

In the dialog box that appears, click on the '+' sign that you can see in the top left corner.

Select the 'Remote' option that you can find in the drop down list.

In the next window that appears, specify the port in the place where you have to specify it. Give a name for the configuration as well.

Then say Apply and then click Ok.

Now the configurations are set.

### Step 4 :

You can then attach to the running tests using the IDE.

Go to Run --> Debug...

Then select the configuration that you specified before.

Now the tests are attached to the remote debugger.

This is all what you need to do.

The tests will be paused at the break points that you have specified before. The details of the requests coming in and out when the tests are run can be seen in the IDE. You can click and remove the break points one by one and resume the program after each pause, through the IDE.

