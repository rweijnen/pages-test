---
id: 348
title: 'The messaging interface has returned an unknown error'
date: '2009-06-12T12:18:09+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/06/12/the-messaging-interface-has-returned-an-unknown-error/'
permalink: /2009/06/12/the-messaging-interface-has-returned-an-unknown-error/
views:
    - '2915'
short-url:
    - 'http://bit.ly/g184e4'
categories:
    - script
---

Today someone asked questions about a script I wrote back in 2007 to solve a bug in Outlook (2003 but at least Outlook XP has the same issue). If you have access to someone’s calendar and want to make a print of it Outlook wants to print it in it’s default view which is a combined view on calendar appointments and tasks. However if you do not have permissions to the other persons tasks folder Outlook refuses to print and displays the following error: **The messaging interface has returned an unknown error. If the problem persists, restart Outlook.**

To resolve it you can go to the Calendar | Daily View | Print, then click Page Setup and under Include Options deselect Taskpad. I didn’t want to do this for all users that’s why I wrote the script.

The settings is stored in a file called OutlPrnt which is stored in %appdata%\\Microsoft\\Outlook. I changed the setting through the GUI and watched the changes to that file, changed it back in watched the changes again. That’s how I discovered that this setting is stored at offset 0xE10 (dec. 3600) with the following possible values that correspond to the GUI options

[![PageSetupDailyStyle](http://192.168.40.25:8081/wp-content/uploads/2009/06/pagesetupdailystyle-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/06/pagesetupdailystyle.png)

Taskpad = 4  
Notes area (blank) = 1  
Notes area (lined) = 2

So to change this is a simple exercise in VBS, just use the FileSystemObject to read the file with the OpenTextFile method, skip to offset &amp;HE10 and change it to the desired value. The script can be downloaded below.

[ Outlprnt.vbs (1066 downloads ) ](http://192.168.40.25:8081/download/outlprnt-vbs/?tmstv=1726048918 "Version 1.0")