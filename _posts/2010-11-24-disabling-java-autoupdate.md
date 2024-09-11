---
id: 804
title: 'Disabling Java Autoupdate'
date: '2010-11-24T16:07:28+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/11/24/disabling-java-autoupdate/'
permalink: /2010/11/24/disabling-java-autoupdate/
views:
    - '2262'
short-url:
    - 'http://bit.ly/fUypwT'
categories:
    - Altiris
    - General
    - Java
---

I did an unattended install of Java Jre1.6\_0.22 using an mst file that puts all (auto)update properties to off.

However it seems that Java simply ignores this, so usually a script runs after the install to the delete some registry keys and perform some extra configuration.

With more recent Java versions it’s easier to simply uninstall the update component with the following commandline:

> msiexec /x {4A03706F-666A-4037-7777-5F2748764D10} /qb-