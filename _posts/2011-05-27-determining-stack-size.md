---
id: 1794
title: 'Determining stack size'
date: '2011-05-27T15:10:00+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/05/27/determining-stack-size/'
permalink: /2011/05/27/determining-stack-size/
views:
    - '2375'
short-url:
    - 'http://bit.ly/mjaBE2'
categories:
    - Delphi
    - Programming
---

I just read an answer on [StackOverflow](http://stackoverflow.com/questions/6150018/what-is-a-safe-maximum-stack-size-or-how-to-measure-use-of-stack) with this code:

 “&gt;

Unfortunately it lacked explanation, so what does this code do?

It reads offset $4 from the Thread Information Block (the top of stack) into eax and then offset $8 (stack base) into ebx.

Then it substracts the two and moves that into variable eu, that’s all!