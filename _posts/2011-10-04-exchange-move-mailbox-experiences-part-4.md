---
id: 2128
title: 'Exchange Move Mailbox Experiences Part 4'
date: '2011-10-04T13:43:23+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/10/04/exchange-move-mailbox-experiences-part-4/'
permalink: /2011/10/04/exchange-move-mailbox-experiences-part-4/
views:
    - '2551'
short-url:
    - 'http://bit.ly/qyRmob'
categories:
    - Exchange
tags:
    - Exchange
    - Exchange2003
    - Exchange2010
---

The previous part ([part 3](http://192.168.40.25:8081/2011/09/28/exchange-move-mailbox-experiences-part-3/)) addressed Mailbox Size but did you know that even Message Size (or rather Item size) can prevent a successful move as well?

Here’s an example move mailbox log:

 “&gt;

As you can see in the log this mailbox there is one item with a size of 72 MegaBytes.

Let’s see this in Outlook:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/10/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/10/image.png)

It gets even worse when we open the Message:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/10/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/10/image1.png)

It was addresses to several internal recipients and as you know Exchange 2010 no longer supports [Single Instance Storage](http://support.microsoft.com/kb/175481).