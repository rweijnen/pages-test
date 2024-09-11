---
id: 372
title: 'Dell Systems Build and Update Utility DVD'
date: '2009-06-15T16:58:17+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/06/15/dell-systems-build-and-update-utility-dvd/'
permalink: /2009/06/15/dell-systems-build-and-update-utility-dvd/
views:
    - '8631'
short-url:
    - 'http://bit.ly/eLoKpT'
categories:
    - General
---

I was installing Dell Deployment Solution (the Dell branded version of Altiris) and at end of the installation you can choose to add drivers for scripted installed. If you do it asks for the Dell Systems Build and Update Utility DVD in order to install drivers for scripted install:

[![Dell1](http://192.168.40.25:8081/wp-content/uploads/2009/06/dell1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/06/dell1.png)

I searched on the Dell site for this DVD but the only version I could find was a version of 20-11-2008 (5.5.1 AA00) with the filename OM\_5.5.1\_SUU\_A00.ISO and it is not accepted by the installer:

![HelpfullErrorMsg](http://192.168.40.25:8081/wp-content/uploads/2009/06/helpfullerrormsg.png)

What a helpfull message 😉

After some trial &amp; error I found at the Dell uses 2 versions called SUU and SBUU so next I searched on SBUU and found some old versions on the Dell site but by searching on the filename OM\_x.y.z\_SBUU\_Axx and incrementing the versions I found a more recent version on [this](http://ftp2.us.dell.com/secure/sysman/) dell ftp link. The most recent version I found was OM\_5.5.0\_SBUU\_A00.ISO from 08-10-2008 so I think there must be newer versions! If anyone has found it please let me know…

[OM\_5.5.0\_SBUU\_A00.ISO](http://ftp2.us.dell.com/secure/sysman/OM_5.5.0_SBUU_A00.ISO "OM_5.5.0_SBUU_A00.ISO")

/Edit: this [post](http://www.delltechcenter.com/page/2%2F8%2F2008+-+OpenManage+Download+-+Comments "OpenManage Download - Comments") explains Dell’s abbreviations