---
id: 2461
title: 'Where to download TraceView'
date: '2012-02-22T13:57:29+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/02/22/where-to-download-traceview/'
permalink: /2012/02/22/where-to-download-traceview/
views:
    - '6391'
short-url:
    - 'http://bit.ly/AtW4wW'
categories:
    - Citrix
---

[![TraceView Icon](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb19.png "TraceView Icon")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image19.png)Various Citrix knowledge base articles refer to a tool called TraceView.exe to view the output of diagnostic traces.

[CTX106233](http://support.citrix.com/article/CTX106233) describes where to download traceview but this article is outdated because it describes an older version of the DDK (the Windows Driver Development).

The current DDK version (7.1.0) can be downloaded [here](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=11800) and has the filename “GRMWDK\_EN\_7600\_1.ISO”.

To install TraceView, launch the DDK installer (KitSetup.exe) and select Tools

[![Microsoft Windows Driver Kit: 7.1.0.7600 | Tools](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML148e0d73_thumb.png "Microsoft Windows Driver Kit: 7.1.0.7600")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML148e0d73.png)

Or directly install the tracingtool\_x86.msi or tracingtool\_x64.msi from the WDK directory.

The msi installs traceview.exe in “c:\\WinDDK\\7600.16385.win7\_wdk.100208-1538\\tools\\tracing\\i386\\traceview.exe”