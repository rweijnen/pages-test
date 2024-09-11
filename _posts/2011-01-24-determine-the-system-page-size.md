---
id: 1268
title: 'Determine the System Page Size'
date: '2011-01-24T15:46:58+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1268'
permalink: /2011/01/24/determine-the-system-page-size/
views:
    - '2471'
short-url:
    - 'http://bit.ly/fHHqBu'
categories:
    - Delphi
    - 'Windows Internals'
---

Just a quick note: if you want to determine the page size of the OS (Windows) you can use the [GetSystemInfo](http://msdn.microsoft.com/en-us/library/ms724381(v=vs.85).aspx) function.

Example:  
“&gt;  
Note that MSDN recommends to use the [GetNativeSystemInfo](http://msdn.microsoft.com/en-us/library/ms724340(v=vs.85).aspx) function when running in a 32 bit app on an x64 OS (and you can use the [IsWow64Process](http://msdn.microsoft.com/en-us/library/ms684139(v=vs.85).aspx) function to determine that).

One example where you need to know the PageSize is when you want to create a Paging File using the [NtCreatePagingFile](http://undocumented.ntinternals.net/UserMode/Undocumented%20Functions/NT%20Objects/File/NtCreatePagingFile.html) function because this function requires that the MinimumSize and MaximumSize parameters are a multiple of the PageSize.

Some interesting links on the subject:

- [Why is the page size on ia64 8K?](http://blogs.msdn.com/b/oldnewthing/archive/2004/09/08/226797.aspx)
- [Pushing the Limits of Windows: Paged and Nonpaged Pool](http://blogs.technet.com/b/markrussinovich/archive/2009/03/26/3211216.aspx)