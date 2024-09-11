---
id: 931
title: 'Citrix Online Plugin could not launch the requested published application'
date: '2010-12-22T14:10:31+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/12/22/citrix-online-plugin-could-not-launch-the-requested-published-application/'
permalink: /2010/12/22/citrix-online-plugin-could-not-launch-the-requested-published-application/
views:
    - '11297'
short-url:
    - 'http://bit.ly/g8yIuV'
categories:
    - Altiris
    - Citrix
---

After doing an unattended installation of the Citrix Online Plugin it was not possible to launch a Published Application.

It would just give the error mesage: “citrix online plugin could not launch the requested published application”.

Even though the installation finished without errors and the logfiles indicated no failure at all I was able to fix it by using 2 steps described in [CTX123761](http://support.citrix.com/article/CTX123761 "Offline Plug-in 11.2 Unattended Installation Fails").

First install the [Microsoft Visual C++ 2005 Redistributable Package SP1](http://www.microsoft.com/downloads/details.aspx?FamilyID=200b2fd9-ae1a-4a14-984d-389c36f85647&displaylang=en "Microsoft Visual C++ 2005 Redistributable Package SP1") and the (re)deploy the Online Plugin adding a Transform called transform\_notstrict.mst which can be found in the aforementioned KB Article.