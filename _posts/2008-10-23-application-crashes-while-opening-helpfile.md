---
id: 118
title: 'Application crashes while opening helpfile'
date: '2008-10-23T13:48:35+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/10/23/application-crashes-while-opening-helpfile/'
permalink: /2008/10/23/application-crashes-while-opening-helpfile/
views:
    - '10239'
short-url:
    - 'http://bit.ly/hTBNlH'
categories:
    - General
---

I had an application that crashed when opening the Help Topics entry from the Help menu. A trace with Process Monitor showed that it was opening a help (.chm) file. Using Explorer I could normally open the Helpfile so thas was strange. Process Monitor did not reveil any ACCESS\_DENIED or other problems.

I did notice that ieframe.dll was being accessed several times and some further debugging revealed that a dll in the application directory was loaded (psapi.dll). This is strange because psapi.dll resided in the windows\\system32 folder normally. Also the copy in the application directory was an old version (4.0.1371.1).

I renamed this dll to .old and restarted the application so that it loaded the version out of the system32 folder and then the help opened fine!