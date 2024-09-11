---
id: 417
title: 'The poor man&#8217;s e-mail archiving solution'
date: '2009-09-18T12:11:36+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/09/18/the-poor-mans-e-mail-archiving-solution/'
permalink: /2009/09/18/the-poor-mans-e-mail-archiving-solution/
views:
    - '2022'
short-url:
    - 'http://bit.ly/haQToP'
categories:
    - General
---

My work e-mailbox is hosted on an Exchange server and is very limited in size (only 100 MB) so I use a PST file to archive mail when it’s full. This has of course several disadvantages such as possible corruption on the PST and some limitations. I open my mailbox from severals places: Outlook on my laptop, Outlook Web Access remote (no PST available in OWA), Outlook from my Virtual Desktop (no PST since it’s located on my laptop) and so on.

Then I got the idea to create a seperate GMail account and use that for archiving. I added the new account as IMAP mailbox in Outlook and create some folders &amp; subfolders in it:

![OutlookImap](http://192.168.40.25:8081/wp-content/uploads/2009/09/outlookimap.png)

Of course we can simply drag mails into the desired folder. The nice thing is that when you access GMail through the web the folders are visible as Labels:

![GMailLabels](http://192.168.40.25:8081/wp-content/uploads/2009/09/gmaillabels.png)

Searching through archived mails can be done with GMail’s search functionality or Outlook’s.

So now I can access my archive everywhere and open it concurrently.

Some helpfull links:

- [IMAP – GMail help](https://mail.google.com/support/bin/topic.py?hl=en&topic=12806)
- [Outlook 2007 – GMail help](https://mail.google.com/support/bin/answer.py?hl=en&answer=77689)
- [Outlook 2003 – GMail help](https://mail.google.com/support/bin/answer.py?hl=en&answer=77661)
- [Recommended Client Settings – GMail help](https://mail.google.com/support/bin/answer.py?answer=78892)