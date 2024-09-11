---
id: 754
title: 'Programmatically Changing the Driver Signing options'
date: '2010-11-11T09:57:17+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/11/11/programmatically-changing-the-driver-signing-options/'
permalink: /2010/11/11/programmatically-changing-the-driver-signing-options/
views:
    - '4091'
short-url:
    - 'http://bit.ly/h9sdC1'
categories:
    - Programming
    - 'Windows 2003'
---

Today I was tested my unattended Citrix installation (XenApp 5 on Windows 2003) and I noticed that the install was taking longer than expected.

This was because of a popup:

![DriverSigning](http://192.168.40.25:8081/wp-content/uploads/2010/11/driversigning-1.png)

I am not sure if this popup is shown because I ran MsiExec with /Qb- (I usually do that when testing) but if the Popup is not shown it means that at least the installation of this driver (probably Citrix Universal Print driver) fails.

So this means I needed to script turning off Driver Signature Warnings. A quick search led me to kb article [kb298503](http://support.microsoft.com/kb/298503 "Driver signing registry values cannot be modified directly in Windows") which is titled “*Driver signing registry values cannot be modified directly in Windows*“. As you may guess that title drew my attention.So I modified with Regedit, expecting an error message (I assumed there was simply a Deny permission) but it succeeded. But after a little while the value is reset back to the original value.

When making the change in the GUI and observing this with Process Monitor it’s clear that the REG\_BINARY Value *PrivateHash* under *HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup* is also changed.

I played around in the GUI with the possible values and noticed that the PrivateHash key is always the same when setting the same value.

I searched which component(s) refer to PrivateHash in Windows\\System32 and the only result I got was setupapi.dll and when I openend it with Ida it clearly showed that an MD5 Hash is calculated based on a Seed and the Value of the Policy key.

The seed comes from a REG\_DWORD called seed in *HKLM\\SYSTEM\\WPA\\PnP*.

I wrote a small commandline tool that changes the Driver Signing Policy

> DriverSignPolicy.exe  
> DriverSignPolicy (c) 2010 Remko Weijnen ([www.remkoweijnen.nl](https://www.remkoweijnen.nl/))  
> Sets the Driver Signing Option for Server 2003 by commandline
> 
> the following paramater values can be used:  
> 0 (Ignore)  
> 1 (Warn)  
> 2 (Block)

Downloads are below (executable and source).

As usual have fun and **please leave a comment** if you think it’s usefull. Usage Example:

[![DriverSignPolicy](http://192.168.40.25:8081/wp-content/uploads/2010/11/driversignpolicy-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/driversignpolicy-1.png)

[ DriverSignPolicySource.zip (1542 downloads ) ](http://192.168.40.25:8081/download/driversignpolicysource-zip/?tmstv=1726048919 "Version 1.0")