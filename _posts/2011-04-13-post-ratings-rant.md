---
id: 1674
title: 'Post Ratings Rant'
date: '2011-04-13T22:32:30+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/04/13/post-ratings-rant/'
permalink: /2011/04/13/post-ratings-rant/
views:
    - '2189'
short-url:
    - 'http://bit.ly/enzN59'
categories:
    - Uncategorized
tags:
    - MySQL
    - Wordpress
---

On my blog I offer visitors the options to rate the articles I write using either thumbs up/down or a 1-10 star rating.

I am interested in what content my readers like so these ratings (and of course comments you leave) indicate what kind of articles you like and which you don’t.

However some people feel the need to abuse this, take a look at this:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image2.png)

Someone felt it was necessary to rate a lot of articles with the lowest possible rating (1) meaning they are all crap.

The IP Address of this voter is 192.33.238.14:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image3.png)

Let’s check what this IP has voted in MySQL:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image4.png)

So this voter **rated 10 articles within 36 seconds** so I assume he/she didn’t even bother to read the articles at all!

As a final step let’s check who [owns this IP](http://whois.domaintools.com/192.33.238.14):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image5.png)

I somehow know this company.