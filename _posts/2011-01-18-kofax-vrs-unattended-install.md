---
id: 1222
title: 'Kofax VRS Unattended Install'
date: '2011-01-18T14:53:39+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1222'
permalink: /2011/01/18/kofax-vrs-unattended-install/
views:
    - '6136'
short-url:
    - 'http://bit.ly/eocuD0'
categories:
    - Altiris
    - Packaging
    - 'Unattended Installation'
---

Recently I needed to create an Unattended Install for an application that uses a piece of software (for scanning) called [Kofax VRS](http://www.kofax.com/vrs-virtualrescan/).

This Kofax software comes with an .msi file but there was no documentation on the install options.

In fact it didn’t seem like the Vendor anticipated on an Unatttended Install.

I browsed in the msi file using [Microsoft’s Orca tool](http://msdn.microsoft.com/en-us/library/aa370557(v=vs.85).aspx) and tried some of the properties I found in the public properties table.

The VRSMODE property relates to the options in the first dialog:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image.png)

The CONFIGSCANNERLATER2 property corresponds to this dialog (there is also a CONFIGSCANNERLATER property but it seems to have no effect):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image1.png)

So my complete install line was:  
“&gt;