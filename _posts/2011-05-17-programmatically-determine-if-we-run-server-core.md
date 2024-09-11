---
id: 1748
title: 'Programmatically determine if we run server core'
date: '2011-05-17T21:32:22+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/05/17/programmatically-determine-if-we-run-server-core/'
permalink: /2011/05/17/programmatically-determine-if-we-run-server-core/
views:
    - '3736'
short-url:
    - 'http://bit.ly/ii6Kg5'
categories:
    - Delphi
    - 'Windows 2008 R2'
    - 'Windows 7'
    - 'Windows Internals'
---

If you want to check if you are running on a Server Core edition of Windows you can use the [GetProductInfo](http://msdn.microsoft.com/en-us/library/ms724358.aspx) API.

GetProductInfo takes 4 input parameters that can be obtained using [GetVersionEx](http://msdn.microsoft.com/en-us/library/ms724451(VS.85).aspx) and the [OSVERSIONINFOEX](http://msdn.microsoft.com/en-us/library/ms724833(v=VS.85).aspx) structure:

 “&gt;

No we call GetProductInfo:

“&gt;

The value returned in dwProdType can be checked against one of the constants for the Server Core editions:

“&gt;

So if put this all together we can write a function for it:

“&gt;

But where do these values come from?

GetProductInfo is exported by Kernel32.dll, this function is a stub to RtlGetProductInfo which is exported by ntdll.dll

RtlGetProductInfo performs a few checks on the input parameters and then calls the undocumented NtQueryLicenseValue API which has this signature:

“&gt;

The value of the Name parameter on my Windows 7 machine is *Kernel-ProductInfo*, the ulType parameter is SL\_DATA\_DWORD (4), the buffer is a PDWORD with Length being SizeOf(DWORD).

If the call to NtQueryLicenseValue fails, a special value of 0xABCDABCDu is returned which equals PRODUCT\_UNLICENSED.

You can directly check this value with the *Licensing Demo* from my [Having fun with Windows Licensing](http://192.168.40.25:8081/2010/06/15/having-fun-with-windows-licensing/) article:

[![SNAGHTML6063f2](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML6063f2_thumb.png "SNAGHTML6063f2")](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML6063f2.png)