---
id: 3338
title: 'Magic Filter Preview'
date: '2013-08-10T16:34:26+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3338'
permalink: /2013/08/10/magic-filter-preview/
views:
    - '1355'
short-url:
    - 'http://bit.ly/15nqZV3'
    - 'http://bit.ly/15nqZV3'
categories:
    - ThinKiosk
tags:
    - Citrix
    - MagicFilter
    - RDS
    - ThinKiosk
    - VMWare
---

In Enterprise environments users are often working on a remote (virtual) desktop such as when using SBC or VDI.

They typically get a full screen session, perhaps on a thin client, and have not idea that they are using a remote desktop.

**<u>The Problem</u>**   
[![image](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image10.png)However when they press Ctrl-Alt-Delete they get either the local Security Attention Screen / Task Manager or nothing at all if it has been blocked.

Clever users know they can use alternative key combinations such as *Shift-F2* for Citrix or *Ctrl-Alt-End* for RDS.

But that’s not the seamless experience we want to give our users, is it?

**<u>Magic Filter</u>**   
That’s why I have been working with [Andrew Morgan](http://andrewmorgan.ie/) on a new feature called Magic Filter. Magic Filter will be included in the upcoming [ThinKiosk 4.0](http://andrewmorgan.ie/2013/05/23/thinkiosk-4-0-preview-and-feature-teaser/) release.

**<u>How does it work?</u>**   
Magic Filter intercepts special key strokes, such as *Ctrl-Alt-Delete* and *Windows L,* on the local machine and passes them on to a remote session.

Magic Filter is **not** a (filter) driver but just a user mode process running with user privileges.

**<u>What is supported?</u>**   
Magic Filter supports the following Operating Systems:

- Windows XP
- Windows Vista
- Windows 7
- Windows 8 (not RT)
- Windows Server 2003
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012.

The following clients are currently supported:

- Citrix Online Plugin
- Citrix Receiver
- VMware View Client

Although we are already testing with the Remote Desktop Connection client (mstsc.exe) it is not yet included in this release.

**<u>The Preview</u>**

<div class="wlWriterEditableSmartContent" id="scid:5737277B-5D6D-4f48-ABFC-DD9C333F4C5D:5cdaa96d-7fe6-41b1-aa08-ab702a006794" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px"><div><object height="252" width="448"><param name="movie" value="http://www.youtube.com/v/D49Tnbl_yS8?hl=en&hd=1"></param></object></div><div style="width:448px;clear:both;font-size:.8em">Magic Filter Preview</div></div>After I released the Preview [Anton van Pelt](https://twitter.com/antonvanpelt) asked why I used Windows Server 2003 for the demo. I did that to emphasize that we are not only supporting the newer OS versions but also older versions. As I reply to Anton I made a second video showing that it works on Windows NT 4 and Server 2012 just as well!

<div class="wlWriterEditableSmartContent" id="scid:5737277B-5D6D-4f48-ABFC-DD9C333F4C5D:f015225f-46d4-49f0-83da-66b8a5e207bf" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px"><div><object height="252" width="448"><param name="movie" value="http://www.youtube.com/v/Ie5JnP16qZs?hl=en&hd=1"></param></object></div><div style="width:448px;clear:both;font-size:.8em">Magic Filter on Windows NT and Server 2012</div></div>