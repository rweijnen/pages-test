---
id: 1677
title: 'HP Smart Array controller previous lockup code 0xAB'
date: '2011-04-28T11:47:51+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/04/28/hp-smart-array-controller-previous-lockup-code-0xab/'
permalink: /2011/04/28/hp-smart-array-controller-previous-lockup-code-0xab/
views:
    - '4606'
short-url:
    - 'http://bit.ly/kZEQLl'
categories:
    - General
tags:
    - Hewlett-Packard
    - HP
---

I ran into an error when I was upgrading hardware on an HP BL460c G6 Blade.

After placing 2 new (larger) hard drives the Array Configuration would hang after saving the configuration (It just kept blinking “Saving Configuration.” forever.

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/04/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/04/image6.png)

After a reset the Smart Array Controller (P410) halted a very long time on Initializing and eventually failed.

I retried the whole procedure with the same result but this time the controller reported: “previous lockup code 0xAB”.

My next step would be a firmware update and this kb article from HP confirms that it was indeed a firmware issue: [HP Serial Smart Array Controllers P410/411/212/712m/410i – May Hang with Lockup Error Code 0XAB Configured in Zero Memory Mode](http://h20000.www2.hp.com/bizsupport/TechSupport/Document.jsp?lang=en&cc=us&taskId=110&prodSeriesId=3883890&prodTypeId=329290&objectID=c01916390).

This kb article also tells me why I didn’t have this error on another blade: that one had the Battery Backed Write Cache module.