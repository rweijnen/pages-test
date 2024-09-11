---
id: 242
title: 'Accessing kernel objects in other sessions part 2'
date: '2009-02-03T14:36:53+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/02/03/accessing-kernel-objects-in-other-sessions-part-2/'
permalink: /2009/02/03/accessing-kernel-objects-in-other-sessions-part-2/
views:
    - '2686'
short-url:
    - 'http://bit.ly/dSHyLk'
categories:
    - Delphi
    - General
    - Programming
---

In [part 1](/blog/2009/01/27/accessing-kernel-objects-in-other-sessions/) I showed how to create and open objects in Terminal Server Sessions. However, these are not all of the possible places where you can place objects via documented kernel32.dll functions.

If we look into Winobj again, we notice, that every **BaseNamedObjects** directory has a subdirectory named **Restricted**. To be honest, I do not know why it’s created; it’s security allows object creation for *LocalSystem* and *RESTRICTED* special user (in windows 2000, *Everyone* can also create objects in it). So, we can use it as prefix for object creation, for example, **Restricted\\MyAppEvent**: [![RestrictedObject](http://192.168.40.25:8081/wp-content/uploads/2009/02/restrictedobject-small.gif)](http://192.168.40.25:8081/wp-content/uploads/2009/02/restrictedobject.gif)

Of course, you can still use **Global**, **Local**, or **Session** links for accessing objects in **Restricted** directory, e.g. **Global\\Restricted\\Objname**, **Session\\6\\Restricted\\Objname**. You can always create objects in **Global\\Restricted** directory, while may fail creating objects in session’s **Restricted** directory.

What if you will use **Session** link, but do not add the session number to it (like **Session\\MyAppEvent**)? It is allowed, and you’ll have your object created into the **\\Sessions\\BNOLINKS** directory: [![BNOLinksObject](http://192.168.40.25:8081/wp-content/uploads/2009/02/bnolinksobject-small.gif)](http://192.168.40.25:8081/wp-content/uploads/2009/02/bnolinksobject.gif)

So if you want to hide your object, but still share it with someone else, you can use it.

Download new SessionObjects version 1.1: it creates objects in all possible places.

[ Session Objects 1.1 (1138 downloads ) ](http://192.168.40.25:8081/download/session-objects-1-1/?tmstv=1726048918 "Version 1.1")