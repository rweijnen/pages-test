---
id: 3495
title: 'Cannot connect to iTunes Store after enabling https scanning in Sophos UTM'
date: '2014-07-16T18:33:25+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3495'
permalink: /2014/07/16/cannot-connect-to-itunes-store-after-enabling-https-scanning-in-sophos-utm/
views:
    - '2666'
short-url:
    - 'http://bit.ly/1yrJetV'
categories:
    - Uncategorized
tags:
    - Apple
    - AppStore
    - iTunes
    - Sophos
    - UTM
---

![Sophos UTM Icon](https://secure2.sophos.com/en-us/medialibrary/Images/Products/Icons/icon-utm.png?la=en "Sophos UTM")I am currently implementing [Sophos UTM](http://www.sophos.com/en-us/products/free-tools/sophos-utm-home-edition.aspx) and I quite like this solution. It is free up for home usage and can easily be installed on a hypervisor.

I wanted to scan encrypted traffic (ssl) as well so I activated the "Decrypt and scan" option:

[![image](http://192.168.40.25:8081/wp-content/uploads/2014/07/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2014/07/image2.png)

When testing this on one of my iPad’s I noticed that the App Store didn’t work properly anymore.

When I tried to update applications I got the following error: "**Cannot connect to iTunes Store**". Additionally when I searched for Apps the search would return no results.

To fix this go to Web Protection | Filtering Options and click Edit on the "Apple Update" item:

[![image](http://192.168.40.25:8081/wp-content/uploads/2014/07/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2014/07/image3.png)

In the "For all request" section add the following Target Domain rule:

^https?://(\[A-Za-z0-9.-\]\*\\.)?apple.com

[![For all requests | Matching these URLs | Target Domains](http://192.168.40.25:8081/wp-content/uploads/2014/07/image_thumb4.png "^https?://([A-Za-z0-9.-]*\.)?apple.com")](http://192.168.40.25:8081/wp-content/uploads/2014/07/image4.png)

This was tested on Sophos UTM with firmware version 9.201-25 in Transparent Mode.