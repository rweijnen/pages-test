---
id: 2673
title: 'My Network Places Internals'
date: '2012-07-19T11:26:12+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2673'
permalink: /2012/07/19/my-network-places-internals/
short-url:
    - 'http://bit.ly/NSpjKL'
views:
    - '2880'
categories:
    - PowerShell
    - Vista
    - 'Windows 2003'
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 7'
    - 'Windows Internals'
---

[![Nethood Icon](http://192.168.40.25:8081/wp-content/uploads/2012/07/image_thumb2.png "Nethood")](http://192.168.40.25:8081/wp-content/uploads/2012/07/image2.png)I am using a PowerShell script to copy some elements of from the users old profile location to a new location. This includes the Nethood ("My Network Places") folder which contains the Network Places shortcuts.

A user reported that she could not save documents to Network Places anymore and after inspection the Network Places shortcuts were broken.

I started comparing the old Nethood folder to the new and observed the following difference in Explorer:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/07/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/07/image3.png)

When copying entries from the Nethood folder with Explorer manually they worked fine, so somehow Explorer gives the Nethood folder special treatment.

When you browse the Nethood folder from a command prompt or a tool like Total Commander you can see that an entry in Nethood is really a folder containing 2 files:

1. <font color="#35383d">desktop.ini</font>
2. <font color="#35383d">target.lnk</font>

 [Desktop.ini](http://msdn.microsoft.com/en-us/library/windows/desktop/cc144102%28v=vs.85%29.aspx) is a hidden file that instructs Explorer how to display a folder and in the case of a Nethood entry it’s always the same:  
“&gt;

The Guid {0AFACED1-E828-11D1-9187-B532F1E9575D} belongs to "Folder Shortcut" and indeed the shortcut in target.lnk is a shortcut to a folder.

If we inspect target.lnk we can see that’s in indeed a shortcut to a (shared) folder:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/07/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/07/image4.png)

But there is two more ingredients missing for Explorer to show the folder as a Nethood entry:

- The folder needs to have the *Read-only* Attribute.
- The file desktop.ini needs to have the *Hidden* Attribute.

And this is exactly what went wrong in the PowerShell filecopy: the Read-only attribute wasn’t copied along!

I wrote a Fixup function in PowerShell:

“&gt;