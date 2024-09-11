---
id: 3402
title: 'Error 1603 when installing Microsoft App-V 5.0 SP1 client'
date: '2013-10-23T12:48:54+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3402'
permalink: /2013/10/23/error-1603-when-installing-microsoft-app-v-5-0-sp1-client/
views:
    - '1598'
short-url:
    - 'http://bit.ly/1a6XkI5'
    - 'http://bit.ly/1a6XkI5'
categories:
    - App-V
tags:
    - App-V
    - 'Error 1603'
    - MSI
---

I wanted to do an unattended install of the Microsoft App-V 5.0 SP1 client.

I wanted to install using the MSI’s instead of using the exe installer so I unpacked the MSI’s from the installer as documented [here](http://blogs.technet.com/b/gladiatormsft/archive/2013/04/24/mdop-2013-embeds-app-v-msi-s-inside-of-exe-files.aspx).

The install failed however with MSI error 1603. I activated logging but that was not very helpful since it only logged "*MainEngineThread is returning 1603*".

Manual install of the MSI gave a bettor error message:

[![Microsoft Application Virtualization (App-V) Client 5.0 Service Pack 1 x86 requries Microsoft Visual C++ 2005 Redistributable (x86) with minimum version 8.0.61001](http://192.168.40.25:8081/wp-content/uploads/2013/10/SNAGHTML8b0098e_thumb.png "Microsoft Application Virtualization (App-V) Client 5.0")](http://192.168.40.25:8081/wp-content/uploads/2013/10/SNAGHTML8b0098e.png)

I had already installed the MSVC++ 2005 SP1 runtime but the version was slightly lower.

Unfortunately Microsoft doesn’t publish the build numbers with their downloads so it takes some searching to determine the correct download.

Version 8.0.61001 is labeled as "*Microsoft Visual C++ 2005 Service Pack 1 Redistributable Package MFC Security Update*" and can be downloaded [here](http://www.microsoft.com/en-us/download/details.aspx?id=26347).

There is a similar requirement for the Microsoft Visual C++ 2010 runtime which should be at least 10.0.40219. This one is easier though because the required version is extracted together with the MSI files.

As a final note you need to set the **AcceptEULA MSI property to 1** for both the client and language pack MSI or the install will fail.