---
id: 1242
title: 'Enumerating Session Processes with NtQuerySystemInformation'
date: '2011-01-20T15:34:23+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1242'
permalink: /2011/01/20/enumerating-session-process-with-ntquerysysteminformation/
short-url:
    - 'http://bit.ly/g9q2lT'
views:
    - '4452'
categories:
    - Delphi
    - Programming
    - 'Strange Error'
    - 'Windows 2003'
    - 'Windows XP'
---

As you may know, you can enumerate processes of a specific Terminal Server or Citrix session using the [NtQuerySystemInformation](http://msdn.microsoft.com/en-us/library/ms724509(VS.85).aspx) function.

On x86 system the code below works fine:  
“&gt;  
While this works fine on Windows XP and 2003 x86, it fails to work correctly on the x64 versions of Windows XP and 2003 (or maybe even higher).

The problem is that RetLength is always SizeOf(SYSTEM\_SESSION\_PROCESS\_INFORMATION) and thus we are in an endless loop!

If you disassemble an a call to NtQuerySystemInformation, for example in the [CreateToolhelp32Snapshot](http://msdn.microsoft.com/en-us/library/ms682489(VS.85).aspx) API, you will see that it just executes NtQuerySystemInformation and increases the buffer size in a loop.

Ok, let’s do it in the same way 🙂  
“&gt;  
But this code doesn’t work as well. The reason is that you need to place the whole buffer in a contiguous memory area.

So finally the working code is :  
“&gt;