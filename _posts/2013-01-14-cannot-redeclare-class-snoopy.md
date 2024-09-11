---
id: 2990
title: 'Cannot redeclare class Snoopy'
date: '2013-01-14T19:31:34+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2990'
permalink: /2013/01/14/cannot-redeclare-class-snoopy/
short-url:
    - 'http://bit.ly/VFYRN8'
views:
    - '871'
categories:
    - Programming
tags:
    - PHP
    - Wordpress
---

After installing a new Plugin in WordPress called [Native Apps Builder](http://wordpress.org/extend/plugins/native-apps-builder/) I got the following error when I tried to go to the Plugin’s settings:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image12.png)Fatal error: Cannot redeclare class Snoopy in www/blog/wp-content/plugins/native-apps-builder/appsbuilderapi.php on line 34

This error is caused because a class named "Snoopy" is being declared twice. I figured that two plugins were incompatible with each other so I first needed to know which ones.

Easiest way to find that out is to use the Linux Shell and search for the string Snoopy in all files from the WordPress Plugin folder:

[![SNAGHTMLa49c24b](http://192.168.40.25:8081/wp-content/uploads/2013/01/SNAGHTMLa49c24b_thumb.png "SNAGHTMLa49c24b")](http://192.168.40.25:8081/wp-content/uploads/2013/01/SNAGHTMLa49c24b.png)

I opened the first file that was also using Snoopy, *sitemap-core.php*:

 “&gt;

I concluded from that code that Snoopy is already included in WordPress by default in the *wp-includes* folder.

I openend *wp-includes/class-snoopy.php* and the class is only loaded conditionally:

“&gt;

This seemed like the proper way to do it so I decided to modify appsbuilderapi.php to define the class Snoopy conditionally as well.

Go to the plugin in WordPress and click Edit:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image13.png)

Select the proper file from the filelist:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image14.png)

Scroll down to the class definition (class Snoopy) and insert the following line before it:

“&gt;

Then scroll down to the end of the class definition and add an endif to close the condition:

“&gt;

Click Update File to save the changes and then it works:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/01/image_thumb15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/01/image15.png)