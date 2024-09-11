---
id: 483
title: 'Getting Back the Classic Event Viewer in Vista and Windows 7'
date: '2009-11-26T13:15:55+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/11/26/getting-back-the-classic-event-viewer-in-vista-and-windows-7/'
permalink: /2009/11/26/getting-back-the-classic-event-viewer-in-vista-and-windows-7/
views:
    - '7156'
short-url:
    - 'http://bit.ly/fWHBlY'
categories:
    - Vista
    - 'Windows 2008'
    - 'Windows 7'
---

I never liked the new eventviewer that was introduced with Windows Vista. If you want to have the old eventviewer back (you can use the old and new one together) you need to follow the following steps:

1. Open a command prompt as Adminstrator.
2. Type Regsvr32 els.dll (if you get error code 0x80070005 then you didn’t run as Administrator).
3. Start mmc.exe and goto File | Add/Remove Snapin.
4. From the available Snapins choose “Classic Event Viewer”.
5. Right-Click Classic Event Viewer under Console Root and select New Window from Here.
6. Choose Customize from the View menu.
7. Deselect the Action Pane and Click OK
8. Now save the file with a name of your choice eg EventVwrC.msc.

It should look like this:

[![EventVwrC](http://192.168.40.25:8081/wp-content/uploads/2009/11/eventvwrc-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/eventvwrc.png)

Doubleclicking an Event feels familiar as well:

[![EventProps](http://192.168.40.25:8081/wp-content/uploads/2009/11/eventprops-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/eventprops.png)