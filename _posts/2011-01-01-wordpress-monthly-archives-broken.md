---
id: 1047
title: 'WordPress monthly archives broken'
date: '2011-01-01T19:44:20+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/01/01/wordpress-monthly-archives-broken/'
permalink: /2011/01/01/wordpress-monthly-archives-broken/
views:
    - '3672'
short-url:
    - 'http://bit.ly/i4iSBq'
categories:
    - General
---

I noticed that the monthly archives on my blog were not working. I first thought it was a problem with the .htaccess file but it was correct.

I also tried resetting the permalinks option in WordPress to default an back but that did help as well.

After some digging I was able to fix it by selecting an option in the Robots Meta plugin I use:

[![ArchiveSettings](http://192.168.40.25:8081/wp-content/uploads/2011/01/archivesettings-small.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/archivesettings.png)

I selected the Disable the date-based archives option, saved settings and de-selected it again. I am not sure what it changes (the .htaccess file was not changed) nor did I check the code to see what it does but at least it’s working again!