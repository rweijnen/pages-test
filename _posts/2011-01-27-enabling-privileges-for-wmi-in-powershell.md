---
id: 1301
title: 'Enabling Privileges for WMI in PowerShell'
date: '2011-01-27T11:05:30+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1301'
permalink: /2011/01/27/enabling-privileges-for-wmi-in-powershell/
views:
    - '6032'
short-url:
    - 'http://bit.ly/fwTyG1'
categories:
    - PowerShell
---

A few days ago I wrote about a [PowerShell Script to Install Printer Drivers](/blog/2011/01/25/script-to-install-all-print-drivers-on-citrix-or-terminal-server/).

I noticed there was a problem with this script: some drivers fail to load with error 1797 which means [ERROR\_UNKNOWN\_PRINTER\_DRIVER](http://msdn.microsoft.com/en-us/library/ms681386(v=vs.85).aspx).

I reread the [AddPrinterConnection](http://msdn.microsoft.com/en-us/library/aa384769(v=vs.85).aspx) documentation on MSDN but it didn’t mention anything about additional required permissions or anything.

But then I read the remarks sections of the [Win32\_Printer Class](http://msdn.microsoft.com/en-us/library/aa394363(v=VS.85).aspx) and it mentions that for some operations the SeLoadDriverPrivilege is required.

In VBScript we can indicate it like this:  
“&gt;  
But how to do this in PowerShell?

I didn’t find a way to enable a specific privilege but we can enable all by setting Scope.Options.EnablePrivileges to $true.

So I modified the script like this:  
“&gt;