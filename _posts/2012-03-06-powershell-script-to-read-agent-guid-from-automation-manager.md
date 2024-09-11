---
id: 2500
title: 'PowerShell script to read Agent Guid from Automation Manager'
date: '2012-03-06T16:00:06+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/03/06/powershell-script-to-read-agent-guid-from-automation-manager/'
permalink: /2012/03/06/powershell-script-to-read-agent-guid-from-automation-manager/
views:
    - '4143'
short-url:
    - 'http://bit.ly/x9KcHZ'
categories:
    - PowerShell
    - RES
tags:
    - 'Automation Manager'
    - PowerShell
    - RES
---

From a script I needed to schedule a project in RES Automation Manager 2011 for a particular server.

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image2.png)This can be done with the WMC.exe commandline tool as documented in the [Admin Guide](http://support.ressoftware.com/automationmanageradminguide/15833.htm). However we must specify the agent’s GUID instead of it’s name. We can of course use the AM console to get the agent’s GUID but it’s more flexible to script this.

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image3.png)Unfortunately there’s no API we can call so I am directly quering the AM database using a PowerShell script.

The script read the database server and database name from the registry so it assumes you have the AM console installed.

It also assumes that you are using a SQL database and that the user running the script has permissions to read the database.

 “&gt;

Usage example:

“&gt;