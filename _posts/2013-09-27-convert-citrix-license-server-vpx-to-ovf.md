---
id: 3397
title: 'Convert Citrix License Server VPX to OVF'
date: '2013-09-27T16:41:54+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3397'
permalink: /2013/09/27/convert-citrix-license-server-vpx-to-ovf/
views:
    - '5221'
short-url:
    - 'http://bit.ly/1eMyZcj'
    - 'http://bit.ly/1eMyZcj'
categories:
    - Citrix
tags:
    - Citrix
    - Hyper-V
    - OVF
    - Xen
---

I wanted to run a virtual Citrix License server in my LAB.

Unfortunately Citrix only provides the [VPX License Server](http://www.citrix.com/downloads/licensing/license-server/license-server-vpx-version-1110.html) in XenServer format (.xva). If you want to run the VPX on VMware ESX or Microsoft Hyper-V you need to convert it first.

The option to convert a Xen Virtual Appliance to OVF format was removed in XenConvert 2.4.1. So for a conversion you need version 2.3.1.

Here are the direct download links:

- <font color="#35383d">[XenConvert 2.3.1 (Windows 32-bit)](http://downloadns.citrix.com.edgesuite.net/akdlm/5320/XenConvert_Install.exe)</font>
- <font color="#35383d">[XenConvert 2.3.1 (Windows 64-bit)](http://downloadns.citrix.com.edgesuite.net/akdlm/5322/XenConvert_Install_x64.exe)</font>

However when I tried to convert the downloaded VPX (Citrix\_License\_Server\_VPX\_v11.10.0\_Build\_12002.xva) I got the error "Failed to decode tar header record":

[![Failed to decode tar header record](http://192.168.40.25:8081/wp-content/uploads/2013/09/SNAGHTML490f56b6_thumb.png "Citrix XenConvert 2.3.1")](http://192.168.40.25:8081/wp-content/uploads/2013/09/SNAGHTML490f56b6.png)

I inspected the downloaded xva file with a Hex Editor:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/09/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/09/image2.png)

I recognized the ZIP header signature so I renamed the file to .zip and unpacked it. Inside the ZIP was another xva with the same name (Citrix\_License\_Server\_VPX\_v11.10.0\_Build\_12002.xva).

This file ran successfully through the converter:

[![Conversion was successful!](http://192.168.40.25:8081/wp-content/uploads/2013/09/SNAGHTML49271c75_thumb.png "Citrix XenConvert 2.3.1")](http://192.168.40.25:8081/wp-content/uploads/2013/09/SNAGHTML49271c75.png)

I created a VM in Hyper-V from the VHD that XenConvert produced, one problem remains however: the VPX tries to boot with a Xen-Dom0 kernel which fails to boot on Hyper-V:

[![Error 13: invalid or unsupported executable format](http://192.168.40.25:8081/wp-content/uploads/2013/09/image_thumb3.png "kernel /vmlinuz-2.6.18-274.7.1.el5xen")](http://192.168.40.25:8081/wp-content/uploads/2013/09/image3.png)

I don’t have a fix for that issue (yet), so this might be continued…