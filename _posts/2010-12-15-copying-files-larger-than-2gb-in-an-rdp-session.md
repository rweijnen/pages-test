---
id: 873
title: 'Copying files larger than 2GB in an RDP Session'
date: '2010-12-15T14:00:50+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/12/15/copying-files-larger-than-2gb-in-an-rdp-session/'
permalink: /2010/12/15/copying-files-larger-than-2gb-in-an-rdp-session/
views:
    - '2834'
short-url:
    - 'http://bit.ly/ft9PxA'
categories:
    - 'Terminal Server'
---

Just had a good laugh while reading Microsoft [KB article 2258090](http://support.microsoft.com/kb/2258090 "Copying files larger than 2 GB over a Remote Desktop Services or Terminal Services session by using Clipboard Redirection (copy and paste) fails silently"):

When you try to copy a file that is larger than 2 GB over a Remote Desktop Services or a Terminal Services session through Clipboard Redirection (copy and paste) by using the RDP client 6.0 or a later version, the file is not copied. And, you do not receive an error message.

Wow, did you ever (attempt to) copy such a large file in your Remote Desktop Session?