---
id: 336
title: 'Sending WM_KEYDOWN or WM_KEYUP'
date: '2009-03-30T22:57:28+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/03/30/sending-wm_keydown-or-wm_keyup/'
permalink: /2009/03/30/sending-wm_keydown-or-wm_keyup/
views:
    - '12817'
short-url:
    - 'http://bit.ly/e6Ytut'
categories:
    - Delphi
    - Programming
---

If you read the MSDN documentation of [WM\_KEYDOWN](http://msdn.microsoft.com/en-us/library/ms646280(VS.85).aspx) and [WM\_KEYUP](http://msdn.microsoft.com/en-us/library/ms646281(VS.85).aspx) you can see that those message require us to interpret lParam as a bitfield:

> lParam  
> Specifies the repeat count, scan code, extended-key flag, context code, previous key-state flag, and transition-state flag, as shown in the following table.
> 
> 0-15  
> Specifies the repeat count for the current message. The value is the number of times the keystroke is autorepeated as a result of the user holding down the key. The repeat count is always one for a WM\_KEYUP message.  
> 16-23  
> Specifies the scan code. The value depends on the OEM.  
> 24  
> Specifies whether the key is an extended key, such as the right-hand ALT and CTRL keys that appear on an enhanced 101- or 102-key keyboard. The value is 1 if it is an extended key; otherwise, it is 0.  
> 25-28  
> Reserved; do not use.  
> 29  
> Specifies the context code. The value is always 0 for a WM\_KEYUP message.  
> 30  
> Specifies the previous key state. The value is always 1 for a WM\_KEYUP message.  
> 31  
> Specifies the transition state. The value is always 1 for a WM\_KEYUP message.

I was looking for a convenient way to get and read the bits and this is what I made up:  
  
“&gt;  
Note that I use a set to emulate a bitfield (as described [here](http://192.168.40.25:8081/2008/05/01/working-with-bitfields-in-delphi/)). Below a usage example:  
“&gt;  
I also wrote some helper functions that might be helpfull:  
“&gt;