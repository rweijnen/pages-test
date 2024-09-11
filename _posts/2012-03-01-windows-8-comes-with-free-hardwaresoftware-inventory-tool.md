---
id: 2490
title: 'Windows 8 comes with free hardware/software inventory tool'
date: '2012-03-01T11:26:44+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/03/01/windows-8-comes-with-free-hardwaresoftware-inventory-tool/'
permalink: /2012/03/01/windows-8-comes-with-free-hardwaresoftware-inventory-tool/
views:
    - '5817'
short-url:
    - 'http://bit.ly/xJmEsL'
categories:
    - 'Windows 8'
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image.png)Yesterday I wrote about the Windows 8 CP [WebSetup installer](http://192.168.40.25:8081/2012/02/29/undocumented-commandline-switches-for-windows-8-cp/) and told you that the Application Compatibility check creates a few XML files in the folder [%AppData%\\Local\\Microsoft\\WebSetup\\Panther](file:///%UserProfile%/AppData/Local/Microsoft/WebSetup/Panther).

So what can we do with them?

The XML files are created by a separate exe in the WebsetupExpanded folder called WicaInventory.exe with the arguments: /apps /fast /ext “exe,sys” /output &lt;XML file&gt; /log &lt;LOG file&gt;

In my case the commandline was:

 “&gt;

Let’s see what’s in WICA\_Programs\_REMKOLAPTOP.xml.

I opened it in Excel as an XML Table and the result is a nice inventory of the software on my machine:

[![SNAGHTML2f934c5](http://192.168.40.25:8081/wp-content/uploads/2012/03/SNAGHTML2f934c5_thumb.png "SNAGHTML2f934c5")](http://192.168.40.25:8081/wp-content/uploads/2012/03/SNAGHTML2f934c5.png)

It contains ao applications, version, publisher, how it was installed, path to the msi etc etc.

The file WICA\_Devices\_REMKOLAPTOP.xml contains a detailed overview of the hardware and drivers:

[![SNAGHTML2fbb930](http://192.168.40.25:8081/wp-content/uploads/2012/03/SNAGHTML2fbb930_thumb.png "SNAGHTML2fbb930")](http://192.168.40.25:8081/wp-content/uploads/2012/03/SNAGHTML2fbb930.png)

Not sure what the EULA says but this looks like a nice, free, hardware and software inventory tool!