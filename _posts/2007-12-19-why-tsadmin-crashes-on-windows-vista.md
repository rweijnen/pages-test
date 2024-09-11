---
id: 66
title: 'Why TSAdmin crashes on Windows Vista'
date: '2007-12-19T21:38:39+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/12/19/why-tsadmin-crashes-on-windows-vista/'
permalink: /2007/12/19/why-tsadmin-crashes-on-windows-vista/
views:
    - '6063'
short-url:
    - 'http://bit.ly/fjuG96'
categories:
    - 'Terminal Server'
    - Vista
---

Have you ever tried running the Terminal Server Administration tool (aka TSAdmin) on Windows Vista? You would need it to remotely administer windows 2000/2003 Terminal Servers. If you try to run it you will get an Access Violation but why? I found the answer to this question today because I was testing my [TSAdmin replacement ](http://192.168.40.25:8081/2007/10/23/tsadminex/)on different Windows versions. Just like TSAdmin I use an (undocumented) function from Utildll.dll called ElapsedTimeString. It’s a very simple function that returns a formatted elapsed time string (as seen in the Idle time column from TSAdmin).

While my TSAdminEx ran fine on Windows XP, 2003 and even 2008 it would crash on Vista. Investigation showed that the stack was corrupted in the process of enumerating processes and sessions. Eventually I pinned it down to ElapsedTimeString but could not understand what went wrong. At least not until I investigated Utildll.dll version from Windows Vista. In what was probably an attempt from Microsoft to produce safer code they replaced wsprintfW by StringCchPrintfW. But StringCchPrintfW has an additional parameter (count of characters) so they introduced a new parameter to ElapsedTimeString. Now that’s not a smart decision as this directly breaks compatibility with software that uses this API, but they probably thought that it wasn’t issue since TSAdmin is not included with Vista (I don’t know of any other MS tool that uses this API).

But why doesn’t the Access Violation appear on Server 2008? Is this still using wsprintfW for string formatting? The answer is no, they also use StringCchPrintfW but use a fixed 15 character length. (so they “fixed” the issue).

That leaves you with 2 options if you still want to use TSAdmin on Vista:

1. Patch TSAdmin or Utildll
2. Use my TSAdminEx instead which also offers some extra functionality over TSAdmin (I hope to finish it soon, should you wish to beta test then leave a comment).

For now I’ll leave you with a screenshot (click to enlarge) of the current Beta version. As you can see it returns detailed information in the process tab like Memory Usage, Virtual Memory Usage, CPU Time and Process Age.

[![TSAdminEx Beta Screenshot](http://192.168.40.25:8081/wp-content/uploads/2007/12/tsadminexbeta.thumbnail.png)](http://192.168.40.25:8081/wp-content/uploads/2007/12/tsadminexbeta.png "TSAdminEx Beta Screenshot")