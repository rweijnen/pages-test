---
id: 3688
title: 'Shodan, search engine for IoT or hackers delight?'
date: '2016-05-14T15:36:22+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3688'
permalink: /2016/05/14/shodan-search-engine-for-iot-or-hackers-delight/
views:
    - '714'
short-url:
    - 'http://bit.ly/1Td0zzE'
    - 'http://bit.ly/1Td0zzE'
categories:
    - General
tags:
    - Citrix
    - Security
---

Today I stumbled upon [Shodan](https://www.shodan.io/), a search engine for devices and services.

I decided to search for Citrix and this was the first page of results:   
[![SNAGHTMLf942758](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLf942758_thumb.png "SNAGHTMLf942758")](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLf942758.png)

It’s interesting to see that we get details such as the name of published applications. But it’s possible to get even more details:

[![SNAGHTMLf96a047](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLf96a047_thumb.png "SNAGHTMLf96a047")](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLf96a047.png)

Seems like this is an old XenApp Server (or perhaps even Presentation Server) that’s directly connected to the internet.

Let’s attempt to connect with RDP:

[![SNAGHTMLf995853](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLf995853_thumb.png "SNAGHTMLf995853")](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLf995853.png)

Wow someone doesn’t care much about security!

Let’s try another one:

[![image](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb-3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image-3.png)

Seeing other services such as Oracle in the list made me think of other searches.

Sharename:

[![SNAGHTMLfa266bf](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLfa266bf_thumb.png "SNAGHTMLfa266bf")](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLfa266bf.png)

Searching for Metaframe brings up numerous old unpatched systems. The screenshot below is from a hotel which offers a lot of services (phun intended):

[![image](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb-4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image-4.png)

They must be secure though because they have a firewall.

[![SNAGHTMLfa7f952](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLfa7f952_thumb.png "SNAGHTMLfa7f952")](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLfa7f952.png)

What about a bank with telnet?

[![SNAGHTMLfab916d](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLfab916d_thumb.png "SNAGHTMLfab916d")](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLfab916d.png)

Searching for Remote Desktop even shows screenshots:

[![SNAGHTMLfacf88e](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLfacf88e_thumb.png "SNAGHTMLfacf88e")](http://192.168.40.25:8081/wp-content/uploads/2016/05/SNAGHTMLfacf88e.png)

If you register for an account you can get an api key for automated queries. Combine it with Metasploit and serve up a list of exploitable systems of your liking!