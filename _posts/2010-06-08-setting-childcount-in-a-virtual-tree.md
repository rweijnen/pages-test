---
id: 573
title: 'Setting ChildCount in a Virtual Tree'
date: '2010-06-08T17:39:57+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=573'
permalink: /2010/06/08/setting-childcount-in-a-virtual-tree/
views:
    - '1143'
short-url:
    - 'http://bit.ly/floTeh'
categories:
    - Delphi
    - Programming
---

When working with the [Virtual TreeView component](http://www.soft-gems.net/index.php?option=com_content&task=view&id=12&Itemid=33) the most optimized way of adding (or removing child nodes is by changing the ChildCount.

I often make the mistake of change the ChildCount of a Node using:

“&gt;

If you look into the source you will see why this will not work, the proper way is:

“&gt;

This is mainly a note to self since I tend to forget it all the time 😉