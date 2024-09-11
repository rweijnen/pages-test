---
id: 1325
title: 'Making String.IndexOf case insensitive'
date: '2011-01-29T10:41:24+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/01/29/making-string-indexof-case-insensitive/'
permalink: /2011/01/29/making-string-indexof-case-insensitive/
views:
    - '6721'
short-url:
    - 'http://bit.ly/hWStCo'
categories:
    - .NET
    - 'C#'
---

I don’t do much programming in .NET based languages but I have to for some things like the Windows Live Writer plugin I am creating.

I didn’t expect this but the [String.IndexOf](http://msdn.microsoft.com/en-us/library/system.string.indexof.aspx) Method is by default case sensitive.

But we can make it case insensitive if we use one of the overloads: [IndexOf(String, StringComparison)](http://msdn.microsoft.com/en-us/library/ms224425.aspx).

Example:

 “&gt;