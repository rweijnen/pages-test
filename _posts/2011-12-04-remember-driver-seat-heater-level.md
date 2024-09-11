---
id: 2226
title: 'Remember Driver Seat Heater Level'
date: '2011-12-04T12:39:26+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/12/04/remember-driver-seat-heater-level/'
permalink: /2011/12/04/remember-driver-seat-heater-level/
views:
    - '2854'
short-url:
    - 'http://bit.ly/uZr7Ao'
categories:
    - Automotive
tags:
    - Passat
    - 'Seat Heater'
    - VCDS
---

![Passat - B7](http://www.kufatec.de/shop/images/categories/380.png)On a friend’s car, a Passat B7 (3AA) the seat heaters didn’t work. So I connected to the car with VCDS to see if we could troubleshoot this.

I opened Control Module 9, Central Electronics:

[![VCDS | Select Control Module | 09 Central Electronics](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb0b435f_thumb.png "VCDS | Select Control Module")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb0b435f.png)

Then went to 7 Long Coding, and it turned out that the Seat Heating was not properly coded.

VCDS didn’t have a label file for this module but it should be Byte 22, Bit 2:

[![Central Electronics Coding | Byte 22 | Bit 2 | Seat Heating off](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb0726fa_thumb.png "VDS | Central Electronics | Long Coding")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb0726fa.png)

I changed it to:

[![Central Electronics Coding | Byte 22 | Bit 2 | Seat Heating on](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb07f6b6_thumb.png "VDS | Central Electronics | Long Coding")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb07f6b6.png)

And now the seat heaters were working again.

But the driver wondered why the seat heater level wasn’t remembered. I am not sure why this isn’t activated by default but it can be easily changed.

Go to Control Module 8, Auto HVAC:

[![VCDS | Select Control Module | 08 Auto HVAC](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb0c0815_thumb.png "VCDS | Select Control Module")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb0c0815.png)

Then go to Adaptation, select channel “Storage of driver seat heater level” and select On as the New value:

[![VCDS | Select Control Module | 08 Auto HVAC | Adaptation | Storage of driver seat heater level](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb10a118_thumb.png "VCDS | Auto HVAC | Adaptation")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLb10a118.png)

There’s not setting for the passenger seat though ![Sad smile](http://192.168.40.25:8081/wp-content/uploads/2011/12/wlEmoticon-sadsmile.png)