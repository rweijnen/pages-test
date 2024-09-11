---
id: 2664
title: 'Could not load file or assembly &#8216;System.Configuration&#8217;'
date: '2012-07-18T13:43:06+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2664'
permalink: /2012/07/18/could-not-load-file-or-assembly-system-configuration/
short-url:
    - 'http://bit.ly/NygvLH'
views:
    - '5468'
categories:
    - General
tags:
    - .NET
    - '.NET FrameWork'
    - 'Windows XP Embedded'
---

Today I was asked to troubleshoot an executable that didn’t work correctly on Windows XP Embedded.

On startup it displayed the following message:

“&gt;

I verified that System.Configuration.dll was present (in C:\\Windows\\Microsoft.NET\\Framework\\v2.0.50727).

However it not present in the [Global Assembly Cache](http://msdn.microsoft.com/en-us/library/yf1d93sz%28v=vs.71%29.aspx) (GAC):

[![Global Assembly Cache](http://192.168.40.25:8081/wp-content/uploads/2012/07/image_thumb.png "C:\Windows\Assembly")](http://192.168.40.25:8081/wp-content/uploads/2012/07/image.png)

After registering the dll to the GAC the executable ran fine.

I used the [Gacutil](http://msdn.microsoft.com/en-us/library/ex0ss12c(v=vs.71).aspx) utility to register the dll with a script (in my case with HP Device Manager)

“&gt;

Gacutil is only distributed with the [Windows SDK](http://www.microsoft.com/en-us/download/details.aspx?id=8279) and if you only need Gacutil then select only Tools under .NET Development:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/07/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/07/image1.png)

Gacutil is installed in the following locations:

- C:\\Program Files\\Microsoft SDKs\\Windows\\v7.1\\Bin\\NETFX 4.0 Tools (v4.0.30319.1)
- C:\\Program Files\\Microsoft SDKs\\Windows\\v7.1\\Bin (v3.5.30729.1)