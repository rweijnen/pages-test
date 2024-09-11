---
id: 2841
title: 'Return username instead of computername to Applications'
date: '2012-12-06T10:45:40+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2841'
permalink: /2012/12/06/return-username-instead-of-computername-to-applications/
views:
    - '2582'
short-url:
    - 'http://bit.ly/VoBTVX'
categories:
    - Citrix
    - 'Terminal Server'
    - 'Windows 2003'
    - 'Windows 2008'
    - 'Windows 2008 R2'
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image8.png)Some applications use the computer’s name as a unique identifier, rather than using the user name. In a single-user-per-computer environment, this strategy works well.

However, in a Multi User environment such as Citrix XenApp or Microsoft’s Remote Desktop Services (Terminal Server), all connected users report the same computername.

If the application relies on unique computernames to handle tasks such as file and record locking, then the application will fail.

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image9.png)We can however set an [Application Compatibility Flag](http://support.microsoft.com/default.aspx?scid=kb;en-us;186499) in the registry to return the username instead of the computername.

To demonstrate this behaviour I wrote a small Test Application called TestAppCompatFlags.exe.

Without the compatibility flags the application will return the Computername and Username when pressing the buttons:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image10.png)

When you press the Set Compatibility Flag button the following registry key will be created:

 “&gt;

In this key a DWORD value Flags will be set to 18 HEX.

You need to restart the test application to test the new behaviour:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image11.png)

[ TestAppCompatFlags.zip (1997 downloads ) ](http://192.168.40.25:8081/download/testappcompatflags-zip/?tmstv=1726048920 "Version 1.0")