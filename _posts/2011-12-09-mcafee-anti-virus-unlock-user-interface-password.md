---
id: 2231
title: 'McAfee Anti Virus Unlock User Interface Password'
date: '2011-12-09T15:50:12+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/12/09/mcafee-anti-virus-unlock-user-interface-password/'
permalink: /2011/12/09/mcafee-anti-virus-unlock-user-interface-password/
views:
    - '18533'
short-url:
    - 'http://bit.ly/rXYMtG'
categories:
    - General
tags:
    - McAfee
    - Password
    - VirusScan
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/12/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/12/image2.png)I needed to change a few settings on a McAfee VirusScan Enterprise 8.7.Oi client. However there was a password protection in place that locks the user interface and nobody around that could tell me the password. So what to do?

Right, we check out where this password is stored and how we can get rid of it!

I openend vsplugin.dll in Ida Pro and searched for related strings such as password, lock etc.

I found out that vsplugin.dll calls some interesting exports in shutil.dll called *UIP*, UiLockInfoLoad1 and UiLockInfoValidate1.

I searched further in shutil.dll and concluded that the password is stored in the UIP value under *HKLM\\Software\\McAfee\\DesktopProtection* registry key.

The value is an [MD5 Hash](http://en.wikipedia.org/wiki/MD5) of the password so we cannot decrypt it. So I tried one of the many MD5 hash databases on the net but I couldn’t find the original string for the password.

This happens because the MD5 hash tables are all based on [ASCII](http://en.wikipedia.org/wiki/ASCII) and McAfee uses a [Unicode](http://en.wikipedia.org/wiki/Unicode) string type. Eg the word password would be stored like this in hex:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/12/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/12/image3.png)

We can of course build our own Unicode hash/rainbow table but an easier solution is to write a known MD5 hash to this registry value. For instance the MD5 hash for the Unicode string password is: *b081dbe85e1ec3ffc3d4e7d0227400cd*.

Another bypass would be to delete the UIP value and set the UIPMode to 0 which disables the user interface password completely. Note that when an EPO server is in place it will always periodically overwrite the settings with those defined in it’s policies.

I’ve also seen that shutil.dll exports a function called UiLockInfoValidate1. This functions checks if the given MD5 hash exists in the Global Atom Table in the from UIP-&lt;hash&gt;.

The Atom seems to be used as a Single Sign on for the user interface unlock. Once you’ve entered the password in one module, it will also unlock the other modules. Possibly we could abuse this system by writing our own hash to the Global Atom Table but I didn’t investigate this further.