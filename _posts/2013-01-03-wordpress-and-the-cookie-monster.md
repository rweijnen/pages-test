---
id: 2976
title: 'WordPress and the Cookie Monster'
date: '2013-01-03T15:19:19+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2976'
permalink: /2013/01/03/wordpress-and-the-cookie-monster/
views:
    - '910'
short-url:
    - 'http://bit.ly/UIAqNw'
categories:
    - General
tags:
    - Cookies
    - Wordpress
---

![Cookie Monster](http://blogs.sfweekly.com/thesnitch/assets_c/2011/10/cookie_monster-thumb-250x224.jpg)As you are probably aware the EU has made [legislation](http://www.theeucookielaw.com/) regarding the use of cookies on websites. This new legislation is active since May 2012.

The Dutch government has also issued legislation in the [Telecommunications LAW](http://www.rijksoverheid.nl/onderwerpen/ict/veilig-online-en-e-privacy/internetbezoek-volgen-met-cookies) which states that you must ask the user for permissions before server out cooking. Unless these cookies are necessary for the correct technical working of the website or service. This leaves some grey areas but for instance using Google Analytics is a clear case of a situation where opt-in is required.

Although I am not using Google Analytics I wanted to check what cookies my own blog was serving out and if it was necessary to ask for an opt-in.

I used a website for this called [CookieChecker](http://www.cookiechecker.nl/) and this was the result:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image7.png)

The "Eigen Cookies" (Own Cookies) are not to worry about but I was curious to the Third-party Cookies because I wasn’t aware of any. So let’s zoom in:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image8.png)

A google search lead me to [this blog post](http://www.keptlight.com/2011/03/wordpress-stats-and-quantcast-connection/) which explains that WordPress Stats is the cause of this Cookie.

I found a WordPress plugin, [WP DoNotTrack](http://wordpress.org/extend/plugins/wp-donottrack/) that stops plugins and themes from adding tracking code or cookies. Goal is to protect visitor’s privacy.

Seemed like a good idea so I installed it and rechecked (please note that if you recheck you will get cached results, you need to click the Refresh link at the bottom of the page:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image9.png)

Now the result is much better:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image10.png)

In case you were wondering, the Third-party Requests are harmless:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image11.png)

If you do use Cookies that require an opt-in you can use a WordPress plugin for it, [Cooking Warning](http://wordpress.org/extend/plugins/cookie-warning/) seems to be a nice one.