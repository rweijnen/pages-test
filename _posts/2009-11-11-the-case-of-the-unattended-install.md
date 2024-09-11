---
id: 444
title: 'The case of the Unattended Install'
date: '2009-11-11T17:47:28+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/11/11/the-case-of-the-unattended-install/'
permalink: /2009/11/11/the-case-of-the-unattended-install/
views:
    - '4220'
short-url:
    - 'http://bit.ly/hWDbWR'
categories:
    - Uncategorized
---

I needed to do an unattended install of an application (in this case Exact Globe 2003) on Citrix. In this case the application provides a special executable, WSetup.exe for an unattended install.

WSetup takes several parameters, the most important ones are:

> /I: Installation Path  
> /S: Install Type  
> /IM: Installation Mode

So this appeared to be an easy task, however when testing the Deployment the /I parameter seemed to be ignored and the whole thing was installed in C:\\Program Files.

I analyzed what WSetup.exe does with my favorite tool, Ida Pro and at first all seemed ok. WSetup determines the default location of the Program Files Directory by reading the ProgramFilesDir registry key (which is of course C:\\Program Files).

Please note: if you are considering to change this key on your environment please note that Microsoft [does not support](http://support.microsoft.com/kb/933700) systems where this key has been changed!

But then something strange happens in the code: the CreateProcess API is called and it launches a copy of itsself (copied to the %temp% dir) under a different name. If we look at the commandline parameters it’s suddenly clear why the /I parameter didnt work:

> C:\\Users\\rweijnen\\AppData\\Local\\Temp\\einstall.exe” /I:”D:\\Apps\\Exact Software” /S:2 /OS:9 /I:”C:\\Program Files (x86)\\Exact Software”

Do you see? A second /I parameter has been added!

My next step was to look in the code for einstall and I noticed that when the executable is named einstall.exe or newinstall.exe the commandline is unchanged.

The OS parameter is determined by calling GetVersionEx, if the version is 5.2 (XP /2003) OS is set to 8, if it’s 6.x it’s set to 9 (note that the current code will fail if there’s a Windows version that returns 7 or higher, bug bug…)

So the trick for getting this application installed is to make a copy of WSetup.exe and name it EInstall.exe or NewInstall.exe.

Case solved!