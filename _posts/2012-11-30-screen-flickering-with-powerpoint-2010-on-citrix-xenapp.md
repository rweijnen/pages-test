---
id: 2782
title: 'Screen flickering with PowerPoint 2010 on Citrix XenApp'
date: '2012-11-30T14:18:22+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2782'
permalink: /2012/11/30/screen-flickering-with-powerpoint-2010-on-citrix-xenapp/
views:
    - '4913'
short-url:
    - 'http://bit.ly/Sk5T6T'
categories:
    - Citrix
    - 'Windows 2003'
tags:
    - Citrix
    - PowerPoint
    - PowerPoint2010
    - 'Screen Flicker'
    - WPF
    - XenApp
---

Within half an hour of writing the article "[Application causes Screen Flickering in Citrix XenApp Session](http://192.168.40.25:8081/2012/11/30/application-causes-screen-flickering-in-citrix-xenapp-session/)" I got a message that the hotfix in that article also fixes a similar problem in PowerPoint 2010.

Office 2010 uses hardware acceleration for displaying images and when this is enabled (which is the default) you will see constant screen flicker when you try to display a presentation with Images on Citrix XenApp (Server 2003):

<div class="wlWriterEditableSmartContent" id="scid:5737277B-5D6D-4f48-ABFC-DD9C333F4C5D:1baf0443-1822-4d90-8c1b-cf6059f5de34" style="padding-bottom: 0px; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; float: none; padding-top: 0px"><div><object height="252" width="448"><param name="movie" value="http://www.youtube.com/v/uhWNwQQUZ3o?hl=en&hd=1"></param></object></div><div style="width:448px;clear:both;font-size:.8em">Screen Flickering when running WPF Applications on Citrix XenApp</div></div>The issue is fixed by installing the Hotfix mention in my [previous article](http://192.168.40.25:8081/2012/11/30/application-causes-screen-flickering-in-citrix-xenapp-session/). As a workaround you can disable hardware acceleration in PowerPoint via File | Options | Advanced | Display | Disable hardware graphics acceleration:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/11/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/11/image3.png)

Or via the registry: set the DWORD **DisableHardware** to 1 in **HKEY\_CURRENT\_USER\\Software\\Microsoft\\Office\\14.0\\Gfx**