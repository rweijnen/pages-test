---
id: 960
title: 'Unattended Installation of the Oracle Client'
date: '2010-12-27T17:20:48+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=960'
permalink: /2010/12/27/unattended-installation-of-the-oracle-client/
views:
    - '5832'
short-url:
    - 'http://bit.ly/hpB3mx'
categories:
    - Altiris
    - Java
    - Oracle
    - 'Unattended Installation'
---

Last week I needed to an unattended installation of the Oracle Client, in my case version 10.2.0.1.0.

Oracle has a record switch that allows you to record an installation and generate a response file using: setup -record -destinationFile response\_file\_name. This is documented [here](http://download.oracle.com/docs/cd/B19306_01/install.102/b14312/advance.htm#BBAEDBJG "Installing Oracle Database Client Using Response Files").

When you are finished you can use this response file to perform the unattended install, eg setup -silent -responseFile response\_file\_name.

There is a problem though, the installer (setup.exe) launches a java based installer and immediately returns.

This was a problem for me since I cannot deploy a dependant application if the Oracle install hasn’t finished yet.

I did some googling and found a lot of questions about this subject and saw a common resolution where a script is watching a certain file that is creating when the installation has finished in a loop.

I figured there should be a better wait so I to a closer look at the installer with [Ida Pro](http://www.hex-rays.com/idapro/ "The IDA Pro Disassebler and Debugger"). I noticed that setup.exe launches another exe. called oui.exe (Oracle Universal Installer) which in turn launches Java.

I also noticed that oui.exe waits for the install to finish using the [MsgWaitForMultipleObjects](http://msdn.microsoft.com/en-us/library/ms684242(VS.85).aspx "MsgWaitForMultipleObjects Function") function if a certain variable is True:

[![MsgWait](http://192.168.40.25:8081/wp-content/uploads/2010/12/msgwait-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/msgwait.png)

It turns out that if we launch oui.exe using start /w instead of setup.exe and pass the switch -WAITFORCOMPLETION the installer will not return until it has finished.

This is my complete job:  
“&gt;