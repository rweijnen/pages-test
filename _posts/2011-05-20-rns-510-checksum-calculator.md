---
id: 1790
title: 'RNS 510 Checksum Calculator'
date: '2011-05-20T16:42:27+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1790'
permalink: /2011/05/20/rns-510-checksum-calculator/
short-url:
    - 'http://bit.ly/l2JNUX'
views:
    - '6000'
categories:
    - Embedded
tags:
    - Checksum
    - RNS510
    - Volkswagen
---

About a year ago I wrote a checksum calculator for the Volkswagen RNS 510 navigation unit.

The checksum is a variant on Crc16 and is present either at the end of certain files (eg #CRC16:db1d) or as a special file with the name CRC.16.

The tool is a little exe that takes the filename as the only parameter.

[![SNAGHTML1424ee0](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML1424ee0_thumb.png "SNAGHTML1424ee0")](http://192.168.40.25:8081/wp-content/uploads/2011/05/SNAGHTML1424ee0.png)

Have fun with it!

[ RNS 510 Checksum Calculator (3946 downloads ) ](http://192.168.40.25:8081/download/rns-510-checksum-calculator/?tmstv=1726048919 "Version 1.0")