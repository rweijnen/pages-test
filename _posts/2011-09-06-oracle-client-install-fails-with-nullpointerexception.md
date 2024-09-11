---
id: 2054
title: 'Oracle Client install fails with NullPointerException'
date: '2011-09-06T15:51:45+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/09/06/oracle-client-install-fails-with-nullpointerexception/'
permalink: /2011/09/06/oracle-client-install-fails-with-nullpointerexception/
views:
    - '4687'
short-url:
    - 'http://bit.ly/pj5etj'
categories:
    - Java
    - Oracle
tags:
    - Java
    - Oracle
---

![](http://t3.gstatic.com/images?q=tbn:ANd9GcR9Cs9herPZ7b62TQZehm5xWRcxW7t2JeV4kyTYpGQgF0TXORvSBg)Today I was asked to assist in troubleshooting an Oracle Client (10g) installation. The installation halted very quickly with a java.lang.NullPointerException:

The install window shows the Exception: “&gt;

I looked at the setup log ([default directory](http://download.oracle.com/docs/html/B10131_02/ts.htm#i1090466) is *SYSTEM\_DRIVE:\\Program Files\\Oracle\\Inventory\\logs*) but this didn’t show any relevant info.

I also tried adding the -debug switch to the install but again no root cause info.

I turns out that this failure is caused when you run the installation with a username that contains an *exclamation mark* (!).

In my case there is a company policy that administrative accounts start with an exclamation mark (eg !rweijnen).