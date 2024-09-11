---
id: 2270
title: 'Error 372 in Thinapped Visual Basic Application'
date: '2012-01-03T22:34:29+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/01/03/error-372-in-thinapped-visual-basic-application/'
permalink: /2012/01/03/error-372-in-thinapped-visual-basic-application/
views:
    - '2615'
short-url:
    - 'http://bit.ly/yHrfx5'
categories:
    - Packaging
tags:
    - ThinApp
    - 'Visual Basic'
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image.png)Today I was troubleshooting a Thinapped Visual Basic Application. The application halts with a Run-time error ‘372’ when trying to run a report:

[![Run-time error 372 Failed to load control 'CrystalActiveReportViewer' from crviewer.dll. Your version of crviewer.dll may be outdated. Make sure you are using the control that was provided with your application](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb1.png "Proaz")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image1.png)

Microsoft KB article [kb942870](http://support.microsoft.com/kb/942870) hints to an ActiveX component that is registered into HKCU instead of HKLM.

A trace with the ThinApp Log Monitor reveals that the application is looking for an ActiveX component under HKCU:

“&gt;

A registry trace shows that the component (Crystal Reports ActiveX Report Viewer) registers itself to HKLM but also creates a few keys under HKCU that are required to make the viewer run.

I decided to register the component from a VBScript inside the ThinApp package since this is easier and more fool proof than capturing all the registry keys and adding them to the package.ini.

The VBS Script simply calls RegSvr32.exe with the /s (silent) parameter:

“&gt;

Just save the VBS script with a name of choice in the same folder as the package.ini file.