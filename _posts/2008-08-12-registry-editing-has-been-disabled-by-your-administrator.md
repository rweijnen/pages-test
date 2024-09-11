---
id: 110
title: 'Registry editing has been disabled by your administrator (not anymore!)'
date: '2008-08-12T23:09:32+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/08/12/registry-editing-has-been-disabled-by-your-administrator/'
permalink: /2008/08/12/registry-editing-has-been-disabled-by-your-administrator/
views:
    - '8716'
short-url:
    - 'http://bit.ly/dYxEO4'
categories:
    - 'Active Directory'
    - General
    - Vista
    - 'Windows 2008'
tags:
    - LinkedIn
---

Most administrator will want to prevent normal users from opening Regedit and a command prompt. Usually this is done by activating the “Prevent access to registry editing tools” and “Prevent access to the command prompt” policy settings. They are located under User Configuration | Administrative Templates | System:

![gpedit](http://192.168.40.25:8081/wp-content/uploads/2008/08/gpedit.png)

Activating the policies will set the matching keys in the registry:

![regkey](http://192.168.40.25:8081/wp-content/uploads/2008/08/regkey.png)

If we try to open regedit we are denied access:

![regedit1](http://192.168.40.25:8081/wp-content/uploads/2008/08/regedit1.png)

So how does this work? Actually regedit contains some code that checks these registry values and if the DWORD value is 1 access is denied.

Sometimes you (the Administrator) want to check a specific registry setting when the user has a problem. We can offcourse do this by starting regedit with elevated permissions and browse to the user’s keys under HKEY\_USERS. But this is inconvenient especially since the user’s registry is not shown under his/her username but the SID:

![HKEYUSERS](http://192.168.40.25:8081/wp-content/uploads/2008/08/hkeyusers-1.png)

Wouldn’t it be nice to have a patched regedit.exe and a patched cmd.exe that ignore these policies?

Here’s your chance: [ Patched Regedit and Cmd prompt (2742 downloads ) ](http://192.168.40.25:8081/download/patched-regedit-and-cmd-prompt/?tmstv=1726048918 "Version 1.0")