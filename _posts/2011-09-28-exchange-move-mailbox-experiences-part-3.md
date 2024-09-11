---
id: 2123
title: 'Exchange Move Mailbox Experiences Part 3'
date: '2011-09-28T11:08:21+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/09/28/exchange-move-mailbox-experiences-part-3/'
permalink: /2011/09/28/exchange-move-mailbox-experiences-part-3/
views:
    - '1729'
short-url:
    - 'http://bit.ly/ncu4nN'
categories:
    - Exchange
    - PowerShell
tags:
    - Exchange
    - Exchange2003
    - Exchange2010
    - PowerShell
---

In [Part 2](http://192.168.40.25:8081/2011/09/27/exchange-move-mailbox-experiences-part-2/) I showed some details about Mailbox Rule corruptions that can disturb Mailbox Moves.

For this part the topic is Mailbox size, which can be an important factor in deciding which mailboxes you want to move first.

In my case the mailbox size was important because we agreed to move smaller mailboxes during the day but larger mailboxes only outside working hours.

For Exchange 2010 mailboxes it’s very easy to obtain the size using PowerShell.

Example:

 “&gt;

| DisplayName | ItemCount | TotalItemSize |
|---|--:|--:|
| Remko Weijnen | 313 | 34.87 MB (36,564,183 bytes |

But how can we get the Mailbox Size for Exchange 2003 mailboxes?

Using OutlookSpy we can see where this information can be read programmatically.

Select the Inbox and click IMAPIFolder, then inspect the PR\_MESSAGE\_SIZE property:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb29.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image29.png)

There is one problem however, the type of this property is [PT\_LONG](http://msdn.microsoft.com/en-us/library/dd920736(v=office.12).aspx) which is a 4 byte Integer. The maximum value it can hold is 2147483647 (2 GigaByte) after which it will overflow to -1.

With Outlook 2003 SP1 and higher we can use the [PR\_MESSAGE\_SIZE\_EXTENDED](http://msdn.microsoft.com/en-us/library/aa193128(v=office.11).aspx) property which is a 64 bit Integer.

I wrote a PowerShell function (requires [Redemption](http://www.dimastr.com/redemption/)) that creates a dynamic MAPI profile, logs on to the desired Mailbox and returns the Mailbox Size:

“&gt;