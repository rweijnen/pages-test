---
id: 2743
title: 'Citrix License Server Crash'
date: '2012-08-23T15:29:40+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2743'
permalink: /2012/08/23/citrix-license-server-crash/
views:
    - '2486'
short-url:
    - 'http://bit.ly/O9CbkB'
categories:
    - Citrix
tags:
    - Citrix
    - 'Licensing Server'
---

After updating a Citrix License server from 11.6.1 to 11.10 the Citrix Licensing Service crashed immediately after startup.

In the Event Log the following error was shown:

[![Application Error | Faulting application lmadmin.exe, version 11.10.0.9, faulting module msvcp80.dll, version 8.0.50727.6195, fault address 0x000038db](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb12.png "Event ID 1000 | Category 100 | Citrix Licensing Service")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image12.png)

I suspected that there was a corrupt licensing file in the MyFiles folder (Default C:\\Program Files\\Citrix\\Licensing\\MyFiles).

Therefore I renemaed the MyFiles folder and the service didn’t crash anymore. Then I tried to Import the license files manually one by one and then I got this error:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image13.png)

However the file was already copied to the MyFiles folder:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image14.png)

So after a restart the service would crash!

I compared the corrupted file with a working file, a "normal" license files looks similar to this one:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image15.png)

The one that made the Licensing Server crash looked like this:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb16.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image16.png)

I think that someone saved some html instead of a license from My Citrix. After removing the invalid license the service didn’t crash anymore.

PS: Shame on Citrix for writing such bad code!