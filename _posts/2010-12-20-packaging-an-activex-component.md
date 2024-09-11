---
id: 908
title: 'Packaging an ActiveX Component: Easy?'
date: '2010-12-20T20:19:18+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=908'
permalink: /2010/12/20/packaging-an-activex-component/
views:
    - '2236'
short-url:
    - 'http://bit.ly/fNGted'
categories:
    - Packaging
    - 'Strange Error'
    - 'Unattended Installation'
---

Today I needed to package an applications that I was told was very simple. If I hear something like that my first thought is always “hmmm”.

So I prepared a machine to build the package and followed the instructions which were to go to a specific URL and download the package (probably an ActiveX control from there).

I opened the URL and immediately got an error message:

![The specified LICD is not available](http://192.168.40.25:8081/wp-content/uploads/2010/12/lciderror.png)

The error message 0x80004005 is not very helpfull since it stands for E\_FAIL (winerror.h).

However I knew that LCID stands for [LocaleID](http://msdn.microsoft.com/en-us/goglobal/bb964664 "Local IDs Assigned by Microsoft") so I figured the error might have something to do with language settings.

So I went to the Control Panel | Regional and Language Options and set the location to English (United States):

![Regional and Language Options](http://192.168.40.25:8081/wp-content/uploads/2010/12/regionallanguageoptions.png)

So that solved the first issue since I now got a login screen and after Login the expected ActiveX control appeared:

[![ActiveXControl](http://192.168.40.25:8081/wp-content/uploads/2010/12/activexcontrol-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/activexcontrol.png)

Having dealt with similar controls before it was easy, just accept the install and goto C:\\Windows\\Downloaded Program Files:

[![Downloaded Program Files Folder](http://192.168.40.25:8081/wp-content/uploads/2010/12/downloadededprogramfiles-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/downloadededprogramfiles.png)

And there is our Control! If you watch this directory from a Command Prompt you can see the actual control:

[![DownloadededProgramFiles2](http://192.168.40.25:8081/wp-content/uploads/2010/12/downloadededprogramfiles2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010-12/downloadededprogramfiles2.png)

The rest is easy: just copy the file to a user accessible folder (%systemroot%\\system32 seems most appropriate) and register it with RegSvr32 XUpload.ocx or silently with RegSvr32 /s XUpload.ocx