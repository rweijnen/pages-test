---
id: 2135
title: 'Exchange Move Mailbox Experiences Part 5'
date: '2011-10-04T15:21:42+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/10/04/exchange-move-mailbox-experiences-part-5/'
permalink: /2011/10/04/exchange-move-mailbox-experiences-part-5/
views:
    - '2048'
short-url:
    - 'http://bit.ly/rqnDN1'
categories:
    - Exchange
tags:
    - Exchange
    - Exchange2003
    - Exchange2010
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image1.png)Another interesting Move Mailbox error and in this case I am really unsure how this would be possible!

Let’s look at the Move Mailbox log:

 “&gt;

This error occurs because of a size constraint just like the one in the [previous part](http://192.168.40.25:8081/2011/10/04/exchange-move-mailbox-experiences-part-4/).

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/10/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/10/image2.png)But the strange part is that it seems to be a PST file (type IPM.Document.PSTFile) located in the Root folder of the Exchange mailbox (which is not possible AFAIK).

This PST is not visible anywhere in Outlook though so I used OutlookSpy to find it. I selected the Top Of Information store (Mailbox – John Doe) and clicked *IMAPIFolder* and then the *GetContentsTab*:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/10/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/10/image3.png)

I made a backup by using OpenEntry -&gt; Save as MSG file:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/10/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/10/image4.png)

And deleted this item.