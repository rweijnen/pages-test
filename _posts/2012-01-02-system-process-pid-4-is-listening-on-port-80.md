---
id: 2264
title: 'System Process PID 4 is listening on port 80'
date: '2012-01-02T10:23:21+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/01/02/system-process-pid-4-is-listening-on-port-80/'
permalink: /2012/01/02/system-process-pid-4-is-listening-on-port-80/
views:
    - '9620'
short-url:
    - 'http://bit.ly/scEc3z'
categories:
    - Apple
    - General
    - iPhone
    - 'Windows 7'
---

I wanted to save the SHSH signatures from my iPhone before updating to iOS 5.01. I started [Tiny Umbrella](http://thefirmwareumbrella.blogspot.com/) but it showed an error indicating that there’s already a process listening on port 80:

[![Cannot Start TSS Service | DO NOT TRY RESTORING YOUR DEVICE!!! | System(PID:4) must be killed!!](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML5f09e5bf_thumb.png "Cannot Start TSS Service")](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML5f09e5bf.png)

I verified this using netstat (*netstat -aon | find /I “LISTENING” | find /I “:80”*):

[![netstat -aon | find /i "LISTENING" | find /i ":80"](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML5f0d6d75_thumb.png "netstat")](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML5f0d6d75.png)

A telnet to port 80 showed there was indeed a webserver running:

[![SNAGHTML5f0f8e2e](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML5f0f8e2e_thumb.png "SNAGHTML5f0f8e2e")](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML5f0f8e2e.png)

The fact that the webserver is running in the system process means that it needs to have System Privileges. It could be either a Service or, less friendly, some kind of malware DLL that injects itself.

I checked with Process Explorer which DLL’s are loaded into the System Process and this didn’t reveal any strange DLL’s:

[![System Process | PID 4 | Loaded DLL's](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML5f18c268_thumb.png "Process Explorer")](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML5f18c268.png)

So a service seemed most likely but I didn’t have any of the usual suspects (in most likely order):

- <font color="#35383d">IIS Admin Service (</font>Internet Information Server)
- SQL Server Reporting Services
- Apache Tomcat 7

 But in my case it was the “Web Deployment Agent Service” (*MsDepSvc*) which is installed by [Microsoft WebMatrix](http://www.microsoft.com/web/webmatrix/).