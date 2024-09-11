---
id: 1665
title: 'WordPress redirects to wp-admin/install.php'
date: '2011-04-13T22:14:36+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/04/13/wordpress-redirects-to-wp-admininstall-php/'
permalink: /2011/04/13/wordpress-redirects-to-wp-admininstall-php/
views:
    - '6750'
short-url:
    - 'http://bit.ly/dEHqX6'
categories:
    - Uncategorized
tags:
    - MySQL
    - Wordpress
---

My blog has been down for a short period, if you tried to open WordPress would redirect to wp-admin.install.php and you would see this message:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image.png)

I suspected it was a database problem so I went to phpMyAdmin and did a check on all tables:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image1.png)

Even though MySQL reported all tables to be okay, I tried to repair the wp\_options table since this is a common issue with WordPress and indeed this fixed the problem!