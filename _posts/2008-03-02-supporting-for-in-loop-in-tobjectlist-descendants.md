---
id: 80
title: 'Supporting for in loop in TObjectList descendants'
date: '2008-03-02T15:08:19+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/03/02/supporting-for-in-loop-in-tobjectlist-descendants/'
permalink: /2008/03/02/supporting-for-in-loop-in-tobjectlist-descendants/
views:
    - '4206'
short-url:
    - 'http://bit.ly/gxrOZo'
categories:
    - Delphi
    - 'Terminal Server'
---

For my Terminal Server unit in the Jedi Security library I use 2 TObjectList descendants to hold a list of Terminal Server Sessions and Processes. Consider the sample below which connects to a server and enumerates all sessions:  
  
“&gt;  
In the sample I loop through the sessions with a for loop. Even though Delphi supports the for in loop since Delphi 2005 it’s not possible to use this in TObjectList descendants, so we cannot use this:  
  
“&gt;  
To make this possible we need to implement GetEnumerator and an Enumerator class:  
  
“&gt;  
Now we add a function with the name GetEnumerator in the SessionList class:  
  
“&gt;  
And that’s really all!