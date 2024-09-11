---
id: 3504
title: 'Set alert parameters with Visual Studio Authoring Extensions'
date: '2014-07-18T16:08:33+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3504'
permalink: /2014/07/18/set-alert-parameters-visual-studio-authoring-extensions/
short-url:
    - 'http://bit.ly/1nT7oKi'
views:
    - '1556'
categories:
    - Uncategorized
tags:
    - 'Management Pack'
    - SCOM
    - VSAE
---

![System Center Operations Manager Logo](http:///192.168.40.25:8081/wp-content/uploads/2014/07/image_thumb.png?zoom=1.5&resize=89%2C95 "SCOM Logo")In a SCOM Management Pack Custom Properties can be used for Alert Description and Notification as described in this [blog](http://blogs.technet.com/b/kevinholman/archive/2007/12/12/adding-custom-information-to-alert-descriptions-and-notifications.aspx) by Kevin Holman.

In my case I wanted to add the Display Name and the Performance Counter Value in a Performance Threshold Monitor. In XML it would look this this:

 “&gt;

But how to add these parameters when using the [System Center 2012 Visual Studio Authoring Extensions](http://www.microsoft.com/en-us/download/details.aspx?id=30169)?

There is no place in the properties to add extra Alert Parameters:

[![Visual Studio Authoring Extensions | Alert Properties](http://192.168.40.25:8081/wp-content/uploads/2014/07/image_thumb5.png "Alert Properties")](http://192.168.40.25:8081/wp-content/uploads/2014/07/image5.png)

The solutions is to add the required properties as the last lines in the Alert Description field:

[![Visual Studio Authoring Extensions | Specify Alert description](http://192.168.40.25:8081/wp-content/uploads/2014/07/image_thumb6.png "Specify alert description")](http://192.168.40.25:8081/wp-content/uploads/2014/07/image6.png)

If you look into the generated XML (the .mptg.mpx file) you can see that the Alert Parameters have been added:

“&gt;