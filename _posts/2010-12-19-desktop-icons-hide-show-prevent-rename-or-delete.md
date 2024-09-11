---
id: 896
title: 'Desktop Icons, hide, show, prevent rename or delete'
date: '2010-12-19T15:14:04+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=896'
permalink: /2010/12/19/desktop-icons-hide-show-prevent-rename-or-delete/
views:
    - '4778'
short-url:
    - 'http://bit.ly/emUQYe'
categories:
    - Vista
    - 'Windows 2008'
    - 'Windows 7'
---

I was cleaning up some old data on my Hard Drive when I found a program I wrote about a year ago.

At that time I was doing a project where I was deploying a Windows 2008 based Citrix Environment.

I wanted to get rid of the new Personal Folders or User’s files icon on the Desktop and replace it with the familiar My Documents icon.

![Personal](http://192.168.40.25:8081/wp-content/uploads/2010/12/personal.png)

These settings are stored in the Registry under HKEY\_CLASSES\_ROOT\\CLSID\\{folder’s GUID}\\ShellFolder.

More specifially the Attributes Value is a bitmask that determines if the Folder

- <div>can be Copied</div>
- <div>can be Deleted</div>
- <div>can be Linked to</div>
- <div>can be Moved</div>
- <div>can be Renamed</div>
- <div>is Hidden</div>
- <div>can be Dropped files to</div>

So I wrote a small program that can adjust these values:

[![DesktopIconModifier](http://192.168.40.25:8081/wp-content/uploads/2010/12/desktopiconmodifier-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/desktopiconmodifier.png)

It contains a few pre-defined Folders and shows you the registry key and the value for the Attributes mask (and of course it can write the value for you).

Have fun with it and let me know if you like it!

[ Desktop Icons Tool (1925 downloads ) ](http://192.168.40.25:8081/download/desktop-icons-tool/?tmstv=1726048919 "Version 1.4")