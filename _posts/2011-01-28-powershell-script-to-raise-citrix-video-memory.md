---
id: 1316
title: 'PowerShell Script to raise Citrix Video Memory'
date: '2011-01-28T15:23:08+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1316'
permalink: /2011/01/28/powershell-script-to-raise-citrix-video-memory/
short-url:
    - 'http://bit.ly/h1gDfv'
views:
    - '3454'
categories:
    - Citrix
    - PowerShell
    - script
    - 'Windows 2003'
    - 'Windows Internals'
---

On a Citrix XenApp 5 environment a user reported that he was unable to start a Full Screen session on a Dual Monitor Configuration.

He received this error message:

[![foutmelding (2)](http://192.168.40.25:8081/wp-content/uploads/2011/01/foutmelding-2_thumb.png "foutmelding (2)")](http://192.168.40.25:8081/wp-content/uploads/2011/01/foutmelding-2.png)

Citrix has a KB Article: “[How to Allow More Memory for Session Graphics on Windows Server 2003](http://support.citrix.com/article/CTX114497)” that explains exactly how we can solve this.

We need to change the *MaxLVBMem* registry value and we can use the Excel Sheet from the KB Article to calculate the proper value.

Please don’t set this value too high because a higher value means you will restrict other kernel memory pools.

You also need to deny the SYSTEM account the SetValue permission on the *HKLM\\SYSTEM\\CurrentControlSet\\Control\\Session Manager\\Memory Management* key to prevent the Citrix IMA service from overwriting the new value.

So I wrote a small PowerShell script to change the permission and set the value:  
“&gt;