---
id: 3363
title: 'RD Gateway connection fails on Windows 7'
date: '2013-08-15T11:27:15+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3363'
permalink: /2013/08/15/rd-gateway-connection-fails-on-windows-7/
views:
    - '4573'
short-url:
    - 'http://bit.ly/16dBRsC'
    - 'http://bit.ly/16dBRsC'
categories:
    - 'Terminal Server'
    - 'Windows 2012'
    - 'Windows 7'
tags:
    - RDGateWay
    - RDS
    - Server2012
---

I needed to connect remotely via Remote Desktop to a Windows Server 2012 machine.

I received an rdp file that was configured to use an RD Gateway server:

[![Remtoe Desktop Connection | RD Gateway Server Settings](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML243e76_thumb.png "RD Gateway Server Settings")](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML243e76.png)

However when trying to connect from my Windows 7 laptop (x64) machine, I got the following error message:

[![The two computers couldn't connect in the amount of time allocated. Try connecting again. If the problem continues, contact your network administrator or technical support.](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML2c46b0_thumb.png "Remote Desktop Connection")](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML2c46b0.png)

Followed by this message when I tried to connect again:

[![Your computer can't connect to the remote computer because an error occurred on the remote computer that you want to connect to. Contact your network administrator for assistance.](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML2c148a_thumb.png "Remote Desktop Connection")](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML2c148a.png)

After some troubleshooting I decided to test the connection from a virtual machine and to my surprise it worked fine there.

Knowing that I was able to connect to other machines with Remote Desktop from my laptop without problems there had to be a difference.

I compared the versions of mstsc.exe and mstscax.dll and noticed that the version of my laptop was newer:

| [![file version 6.2.9200.16398](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML318e4b_thumb.png "mstsc.exe Properties")](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML318e4b.png) | [![6.1.7600.16385](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb9.png "mstsc.exe Properties")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image11.png) |
|---|---|

I copied mstsc.exe and mstscax.dll from the virtual machine to my laptop in a separate folder. When starting the copied mstsc.exe I got an error message about missing mui (multi language) files:

[![The system cannot find the file specified | <LANG_NAME>\mstsc.exe.MUI](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML34a0e1_thumb.png "Error")](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML34a0e1.png)

To solve this I created a subfolder en-US and copied the files mstsc.exe.mui and mstscax.dll.mui from the virtual machine.

Mstsc now launched but a connection using the specified RDP file yielded yet another error:

[![You have specified to use a gateway server, however this feature is not supported by your system configuration.](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML36aaa1_thumb.png "Remote Desktop Connection")](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML36aaa1.png)

I found the error message string in mstsc.exe.mui with a resource id of 13349:

[![SNAGHTML3866be](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML3866be_thumb.png "SNAGHTML3866be")](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML3866be.png)

I opened mstsc.exe in Ida Pro and searched for 13349 but no hits. I then searched on 3425 which is the hex value of 13349 and then I got a hit in a function called *CContainerWnd::StartConnection:*

[![CContainerWnd::StartConnection](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb10.png "CContainerWnd::StartConnection")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image12.png)

So if IsProxySupportEnabled returns FALSE, this message is displayed. The IsProxySupportEnabled function doesn’t do much, it just loads a DLL named AALibrary:

[![IsProxySupportEnabled](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb11.png "IsProxySupportEnabled")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image13.png)

Clicking on in the code showed some traceoutput that identifies AALibrary as aaclient.dll:

[![RegQueryExValue for aaclient.dll failed](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb12.png "aaclient.dll")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image14.png)

I copied over aaclient.dll to the same folder as mstsc.exe and aaclient.dll.mui to the en-US subfolder and finally it worked!

For you convenience I have attached a zipped copy of the mstsc files.

[ mstsc.exe build 6.1.7600.16385 (43201 downloads ) ](http://192.168.40.25:8081/download/mstsc-exe-build-6-1-7600-16385/?tmstv=1726048920 "Version 6.1.7600.16385")