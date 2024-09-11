---
id: 3620
title: 'Upload ovf/ova to vCloud Director with PowerShell'
date: '2015-10-14T10:08:49+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3620'
permalink: /2015/10/14/upload-ovfova-to-vcloud-director-with-powershell/
short-url:
    - 'http://bit.ly/1Psz4Dr'
views:
    - '1743'
categories:
    - PowerShell
    - VMWare
tags:
    - OVA
    - OVF
    - ovftool
    - PowerShell
    - 'vCloud Director'
    - VMWare
---

Recently support for NPAPI has been [removed](https://support.google.com/chrome/answer/6213033?hl=en) from Google Chrome. While understandable from a security point of view it does mean that some plugins no longer work.

A good example is VMware’s Client Integration Plugin where we’ve lost the ability to upload an ovf template. While VMware has published a fix for vCenter (see [this kb](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2130672)), it has not been fixed for vCloud Director:

[![The attempted operation cannot be performed using this browser. Re-try using an alternative method:

\- Use the VMware OVF Tool to perform the operation. You can download the OVF Tool and its User Guide from the OVF Tool product page at https://www.vmware.com/support/developer/ovf/.
\- Use a browser and platform combination that is supported by vCloud Director for this operation. For the supported browser and platform combinations, see the Release Notes for this version of vCloud Director.](http://192.168.40.25:8081/wp-content/uploads/2015/10/image_thumb7.png "Browser Compatiblity Warning")](http://192.168.40.25:8081/wp-content/uploads/2015/10/image7.png)

To upload the ovf you need ovf tool (as the warning indicates), this comes with the integration plugin, on my machine it’s located in `c:\\Program Files (x86)\\VMware\\Client Integration Plug-in 6.0\\ovftool.exe`.

William Lam’s [blog article](https://blogs.vmware.com/vsphere/2013/05/how-to-import-an-ova-into-vcloud-director.html) describes the procedure with ovftool with the following example commandline:

“&gt;

It’s a bit troublesome to form the vcloud locator as we need to escape special characters such as spaces and combine all the arguments with &amp; characters.

So I created a PowerShell script that wraps ovftool and makes this all nice and easy for you. Let’s see how it works!

If you don’t specify parameters, the script will ask you for Catalog, Computername, Organization and Credentials:

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/10/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/10/image8.png)

The script has a hardcoded path to ovftool but if it cannot find it will popup a browse dialog:

[![image](http://192.168.40.25:8081/wp-content/uploads/2015/10/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2015/10/image9.png)

[![Download PowerShell script to upload OVA/OVF to vCloud Director or vCloud Air](http:///192.168.40.25:8081/wp-content/uploads/2014/07/downloadbutton.gif?resize=120%2C36 "Download")](https://remkoweijnen.sharefile.eu/d-s3fb364c571e42e59)