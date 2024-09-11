---
id: 890
title: 'Default Explorer View'
date: '2010-12-19T14:40:40+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/12/19/default-explorer-view/'
permalink: /2010/12/19/default-explorer-view/
views:
    - '4130'
short-url:
    - 'http://bit.ly/gBnYFB'
categories:
    - Citrix
    - 'Terminal Server'
    - 'Windows 2003'
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 7'
    - 'Windows XP'
---

As you probably know there are several different Folder Views in Windows Explorer:

![ExplorerView](http://192.168.40.25:8081/wp-content/uploads/2010/12/explorerview.png)

The Explorer keeps tracks of the last used View per Folder in the registry in the key HKEY\_CURRENT\_USER\\Software\\Microsoft\\Windows\\Shell\\Bags. This [KB article](http://support.microsoft.com/kb/813711 "Changes to the size, view, icon or position of a folder are lost") sort of desribes this functionality.

But did you know that you can force a specific view for all folders?

It’s very easy, just set the following key and values:

; Force default explorer view to Details (mode = 4)  
\[HKEY\_CURRENT\_USER\\Software\\Microsoft\\Windows\\ShellNoRoam\\Bags\\All Folders\\Shell\]  
“Mode”=dword:00000004  
“WFlags”=dword:00000000  
“Status”=dword:00000001  
“Address”=dword:ffffffff  
“Vid”=”{137E7700-3573-11CF-AE69-08002B2E1262}”

Use the following values for Mode and Vid:

| View | Mode | Vid |
|---|---|---|
| Icons | 1 | {0057D0E0-3573-11CF-AE69-08002B2E1262} |
| List | 3 | {0E1FA5E0-3573-11CF-AE69-08002B2E1262} |
| Details | 4 | {137E7700-3573-11CF-AE69-08002B2E1262} |
| Thumbnail | 5 | {8BEBB290-52D0-11D0-B7F4-00C04FD706EC} |
| Tiles | 6 | {65F125E5-7BE1-4810-BA9D-D271C8432CE3} |
| Filmstrip | 7 | {8EEFA624-D1E9-445B-94B7-74FBCE2EA11A} |

Or you can also set it into HKEY\_LOCAL\_MACHINE to make it effective for every user on the system.