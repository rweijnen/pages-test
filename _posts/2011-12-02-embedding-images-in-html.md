---
id: 2209
title: 'Embedding images in HTML'
date: '2011-12-02T13:09:43+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/12/02/embedding-images-in-html/'
permalink: /2011/12/02/embedding-images-in-html/
views:
    - '2972'
short-url:
    - 'http://bit.ly/tl4fvB'
categories:
    - PowerShell
    - script
tags:
    - base64
    - embed
    - hta
    - PowerShell
---

I was creating a small dialog in an [.hta file](http://en.wikipedia.org/wiki/HTML_Application) and to make a little prettier for the user I included a company logo:

[![SNAGHTMLdfa805](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLdfa805_thumb.png "SNAGHTMLdfa805")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTMLdfa805.png)

But I wanted to deploy the .hta as a single file.

And this can be done using data: followed by a base64 encoded png:

 “&gt;

The Base64 encoding can easily be done with PowerShell:

“&gt;

Example:

“&gt;