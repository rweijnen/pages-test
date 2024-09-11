---
id: 2195
title: 'Determining if Battery Backed Write Cache is installed'
date: '2011-11-15T16:27:42+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/11/15/determining-if-battery-backed-write-cache-is-installed/'
permalink: /2011/11/15/determining-if-battery-backed-write-cache-is-installed/
views:
    - '13628'
short-url:
    - 'http://bit.ly/tMQmXp'
categories:
    - General
tags:
    - BBWC
    - Hewlett-Packard
    - HP
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image8.png)I wanted to know if a certain server had a [Battery Backed Write Cache module](http://192.168.40.25:8081/2011/05/02/extremely-slow-virtual-machines-on-hp-smart-array-p410/) (BBWC) on it’s array controller.

I suspected it did not, but I had to be sure. Since this server was running production I couldn’t open (Visual Inspection) or reboot it.

The server didn’t have Insight Agents installed so I couldn’t query it via iLO or the Insight Agents webpage either.

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image9.png)I knew that when you do a full install of the array controller bundled software it comes with a commandline tool, hpacucli.exe.

I decided to see if I could find out with this tool, so I went to this [HP Smart Array P410i Controller download page](http://h20000.www2.hp.com/bizsupport/TechSupport/SoftwareIndex.jsp?lang=en&cc=us&prodNameId=3902575&prodTypeId=329290&prodSeriesId=3902574&swLang=8&taskId=135&swEnvOID=1005#78214).

From the “Software – System Management” section I downloaded the “[HP Proliant Array Configuration Utility for Windows](ftp://ftp.hp.com/pub/softlib2/software1/sc-windows/p414707561/v65755/cp014264.exe)“:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image10.png)

Then I extracted the files to a temporary location:

[![SNAGHTML5d7de99](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML5d7de99_thumb.png "SNAGHTML5d7de99")](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML5d7de99.png)

Inside was an MSI file, HPACUCLI875120.msi. I extracted the files from this MSI:

 “&gt;

This extracts the tool and some dll’s to *MyFolder\\Program Files\\Compaq\\Hpacucli\\Bin*.

I copied these files to the server and ran hpacucli.exe. The *ctrl all show config* command can be used to show all controllers (in my case just one):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image11.png)

Controller details can be queried using “*ctrl slot=X show*“:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image12.png)

The Battery Count is 0 so we have no BBWC. If you do have a BBWC the following lines are shown (the read/write ration may differ of course):

“&gt;