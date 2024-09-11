---
id: 2327
title: 'Take ownership of a registry key in PowerShell'
date: '2012-01-16T16:39:36+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/01/16/take-ownership-of-a-registry-key-in-powershell/'
permalink: /2012/01/16/take-ownership-of-a-registry-key-in-powershell/
views:
    - '8861'
short-url:
    - 'http://bit.ly/yvvw3C'
categories:
    - PowerShell
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image11.png)After reading Andy Morgan’s (excellent) blog post about[ Removing Screen Resolution and Personalize shell extensions from a users desktop session](http://andrewmorgan.ie/2012/01/16/removing-screen-resolution-and-personalize-shell-extensions-from-a-users-desktop-session/) I couldn’t help it.

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image12.png)I had to write a PowerShell script to take ownership of the mentioned registry keys. So here goes:

The code is only quick to show we can do it (after all PowerShell has no limits) and could be improved error handling and so on. But it works!  
“&gt;