---
id: 2551
title: 'AuthorizationManager check failed when starting PowerShell'
date: '2012-03-15T14:03:18+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/03/15/authorizationmanager-check-failed-when-starting-powershell/'
permalink: /2012/03/15/authorizationmanager-check-failed-when-starting-powershell/
views:
    - '10596'
short-url:
    - 'http://bit.ly/zB1JPG'
categories:
    - PowerShell
---

When Launching a PowerShell script I noticed the following error: “*AuthorizationManager check failed.*“

[![AuthorizationManager check failed.| At line:1 char:2 | Microsoft.PowerShell_profile.ps1'](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb21.png "PowerShell")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image21.png)

This happens because either the Current User or the All Users PowerShell profile is empty.

Solution is to either delete the file (by default it’s not present) or fill it with at least one line of code.

The path to the Current User profile is located in %UserProfile%\\My Documents\\WindowsPowerShell\\profile.ps1.

The path to the All Users profile is located in %windir%\\system32\\WindowsPowerShell\\v1.0\\profile.ps1.

See also: [Windows PowerShell Profiles](http://msdn.microsoft.com/en-us/library/windows/desktop/bb613488(v=vs.85).aspx)