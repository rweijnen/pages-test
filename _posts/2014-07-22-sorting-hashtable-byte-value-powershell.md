---
id: 3522
title: 'Sorting a hashtable by byte value in PowerShell'
date: '2014-07-22T12:03:28+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3522'
permalink: /2014/07/22/sorting-hashtable-byte-value-powershell/
short-url:
    - 'http://bit.ly/1rG9uh8'
views:
    - '1809'
categories:
    - PowerShell
tags:
    - HashTable
    - PowerShell
    - Sort
---

![](https://encrypted-tbn2.gstatic.com/images?q=tbn:ANd9GcTEtkV3oLDtyNhyD55yu4uoWbX_oIrnDuJDl4NK6zC59MPNHCnynsqZ9yae)In a PowerShell script I needed to sort a hash table by byte value (not alphabetically, lowercase parameters will be listed after uppercase ones). An example for this requirement is the [Amazon Product Advertising API](http://docs.aws.amazon.com/AWSECommerceService/latest/DG/rest-signature.html).

Consider the following hashtable as an example:

 “&gt;

If we use the Sort-Object to order the list (note that we need to use the [GetEnumerator](http://technet.microsoft.com/en-us/library/ee692803.aspx) method):

“&gt;

We will get the following result:

“&gt;

If you use the `-CaseSensitive` switch the resulting order will remain the same.

![](http://www.omnilink.com/wp-content/uploads/2012/05/Custom-Solutions_PNG.png)I came up with the following code (download link below) to sort the hashtable in the required order:

“&gt;

Let’s try that:

“&gt;

[![Download PowerShell Script](http://192.168.40.25:8081/wp-content/uploads/2014/07/downloadbutton.gif "Download")](https://remkoweijnen.sharefile.eu/d/s7c81ed5a9ed4de0a)