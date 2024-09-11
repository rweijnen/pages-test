---
id: 4227
title: 'How to remove Windows.old folder'
date: '2018-01-16T13:00:14+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4227'
permalink: /2018/01/16/remove-windows-old-folder/
views:
    - '952'
categories:
    - 'Windows 10'
tags:
    - 'Disk Cleanup'
    - 'Windows 10'
    - 'Windows Insider'
    - Windows.old
---

Just a very quick note here (mostly a note to self) but I had a couple of folder from previous Windows 10 installations named Windows.old Windows.old(1), Windows.old(2) etc.

These folders should be removed when you use Disk Cleanup and select the “Remove previous Windows Installation(s)” option.

[![DiskCleanup](http://192.168.40.25:8081/wp-content/uploads/2018/01/DiskCleanup_thumb.png "DiskCleanup")](http://192.168.40.25:8081/wp-content/uploads/2018/01/DiskCleanup.png)

However I already did that and for some reasons a subfolder named `C:\\Windows.old\\Users\\&lt;username&gt;\\AppData\\Local\\Packages\\Microsoft.Windows.Cortana\_cw  
5n1h2txyewy\\LocalState` couldn’t be deleted and therefore the parent folder couldn’t be deleted.

I couldn’t delete them with Windows Explorer nor via the cmd prompt or PowerShell. Then I tried to use the [`\\\\?\\` prefix](http://192.168.40.25:8081/2016/05/09/invalid-file-handle-when-trying-to-delete-a-file/) and that worked:

[![image](http://192.168.40.25:8081/wp-content/uploads/2018/01/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2018/01/image.png)