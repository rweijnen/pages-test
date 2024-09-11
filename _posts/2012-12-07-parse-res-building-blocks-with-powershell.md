---
id: 2850
title: 'Parse RES Building Blocks with PowerShell'
date: '2012-12-07T13:29:20+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2850'
permalink: /2012/12/07/parse-res-building-blocks-with-powershell/
short-url:
    - 'http://bit.ly/TJjG9B'
views:
    - '2163'
categories:
    - PowerShell
    - RES
tags:
    - PowerShell
    - RES
    - 'Workspace Manager'
    - XML
---

**<span style="text-decoration: underline;">Background  
</span>**Customer uses Citrix XenApp 5 with ThinApp, RES Workspace Manager and RES Workspace Extender.

An application integration strategy is defined, the picture below displays the strategy and preferred order:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image12.png)

**<span style="text-decoration: underline;">Question</span>**  
Customer wanted to know the type (1..7) for all applications currently defined in RES Workspace Manager.

I decided to export all the Applications from RES WM as Building Blocks. This results in a folder with XML files. I decided to parse the XML files with a PowerShell script.

The scripts display a progress bar while running, indicating the current Building Block and percentage complete:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image13.png)

When the script is finished a Save File Dialog pops up so you can save the CSV file:

[![SNAGHTML1406aa0d](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML1406aa0d_thumb.png "SNAGHTML1406aa0d")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML1406aa0d.png)

And the resulting Excel Sheet:

[![SNAGHTML140a746d](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML140a746d_thumb.png "SNAGHTML140a746d")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML140a746d.png)

The Script:  
“&gt;  
[ ParseBuildingBlocks.zip (1988 downloads ) ](http://192.168.40.25:8081/download/parsebuildingblocks-zip/?tmstv=1726048920 "Version 1.0")