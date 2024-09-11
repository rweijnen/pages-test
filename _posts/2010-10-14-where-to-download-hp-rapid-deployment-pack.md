---
id: 655
title: 'Where to download HP Rapid Deployment Pack'
date: '2010-10-14T11:11:24+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=655'
permalink: /2010/10/14/where-to-download-hp-rapid-deployment-pack/
views:
    - '6585'
short-url:
    - 'http://bit.ly/eDvadf'
categories:
    - Altiris
---

I have started the implementation of a VMWare vShphere environment in which we are going to use an Altiris server for Deployment. Since we are using HP Hardware I needed the HP Branded version of Altiris (it’s called HP Rapid Deployment Pack aka HP RDP).

I had some troubles finding the download spot for it, that’s why I am sharing it here. Hopefully it will save others from a long search!

Last time I needed HP RDP I just went to the HP website, clicked Support &amp; Drivers then selected Download drivers and software and in the Editbox entered “HP Rapid Deployment Pack” which leads to this page:

[![HPRDP1](http://192.168.40.25:8081/wp-content/uploads/2010/10/hprdp1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/10/hprdp1.png)

So where do we download it then?

After some searching it turns out that there is no seperate download anymore for HP RDP but you need to download the HP Insight Software Media DVD. At this time the most recent version is 6.2 – October 2010 and it can be found [here](https://h20392.www2.hp.com/portal/swdepot/displayProductInfo.do?productNumber=ISDVD).

You need an HP Passport to be able to access that page, then click Receive for free and <span style="text-decoration: underline;">select the Double Layer ISO image</span> because that’s the only one that contains the RDP software. The download is almost 7,5 GB (!) so a fast internet connection is needed.

Also note that the name has changed from HP Rapid Deployment Pack to HP Insight Control server deployment. If you search on that product name you will find no downloads but it’s page is [here](http://h18000.www1.hp.com/products/servers/management/deployment-migration.html).

And for extra convenience here are the links to the prereqisuites:

- [WAIK for Vista/2008](http://www.microsoft.com/downloads/en/details.aspx?familyid=94bb6e34-d890-4932-81a5-5b50c657de08&displaylang=en&tm)
- [SQL 2008 (Express)](http://www.microsoft.com/sqlserver/2008/en/us/default.aspx)