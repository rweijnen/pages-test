---
id: 3380
title: 'The publisher could not be verified when launching an application with RES Workspace Manager'
date: '2013-09-13T15:35:25+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3380'
permalink: /2013/09/13/the-publisher-could-not-be-verified-when-launching-an-application-with-res-workspace-manger/
views:
    - '2290'
short-url:
    - 'http://bit.ly/19OA7qv'
    - 'http://bit.ly/19OA7qv'
categories:
    - RES
    - Vista
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 2012'
    - 'Windows 7'
    - 'Windows 8'
tags:
    - RES
    - 'Workspace Manager'
---

Today I was troubleshooting a warning message that popped up when launching a network application with RES Workspace Manager:

[![The publisher could not be verified. Are you sure you want to run this software?](http://192.168.40.25:8081/wp-content/uploads/2013/09/image_thumb.png "Open file - Security Warning")](http://192.168.40.25:8081/wp-content/uploads/2013/09/image.png)

Usually this is a simple fix: add the servername (<file://server>) to the Local Intranet zone:

[![You can add and remove websites from this zone. All websites in this zone will use the zone's security settings.](http://192.168.40.25:8081/wp-content/uploads/2013/09/clip_image002_thumb.jpg "Local intranet")](http://192.168.40.25:8081/wp-content/uploads/2013/09/clip_image002.jpg)

That worked when I launched the application directly. However when launching the application with RES Workspace Manager I would still get the warning. Even stranger: when I clicked Cancel the application would still be launched.

This problem seems related to the way Workspace Manager launches an applications: the program’s path is replaced with %respfdir%\\pwrgate.exe &lt;appid&gt;.

A trace with Process Monitor showed that the ZoneMap was not even checked. That leaves us with two options to fix it:

**<u>Option #1:</u> Change Security Settings for the Internet Zone** An easy way to get rid of this message is to allow Launching applications and unsafe files in the Internet Security Zone:

[![Launching applications and unsafe file (not secure)](http://192.168.40.25:8081/wp-content/uploads/2013/09/clip_image0025_thumb1.jpg "Security Settings - Internet Zone")](http://192.168.40.25:8081/wp-content/uploads/2013/09/clip_image0025.jpg)

Internet Explorer warns us right away that this is a bad idea:

[![Are you sure you want to change the settings for this zone? | The current security settings will put your computer at risk.](http://192.168.40.25:8081/wp-content/uploads/2013/09/2013_09_13_15_14_45_Desktop_spvw9050_Citrix_Receiver_thumb.png "Warning!")](http://192.168.40.25:8081/wp-content/uploads/2013/09/2013_09_13_15_14_45_Desktop_spvw9050_Citrix_Receiver.png)

**<u>Option #2:</u> Change Attachment Manager Policy**   
This option is less worse than option #1 but another solution is preferred. As a workaround it will do though.

Open the Managed Application and create a new User Registry Policy under Configuration. Import *AttachmentManager.admx* (usually in C:\\Windows\\PolicyDefintions).

Include .exe in the "Inclusion list for moderate risk file types":

[![Attachment Manager | Inclusion list for moderate risk file types](http://192.168.40.25:8081/wp-content/uploads/2013/09/clip_image0027_thumb.jpg "User Registry Policy")](http://192.168.40.25:8081/wp-content/uploads/2013/09/clip_image0027.jpg)