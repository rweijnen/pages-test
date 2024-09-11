---
id: 21
title: 'A referral was returned from the server'
date: '2007-10-19T15:58:48+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/10/19/a-referral-was-returned-from-the-server/'
permalink: /2007/10/19/a-referral-was-returned-from-the-server/
views:
    - '31329'
short-url:
    - 'http://bit.ly/eV4Eio'
categories:
    - Programming
tags:
    - 'referall was returned'
    - script
    - VBS
---

Ever tried to run a VBS scripts that queries Active Directory in another domain or from a workstation that is not a domain member? Than you have probably seen this error before:

![Error Message](wp-content/uploads/2007/10/Referral.png)![Error Message](wp-content/uploads/2007/10/Referral.png)![Error Message](http://192.168.40.25:8081/wp-content/uploads/2007/10/referral11.png)

This is because the default settings for Chasing referrals is set to ADS\_CHASE\_REFERRALS\_NEVER.

Add the line below to your script to make it work.  
“&gt;  
So in a script it would look like this:  
“&gt;