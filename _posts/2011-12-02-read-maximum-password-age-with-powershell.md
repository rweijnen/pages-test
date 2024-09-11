---
id: 2212
title: 'Read Maximum Password Age with PowerShell'
date: '2011-12-02T13:47:44+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/12/02/read-maximum-password-age-with-powershell/'
permalink: /2011/12/02/read-maximum-password-age-with-powershell/
views:
    - '4497'
short-url:
    - 'http://bit.ly/t5Fua8'
categories:
    - 'Active Directory'
    - PowerShell
tags:
    - 'Active Directory'
    - maxPwdAge
    - PowerShell
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/12/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/12/image1.png)I needed to read out the Maximum Password age with a PowerShell script in a Windows 2003 domain.

Reading out the [maxPwdAge](http://msdn.microsoft.com/en-us/library/windows/desktop/ms676863(v=vs.85).aspx) attribute is a trivial task in PowerShell (I am re-using the function [AdsLargeIntegerToInt64](http://192.168.40.25:8081/2011/12/01/convert-iadslargeinteger-to-int64-in-powershell/)):

“&gt;

In my case this returns the value -78624000000000 but how do we interpret this?

The value is expressed in 100 nanosecond units which is the same unit as a windows [FILETIME](http://msdn.microsoft.com/en-us/library/windows/desktop/ms724284(v=vs.85).aspx) structure uses.

Knowing that we can use the FromTicks method from the .NET [TimeSpan](http://msdn.microsoft.com/en-us/library/system.timespan.aspx) structure to convert it to the number of days:

“&gt;

And $maxPwdDays is 91 in my case.

Note that I am using ABS to make the value positive since maxPwdAge is always negative.