---
id: 802
title: 'Altiris AxSched ignores the DSN parameter'
date: '2010-11-23T22:48:47+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/11/23/altiris-axsched-ignores-the-dsn-parameter/'
permalink: /2010/11/23/altiris-axsched-ignores-the-dsn-parameter/
views:
    - '1575'
short-url:
    - 'http://bit.ly/frRnxG'
categories:
    - Altiris
    - 'SQL Server'
---

I was playing around with the AxSched tool that comes with Altiris, in my case the version that comes with v6.9 build 453.

I could not make it connect to the Deployment Server from my test machine, it seemed like the /dsn parameter I used was totally ignored.

I also tried the /d and /db parameters but they didn’t have any effect at all, the message was always:

> Error opening database connection:  
> SQL Server does not exist or access denied.  
> ConnectionOpen (Connect()).

So I opened AxSched in Ida Pro and set a Breakpoint to the place where the SQL Connection seems to be made:

![SQLDriverConnect](http://192.168.40.25:8081/wp-content/uploads/2010/11/sqldriverconnect.png)

Ida nicely hints us that the ConnectionString is in the ECX register so we only have to inspect it to see it’s value.

Actually AxSched does several attemps and the used Connection Strings are:

1. DRIVER={SQL Server};DSN=;SERVER=;Trusted\_Connection=yes;
2. DRIVER={SQL Server};DSN=;SERVER=;UID=sa;PWD=;Trusted\_Connection=no;
3. DSN=Altiris eXpress Database;
4. DSN=Altiris eXpress Database;Trusted\_Connection=yes;
5. DSN=Altiris eXpress Database;UID=sa;PWD=;Trusted\_Connection=no;

So I bypassed by adding a DSN with the name Altiris eXpress Database and then it works.