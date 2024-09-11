---
id: 577
title: 'Memory Leaks when using the Virtual TreeView Component'
date: '2010-06-09T09:50:49+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=577'
permalink: /2010/06/09/memory-leaks-when-using-virtual-treeview-component/
views:
    - '4740'
short-url:
    - 'http://bit.ly/idmqMl'
categories:
    - Delphi
    - Programming
---

Again a about post about using the [Virtual TreeView component](http://www.soft-gems.net/index.php?option=com_content&task=view&id=12&Itemid=33) (did I mention it’s brilliant?), this time I will talk about memory leaks.

I often use Records to hold the treedata, and usually the record holds some string data (eg a caption) and an (a reference to) an Interface or Object(List) that holds more data.

If you are familiar with Virtual Tree then you know that you must can the NodeData in the OnFreeNode event.

Let’s look at an example:  
“&gt;  
“&gt;  
Looks allright doesn’t it? And still if you test for memoryleaks with Eurekalog or FastMM you will sometimes notice some leaks.

This happens because Virtual Treeview only calls the OnNodeFree event for Validated Nodes and a node that was never “touched” (eg the node was never visible and thus the GetText event was never called) was never validated.

In these cases you can manually validate the node when adding it  
“&gt;