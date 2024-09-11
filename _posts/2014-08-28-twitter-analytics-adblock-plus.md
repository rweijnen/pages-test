---
id: 3537
title: 'Twitter Analytics and AdBlock Plus'
date: '2014-08-28T13:35:47+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3537'
permalink: /2014/08/28/twitter-analytics-adblock-plus/
short-url:
    - 'http://bit.ly/VPBRhE'
views:
    - '635'
categories:
    - Uncategorized
tags:
    - AdBlock
    - Ads
    - Analytics
    - Twitter
---

Twitter has opened access to Analytics for all users. However when you use an Ad Blocker, like Adblock Plus, you’ll get the following error:

[![A problem occurred while loading the page. To use this site, you need to disable AdBlock or any other ads-blocking extension you are using, or customize it to show ads on this site](http://192.168.40.25:8081/wp-content/uploads/2014/08/image_thumb.png "Twitter Analytics")](http://192.168.40.25:8081/wp-content/uploads/2014/08/image.png)

For Adblock Plus you can fix this by adding a filter: go to Filter Preferences and on the "Custom filters" tab add a new filter within a filter group (or create a filter group for this rule).

Use this as a filter rule:

`@@||ads.twitter.com/stylesheets/ads-allow.css`

[![SNAGHTMLea2974c](http://192.168.40.25:8081/wp-content/uploads/2014/08/SNAGHTMLea2974c_thumb.png "SNAGHTMLea2974c")](http://192.168.40.25:8081/wp-content/uploads/2014/08/SNAGHTMLea2974c.png)

And don’t forget to Enable the rule!

**EDIT:** Twitter user [@Ertraeglichkeit](https://twitter.com/Ertraeglichkeit "https://twitter.com/Ertraeglichkeit") mentioned a different method:

[![image](http://192.168.40.25:8081/wp-content/uploads/2014/08/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2014/08/image1.png)

To understand how this works it’s good to know how adblocker detection roughly works, see [here](http://erikswan.net/abp/) for a good explanation.

To implement this in AdBlock Plus use the following filter rule:

`analytics.twitter.com###BlockedAdsWarning`

Or install the "[Element Hiding Helper](https://adblockplus.org/en/elemhidehelper)" for AdBlock Plus then open the Twitter Analytics Page and choose the "Select an element to hide" option:

[![AdBlock Plus menu | Select an element to hide](http://192.168.40.25:8081/wp-content/uploads/2014/08/image_thumb2.png "AdBlock Plus Menu")](http://192.168.40.25:8081/wp-content/uploads/2014/08/image2.png)

Move the mouse around until the red rectangle is around the complete page (because that’s what’s being hidden with the overlay:

[![Element Hiding Helper Screenshot](http://192.168.40.25:8081/wp-content/uploads/2014/08/SNAGHTMLec30a1b_thumb.png "Select an element to hide")](http://192.168.40.25:8081/wp-content/uploads/2014/08/SNAGHTMLec30a1b.png)

In the bottom left corner you can see the html element that is selected. Press the S key to select the object and the rule is composed:

[![Compose element hiding rule Dialog](http://192.168.40.25:8081/wp-content/uploads/2014/08/SNAGHTMLec3df63_thumb.png "Compose element hiding fule")](http://192.168.40.25:8081/wp-content/uploads/2014/08/SNAGHTMLec3df63.png)