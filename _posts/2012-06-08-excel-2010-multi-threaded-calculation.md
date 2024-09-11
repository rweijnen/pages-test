---
id: 2642
title: 'Excel 2010 multi-threaded calculation'
date: '2012-06-08T13:21:19+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2642'
permalink: /2012/06/08/excel-2010-multi-threaded-calculation/
short-url:
    - 'http://bit.ly/JRybzi'
views:
    - '19033'
categories:
    - General
tags:
    - Citrix
    - Excel
    - 'Excel 2010'
    - Office
    - 'Office 2010'
    - XenApp
---

[![Excel 2007 Icon](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb10.png "Excel 2007 Icon")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image10.png)I was just browsing through the Options tab in Excel 2010 when I noticed the following setting:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/06/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/06/image2.png)

This feature was introduced in Excel 2007.   
In the default settings, multi-threaded calculation is Enabled with "Use all processors on this computer".

On a physical desktop this would be the preferred setting since it will make formula calculation as fast as possible.

However on shared environments such as Citrix XenApp, Microsoft RDS and VDI this seems like a very bad setting since one user can use the full processing capacity and hinder other users.

![](http://farm1.staticflickr.com/113/282707058_02305d3cce_z.jpg?zz=1)Unfortunately changes to this setting are not stored in the registry or filesystem. So even when you change it through the GUI the settings are not retained when you close Excel. This sounds like a BUG in Excel.

As a workaround we can change this setting using VBA via the [Application.MultiThreadedCalculation](http://msdn.microsoft.com/en-us/library/ff835198.aspx) property.

To fix it permanently I placed the code in an .XLAM (Excel Add-In) and placed in in the Excel startup folder.

“&gt;

That fixes it:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/06/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/06/image3.png)

For your convenience you can download the Excel Add in: [ DisableMultiThreading.zip (3676 downloads ) ](http://192.168.40.25:8081/download/disablemultithreading-zip/?tmstv=1726048919 "Version 1.0")