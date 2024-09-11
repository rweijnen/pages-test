---
id: 642
title: 'silly issue with DHCP reservation on Netgear WNDR3700 router'
date: '2010-09-23T16:32:44+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=642'
permalink: /2010/09/23/silly-issue-with-dhcp-reservation-on-netgear-wndr3700-router/
views:
    - '9352'
short-url:
    - 'http://bit.ly/gu8VpE'
categories:
    - General
---

I recently bought a new router, a Netgear WNDR3700 and I noticed a silly bug. When you make a DHCP reservation in the webinterface it will present a list of existing clients which makes it easy to add them to the reservation list.

[![NetGearAddAddress](http://192.168.40.25:8081/wp-content/uploads/2010/09/netgearaddaddress-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/09/netgearaddaddress.png)

This seems to work fine at first but when I checked my IP with IPConfig I noticed that I received a different address. I checked the router’s log and saw this:

> \[DHCP IP: 192.168.2.29\] to MAC address 00:21:5c:9a:a0:99, Thursday, September 23,2010 14:03:59

I noticed that the MAC address was lowercase here and not uppercase as in the webinterface. I changed the case in the webinterface and then it worked fine!

I still think it’s a good router though, especially the WAN speed througputs are very good:

[![SpeedTest](http://192.168.40.25:8081/wp-content/uploads/2010/09/speedtest-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/09/speedtest.png)