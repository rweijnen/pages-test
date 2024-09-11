---
id: 2468
title: 'Convert MCli output into PowerShell Objects'
date: '2012-02-29T21:35:49+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/02/29/convert-mcli-output-into-powershell-objects/'
permalink: /2012/02/29/convert-mcli-output-into-powershell-objects/
views:
    - '3717'
short-url:
    - 'http://bit.ly/w80y8G'
categories:
    - Citrix
    - PowerShell
tags:
    - Citrix
    - PowerShell
    - PVS
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb21.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image21.png)I was experimenting today with the PowerShell cmdlets for Citrix Provisioning Server. I was surprised to learn that the output of these cmdlets are not PowerShell types such as collections and objects with methods and properties but just plain text output.

A google search for a method to quickly convert the garbage output to objects led me to [this blog post](http://www.out-web.net/?p=599) by Frank Peter. He describes a clever use of the switch statement with regular expressions with the Get-DiskInfo cmdlet.

Using Frank’s code as a basis I wrote a generic function that converts Mcli output to an array of objects.

I slightly changed the regular expression for the name/value because my output was not indented.

Let’s look at the code:

 “&gt;

Usage Example:

“&gt;

Please note that you need to pass the cmdlet and parameters to call as a string.