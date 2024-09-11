---
id: 2368
title: 'Harmony Client crashes upon exit'
date: '2012-01-31T16:37:08+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/01/31/harmony-client/'
permalink: /2012/01/31/harmony-client/
views:
    - '1287'
short-url:
    - 'http://bit.ly/w9VMd6'
categories:
    - Citrix
    - Packaging
    - 'Windows 2003'
tags:
    - ThinApp
---

Today I was troubleshooting the application “Harmony Client” which crashed upon exiting:

[![Toepassingspop-up: HARMONY_Client.exe - Toepassingsfout : De instructie op 0x77e621b6 verwijst naar geheugen op 0x4b750000. Een lees- of schrijfbewerking op het geheugen is mislukt: | The memory could not be read.](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb21.png "HARMONY_Client.exe - Toepassingsfout")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image21.png)

The application had been thinapped and the error only appeared when starting the thinapped version.

Using Process Monitor I noticed that the application wrote logfiles to the C:\\Temp\\HarmonyClient\\Log folder (which of course it shouldn’t write there.).

 “&gt;

My next step was to trace using the Thinapp Log Monitor but unfortunately the error doesn’t occur when tracing.

This makes me believe it’s a timing issue; upon exit the application is cleaning up memory (objects) but destroys a certain object (eventhandler) while it’s still being accessed.

Upon further inspection of the Process Monitor log I noticed access to a file called HARMONY\_Client.exe.9e3c5c50.ini.inuse which was in the ThinApp SandBox (*%Local AppData%\\ApplicationHistory\\HARMONY\_Client.exe.9e3c5c50.ini.inuse*).

The application creates this file when it’s started and when it exists it copies this to *HARMONY\_Client.exe.9e3c5c50.ini*.

I deleted this file and this made the error go away. Perhaps this fault will surface again in the future, further testing will need to tell us.

**<u>Workaround  
   
</u>**Delete \*.inuse from the SandBox, preferably using a (vbs) script in the thinapp package each time the app is launched.

**<u>Solution  
   
</u>**Contact Vendor and get them to fix their software and while they’re at it, have them fix access to things such as C:\\Temp as well.