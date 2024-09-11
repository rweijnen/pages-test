---
id: 815
title: 'Script to register ASP.NET in IIS'
date: '2010-11-26T12:37:01+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=815'
permalink: /2010/11/26/script-to-register-asp-net-in-iis/
views:
    - '5411'
short-url:
    - 'http://bit.ly/fgloDc'
categories:
    - Altiris
    - General
---

I did an unattended install of the Citrix WebInterface on a testmachine and it failed with error 258.

The logfile clearly indicates the reason:

> \[ ERROR \] com.citrix.wi.install.SetupErrorReporter: Error 258 occurred: ASP.NET 2.0 must be registered and enabled in Microsoft Internet Information Services before the Web Interface can be installed.

We can register ASP.NET in IIS with the [Aspnet\_regiis.exe](http://msdn.microsoft.com/en-us/library/k6h9cz8h(VS.80).aspx "ASP.NET IIS Registration Tool (Aspnet_regiis.exe)") tool that comes with the .NET framework.

The commandline would be:

> aspnet\_regiis.exe -enable -ir

We do need to make sure we are using the proper version of the tool (x86 or x64), I did that in the Embedded Altiris Script:  
“&gt;  
Related Articles:

- [Unattended Install of the Citrix Xenapp WebInterface 5.2](http://192.168.40.25:8081/2009/11/17/unattended-install-of-the-citrix-xenapp-webinterface-5-2/)
- [Citrix Workspace Control not working on HP t5540](http://192.168.40.25:8081/2009/12/03/citrix-workspace-control-not-working-on-hp-t5540/)