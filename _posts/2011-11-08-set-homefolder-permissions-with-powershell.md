---
id: 2154
title: 'Set homefolder permissions with PowerShell'
date: '2011-11-08T21:05:19+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/11/08/set-homefolder-permissions-with-powershell/'
permalink: /2011/11/08/set-homefolder-permissions-with-powershell/
views:
    - '2698'
short-url:
    - 'http://bit.ly/uEpT0j'
categories:
    - PowerShell
tags:
    - Exchange
    - NTFS
    - Permissions
    - PowerShell
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image2.png)Today one of my collegues asked me to write a script that performs two actions for all users of a certain Organizational Unit:

1. <span style="color: #35383d;">Ensure that each user has</span> modify permissions on their homefolder
2. Make each user visible in the Exchange Address List.

Sounds like a PowerShell job right?

I reused my function to [set NTFS Permissions by SID](http://192.168.40.25:8081/2011/09/02/settings-ntfs-permissions-by-sid-in-powershell/):

  
“&gt;  
And then I only needed to get the OU and do a foreach loop on it’s children:  
“&gt;