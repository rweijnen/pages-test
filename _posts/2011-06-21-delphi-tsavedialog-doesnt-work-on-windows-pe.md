---
id: 1892
title: 'Delphi TSaveDialog doesn&rsquo;t work on Windows PE'
date: '2011-06-21T15:03:07+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/06/21/delphi-tsavedialog-doesnt-work-on-windows-pe/'
permalink: /2011/06/21/delphi-tsavedialog-doesnt-work-on-windows-pe/
views:
    - '7324'
short-url:
    - 'http://bit.ly/kbSvMl'
categories:
    - Delphi
tags:
    - Delphi
    - TSaveDialog
    - 'Windows PE'
---

In my [SATA Controller Identification tool](http://192.168.40.25:8081/2011/06/18/switch-sata-operation-mode/) I was using the TSaveDialog (Delphi 2010) but I got a report that under Windows PE the dialog is never shown.

There’s no exception and I didn’t really bother to check why it fails. Instead I decided to replace it with the [GetSaveFileName](http://msdn.microsoft.com/en-us/library/ms646928(v=vs.85).aspx) API which does work under Windows PE.

Example:  
“&gt;