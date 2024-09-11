---
id: 1187
title: 'Convert Drive Bitmask to Drive Letter'
date: '2011-01-14T11:06:03+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/01/14/convert-drive-bitmask-to-drive-letter/'
permalink: /2011/01/14/convert-drive-bitmask-to-drive-letter/
views:
    - '1891'
short-url:
    - 'http://bit.ly/fsrMlv'
categories:
    - Delphi
    - Programming
---

I was writing a test program that will perform some actions when a USB Memory Stick is inserted.

When this happens Windows send a Broadcast a [WM\_DEVICECHANGE](http://msdn.microsoft.com/en-us/library/aa363480(VS.85).aspx) message.

The wParam member of this Message contains a (pointer to) a [DEV\_BROADCAST\_HDR](http://msdn.microsoft.com/en-us/library/aa363246(VS.85).aspx) structure.

if the dbch\_devicetype member of this structure is of type DBT\_DEVTYP\_VOLUME then we can cast the structure to [DEV\_BROADCAST\_VOLUME](http://msdn.microsoft.com/en-us/library/aa363249(v=VS.85).aspx).

And finally the dbcv\_unitmask member of that structure returns a Bitmask containing the Drive Letter.

A fast and convenient method to convert this Bitmask to a Drive Letter (the first found) is the function below:

 “&gt;