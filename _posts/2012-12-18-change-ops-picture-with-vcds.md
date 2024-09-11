---
id: 2918
title: 'Change OPS picture with VCDS'
date: '2012-12-18T12:31:56+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2918'
permalink: /2012/12/18/change-ops-picture-with-vcds/
views:
    - '5811'
short-url:
    - 'http://bit.ly/RBv1JH'
categories:
    - Automotive
tags:
    - OPS
    - Passat
    - VCDS
    - VW
---

This time a car topic: using VCDS you can change the picture that it’s shown on the navigation headunit to match your car.

In my case it’s a [VW Passat Variant B7/3AA](http://en.wikipedia.org/wiki/Volkswagen_Passat#2010_facelift_.28Passat_B7.29) and it can be coded with VCDS:

Open Module 76 – Park Assist:

[![Electronics 1 | 76 - Park Assist](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML18a0ff94_thumb.png "VCDS | Select Control Module")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML18a0ff94.png)

Then select Coding – 07 and enable Bit 4 of Byte 0 (with correct labels you can Enable "Optical Illustration active"):

[![Optical Illustration Active](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML18a1f49c_thumb.png "3AE-919-475 | 3 Bytes long")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML18a1f49c.png)

Then goto Byte 2 and select the correct Model:

[![02 Model: VW Passat Variant](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML18a33987_thumb.png "Ver.1.0.5.2 - 3AE-9190475 | 3 Bytes long")](http://192.168.40.25:8081/wp-content/uploads/2012/12/SNAGHTML18a33987.png)

Now it’s correct:

[![Rijweg controleren!](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb28.png "OPS - optisch parkeersysteem")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image28.png)