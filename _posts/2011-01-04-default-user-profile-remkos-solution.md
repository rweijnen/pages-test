---
id: 1066
title: 'Default User Profile: Remko’s solution'
date: '2011-01-04T15:46:18+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1066'
permalink: /2011/01/04/default-user-profile-remkos-solution/
short-url:
    - 'http://bit.ly/dN4qU9'
views:
    - '31618'
categories:
    - Altiris
    - Citrix
    - 'Terminal Server'
    - 'Windows 2003'
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 7'
    - 'Windows XP'
---

If you are implementing a Citrix, Terminal Server or even just a plain Client-Server environment you will need to create a Default User Profile at some point.

The Default User Profile can be thought of as the initial registry settings that are used when a new profile is created.

Many people think that the Default User Profile is available in regedit via HKEY\_USERS\\.Default but this is NOT the Default User Profile.

[![UsersDefault](http://192.168.40.25:8081/wp-content/uploads/2011/01/usersdefault-small.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/usersdefault.png)

The .Default key is loaded before you logon, so it’s usefull to save the language and keyboard settings that apply before logging on. Many OEM vendors also set a background image here to set a “logon wallpaper”.

The location of the Default User Profile is set in the registry under HKLM\\Software\\Microsoft\\Windows NT\\CurrentVersion\\ProfileList:

[![ProfileList](http://192.168.40.25:8081/wp-content/uploads/2011/01/profilelist-small.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/profilelist.png)

Note that on Windows Vista and higher OS the Default key containts the full path but on Windows 2003/XP you need to assemble the path by combining the values of ProfilesDirectory and DefaultUserProfile:

[![ProfileList2003XP](http://192.168.40.25:8081/wp-content/uploads/2011/01/profilelist2003xp-small.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/profilelist2003xp.png)

But how do we *create* a Default User Profile?

The traditional method is that you logon as a certain user, make you changes and the copy the profile using the Copy To button in the User Profiles page of the Advanced System Properties:

![UserProfilesCopyTo](http://192.168.40.25:8081/wp-content/uploads/2011/01/userprofilescopyto.png)

However this is a lot of manual work that you need to do all over again if you go to a new OS of even another environment (especially if you move on from project to project like me).

Note that you can of course *copy* the Default User Profile from one machine to another but you can’t keep track of changes or easily see the contents.

That’s why I use another way: I create .reg files with all the Default Settings I need.

I use a little know feature of reg files: *you can add comments using the semi colon (;)*

You can also deletee keys or values in .reg files using the minus sign (-).

In the .reg file I use HKEY\_USERS\\#DefUser to reference HKEY\_CURRENT\_USER.

A typical Default User Profile .reg file looks like this:  
“&gt;  
I have created a vbscript that mounts the Default User Profile (under HKEY\_USERS\\#DefUser), imports the regfile (given as parameter) and unmounts the key again.

Script and sample .reg file are available in the download below!

[ DefUser.zip (1963 downloads ) ](http://192.168.40.25:8081/download/defuser-zip/?tmstv=1726048919 "Version v1.0")