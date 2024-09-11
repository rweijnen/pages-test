---
id: 764
title: 'WMI query to Win32_Product returns error 0x80041010'
date: '2010-11-12T15:57:07+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=764'
permalink: /2010/11/12/wmi-query-to-win32_product-returns-error-0x80041010/
views:
    - '4030'
short-url:
    - 'http://bit.ly/gUQBvk'
categories:
    - Altiris
    - script
---

I ran a VBScript that queries the Win32\_Product WMI class on Windows 2003 but it returned error 0x80041010 instead of the expected results.

I looked up that errorcode and it means WBEM\_E\_INVALID\_CLASS. This happened because the “WMI Windows Installer Provider” was not installed.

You can do this through Add/Remove Programs | Windows Components | Management and Monitoring Tools | WMI Windows Installer Provider.

Or if you want to do this with a script you can use SysocMgr:  
“&gt;