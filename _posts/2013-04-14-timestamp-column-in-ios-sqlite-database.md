---
id: 3221
title: 'Timestamp column in iOS SQLite database'
date: '2013-04-14T15:59:45+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3221'
permalink: /2013/04/14/timestamp-column-in-ios-sqlite-database/
views:
    - '9138'
short-url:
    - 'http://bit.ly/11cG2C4'
    - 'http://bit.ly/11cG2C4'
    - 'http://bit.ly/11cG2C4'
    - 'http://bit.ly/11cG2C4'
categories:
    - iPhone
tags:
    - iOS
    - SQLite
---

I was researching a database from an iOS app called &lt;appname&gt;.sqlite. From the filename it was obvious that we were dealing with an SQLite database.

I opened the database with [SQLite Database Browser](http://sqlitebrowser.sourceforge.net/) and the table I looked at has datetime values which are expressed in the TIMESTAMP data format in SQLite:

[![SNAGHTMLb5e026b](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb5e026b_thumb.png "SNAGHTMLb5e026b")](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb5e026b.png)

SQLite documentation indicates that the TIMESTAMP format is based on unix time: the number of seconds elapsed since 01-01-1970 in UTC time.

I tried to convert the TIMESTAMP to a formatted, local time, string like this with the following query:

 “&gt;

But the results were a little odd since I expected dates in 2013:

[![SNAGHTMLb61d276](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb61d276_thumb.png "SNAGHTMLb61d276")](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb61d276.png)

[Apple documentation](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/NSDate_Class/Reference/Reference.html) states that iOS data/time values are stored as a time value relative to an absolute reference date-the first instant of 1 January 2001.

If we calculate the unix time for 01-01-2001 the result is 978307200:

[![SNAGHTMLb65a0fb](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb65a0fb_thumb.png "SNAGHTMLb65a0fb")](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb65a0fb.png)

So to get the correct date we can use the following query:

“&gt;

That looks better:

[![SNAGHTMLb66edd3](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb66edd3_thumb.png "SNAGHTMLb66edd3")](http://192.168.40.25:8081/wp-content/uploads/2013/04/SNAGHTMLb66edd3.png)