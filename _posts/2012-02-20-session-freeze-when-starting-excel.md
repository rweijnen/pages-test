---
id: 2450
title: 'Session freeze when starting Excel'
date: '2012-02-20T15:42:25+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/02/20/session-freeze-when-starting-excel/'
permalink: /2012/02/20/session-freeze-when-starting-excel/
views:
    - '3590'
short-url:
    - 'http://bit.ly/AFtscs'
categories:
    - Citrix
    - 'Windows 2003'
tags:
    - Citrix
    - Excel
    - McAfee
    - Office
    - XenApp
---

**Environment**   
Windows 2003 Enterprise (32 bit), Citrix XenApp 5, RES Workspace Manager 2011, McAfee VirusScan Enterprise 8.7.0i.

**Problem**   
When a opening an Excel workbook from Sharepoint the whole session freezes.

I asked the user to open an Excel workbook from Sharepoint and I noticed the following popup:

[![Some files can harm your computer. If the file information looks suspicious or you do not fully trust the source, do not open the file | You are opening the following file: | File name: My Workbook.xls | From: Sharepoint](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb7.png "Message from webpage")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image7.png)

So my first thought was that the user somehow clicked this message to the background and IE was waiting for a response.

This message appears because the option “Confirm open after download” is active in the file type options:

[![Microsoft Word-document | Confirm open after download](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb8.png "Edit File Type")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image8.png)

I knew this setting was stored in the registry but couldn’t remember where, so I used Process Monitor to quickly determine it’s location. I set a filter on “Process is explorer.exe” and “Operation is RegSetValue”:

[![Process is explorer.exe | Operation is RegSetValue](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb9.png "Process Monitor Filter")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image9.png)

First line is a direct hit, the registry key is “HKCR\\Excel.Sheet.8\\EditFlags”:

[![Output](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb10.png "Process Monitor")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image10.png)

HKEY\_CLASSES\_ROOT is actually a merged view of HKEY\_LOCAL\_MACHINE\\Software\\Classes and HKEY\_CURRENT\_USER\\Software\\Classes so you can set this value either per user or for all users.

In my case I set this value in RES Workspace Manager as an Application Specific Configuration:

[![RES Workspace Manager | Composition | Applications | Managed Applications | Microsoft Office 2003 | Excel | Configurations | Actions](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb11.png "Change registry settings")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image11.png)

After this change the warning wasn’t shown anymore but the workbook still didn’t open.

**Freeze** After a little wait Explorer didn’t respond any more, which in the user’s experience makes the whole session freeze. The RES Dialog was indicating it was still launching Excel:

[![Desktop | Bezig met starten van "Microsoft Excel 2003"](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb12.png "Desktop")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image12.png)

**Process Monitor** So I opened Process Monitor to see what was going on and I noticed a recurring sequence of several NotifyChangeDirectory operations from Explorer. I filtered on this operation:

[![Operation is NotifyChangeDirectory](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb13.png "Process Monitor")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image13.png)

As you can see in the screenshot there is a massive number of entries, each taking up about a second. And after waiting several minutes Excel was launched.

I then tried to open Excel directly and this showed the same behavior.

**Stack** So I doubleclicked one of the entries and inspected the Stack. As you can see the last 6 operations are related to virus scanning because fltMgr.sys is the File Filter Filter Manager (virus scanners usually use a filter driver):

[![Stack View](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb14.png "Process Monitor")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image14.png)

And mfehidk.sys is a driver from McAfee:

[![mfehidk.sys | McAfee Link Driver](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb15.png "Module Properties")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image15.png)

**Virus Scanner** To see whether the Virus Scanner was at fault, I deselected “When reading from disk” (for performance reasons it’s I would recommended to turn this off anyway) from the Scan Items in the On-Access Scan Properties:

[![McAfee On-Access Scan Properties | Default processes | Scan Items | When reading from disk](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb16.png "On-Access Scan Properties")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image16.png)

After that change Excel launched almost immediately!

I am not really satisfied with this workaround so I will ask the customer to report this to McAfee. I will update this article if I get any feedback.