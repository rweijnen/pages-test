---
id: 3111
title: 'MSCOMM32.OCX returns error 80040112'
date: '2013-03-13T15:58:39+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3111'
permalink: /2013/03/13/mscomm32-ocx-returns-error-80040112/
views:
    - '3470'
short-url:
    - 'http://bit.ly/ZzEVJp'
    - 'http://bit.ly/ZzEVJp'
categories:
    - General
    - Programming
    - script
---

[Yesterday](http://192.168.40.25:8081/2013/03/11/the-case-of-the-com-port-redirection/) I wrote about troubleshooting an application that used Com Port redirection in Citrix.

During the troubleshoot I noticed that the application used an ActiveX component, MSCOMM32.OCX, for serial communication.

I wanted to quickly test if the component was correctly registered so I searched the registry from HKEY\_CLASSES\_ROOT for mscomm32.ocx.

This confirmed that the component was registered:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb21.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image21.png)

My next check was to confirm if the control was actually working with a VB Script. For that I needed the ProgID from the same registry key:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb22.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image22.png)

The VBS code is then simply:

 “&gt;

Executing the script failed with error 80040112. I looked up this error code and it stands for CLASS\_E\_NOTLICENSED.

A google search lead me to this [kb article](http://support.microsoft.com/kb/192693) which explains that it’s a license issue that only occurs when you create the control at run-time.

At this point I wasn’t sure if my problem was related to

I decided to open MSCOMM32.OCX in Ida Pro to see where this error comes from. I searched in the Strings Tab for the word Licenses which brought up this part of the code:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb23.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image23.png)

So a registry key under HKEY\_CLASSES\_ROOT\\Licenses is checked. I opened sub\_21C118D3 and set a breakpoint on RegQueryValueA. The debugger shows us the contents of lpString and lpString2 when we hover the mouse over:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb24.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image24.png)

and lpString2:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb25.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image25.png)

So by creating the key HKEY\_CLASSES\_ROOT\\Licenses\\4250E830-6AC2-11cf-8ADB-00AA00C00905 and set it’s default value to kjljvjjjoquqmjjjvpqqkqmqykypoqjquoun we will pass the license check.

“&gt;  
After creating the registry key, the vbs script worked (it didn’t make the application work though but more about that in [yesterday’s](http://192.168.40.25:8081/2013/03/11/the-case-of-the-com-port-redirection/) post).