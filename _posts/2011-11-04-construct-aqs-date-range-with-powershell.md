---
id: 2151
title: 'Construct AQS date range with PowerShell'
date: '2011-11-04T16:05:25+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/11/04/construct-aqs-date-range-with-powershell/'
permalink: /2011/11/04/construct-aqs-date-range-with-powershell/
views:
    - '1689'
short-url:
    - 'http://bit.ly/tS4qNo'
categories:
    - PowerShell
tags:
    - AQS
    - PowerShell
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image.png)For a script I needed to create an AQS ([Advanced Query Syntax](http://msdn.microsoft.com/en-us/library/aa965711(v=vs.85).aspx)) Query that contained a date range.

An example of such is a range is: **date:11/05/04..11/10/04**

However we need to account for regional settings where for example the data seperator and the order of day and month may be different.

In my example I wanted to match any data that is 30 days or older so let’s do this in PowerShell:

First we define a variable for the minimum age:

 “&gt;

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image1.png)Then we use the MinValue property to get the a minimum date value (1/1/0001). This is the From date:

“&gt;

And we use the ToShortDateString method to format the date in the current regional settings.

Now we can calculate the To Date:

“&gt;

And the last step is to format the AQS Query String:

“&gt;

In my case the result at the time of writing is:

“&gt;