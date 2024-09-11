---
id: 4074
title: 'On forbes.com, Cookies and Ads'
date: '2017-03-14T16:34:52+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4074'
permalink: /2017/03/14/forbes-com-cookies-ads/
views:
    - '220'
categories:
    - Uncategorized
tags:
    - 'Adblock Plus'
    - Ads
    - Chrome
    - Cookies
---

Even though I try not to visit the forbes.com site anymore due to their heavy usages of ads, anti adblocker and overwhelming number of cookies they’re trying to push, sometimes however I accidentally follow a tweet that leads to forbes.com and just notice it to late.

Besides wasting your bandwidth, mobile data and especially time there have been a few occasions were the forbes.com page was actually [serving malware](http://www.networkworld.com/article/3021113/security/forbes-malware-ad-blocker-advertisements.html) from their adfeeds.

It annoys me bigtime so let’s “fix” this:

First thing that happens upon visiting the forbes site is that you get a blurred background with a random ad or quote of the day and you need to press `Continue to article`:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-16.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-16.png)

If you open the Developer console in Chrome (Application tab) you can see that Forbes uses a cookie that expires in 24 hours. This cookie make sure that you don’t see the “welcome” ad for 24 hours:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-17.png)

So let’s make this cookie valid for a bit longer, this is easy with the [EditThisCookie](https://chrome.google.com/webstore/detail/editthiscookie/fngmhnnpilhplaeedifhccceomclgfbg) extension. On the forbes.com page click on EditThisCookie and select the dailyWelcomeCookie:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-18.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-18.png)

Click on the Expiration date and select something in the future, then Click Set and Submit changes:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-19.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-19.png)

Then there is an anti adblocker popup:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-20.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-20.png)

You can get rid of it by adding this line to your adblock plus options `@@||forbes.com^$generichide`.

But as I started at the beginning of the post, my best recommendation is to just avoid the forbes.com page.