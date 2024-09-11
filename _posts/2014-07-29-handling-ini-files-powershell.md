---
id: 3527
title: 'Handling ini files in PowerShell'
date: '2014-07-29T15:03:08+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3527'
permalink: /2014/07/29/handling-ini-files-powershell/
short-url:
    - 'http://bit.ly/1pACx25'
views:
    - '10850'
categories:
    - PowerShell
tags:
    - HashTable
    - 'Ini Files'
    - PowerShell
---

![Ini File Icon](http://www.fileinfo.com/images/icons/files/128/ini-41.png "Ini File")This morning Aaron Parker was wondering if Hash Tables could be used to work with ini files:

[![Aaron Parker | @stealthpuppy | @remkoweijnen @msh_dave need to see if this approach works for editing INI files](http://192.168.40.25:8081/wp-content/uploads/2014/07/image_thumb8.png "Aaron Parker | Twitter | @stealthpuppy")](http://192.168.40.25:8081/wp-content/uploads/2014/07/image8.png)

I thought it was a great idea because in Hash Tables you can use the . operator to get or set a Hash Table entry. But I wondered what to do with sections in ini files. Then I got the idea to use nested Hash Tables for that.

The result is two functions, one to read an ini file into a nested Hash Table and one function to write it back to an ini file.

Let’s look at the code (all code can be download with the link at the bottom of the page), starting with reading the ini file:

 “&gt;

Let’s take an example ini file, the notorious win.ini:

“&gt;

And this is how to use it:

“&gt;

But remember we did not write anything to disk, our Hash Tables exist only in memory. So we need a function to write the ini file back to disk:

“&gt;

And finally we can save the ini file back to disk:

“&gt;  
[![Download PowerShell Script](http:///192.168.40.25:8081/wp-content/uploads/2014/07/downloadbutton.gif?resize=120%2C36 "Download")](https://remkoweijnen.sharefile.eu/d/sa370e81030541a5a)