---
id: 532
title: 'Random Active Directory Notes #2'
date: '2010-03-12T16:57:32+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=532'
permalink: /2010/03/12/random-active-directory-notes-2/
views:
    - '1947'
short-url:
    - 'http://bit.ly/fwPjiE'
categories:
    - 'Active Directory'
    - Delphi
    - Programming
---

If you have ever used Adsi you have probably used the [IADs](http://msdn.microsoft.com/en-us/library/aa705950%28VS.85%29.aspx) interface or derived interfaces such as [IADsUser](http://msdn.microsoft.com/en-us/library/aa746340%28VS.85%29.aspx) or [IADsGroup](http://msdn.microsoft.com/en-us/library/aa706021%28VS.85%29.aspx) (maybe even without realising this).

What you need to know is that these interfaces were created to support scripting languages such as VBScript. The reason is that these scripting language have no support at all for structures such as ADSVALUE and don’t work with Pointers.

A typical use of IADs interface would look like this (in Delphi and using Jwa):  
“&gt;  
The IADs interfaces are fine when you are working with a single object but they are very, very slow, when working with many objects. I also find them a pain to work with as only a few AD attributes are present as properties. For other attributes you need to call the [Get](http://msdn.microsoft.com/en-us/library/aa746347%28VS.85%29.aspx) method which doesn’t always work, in which case you probably need to call the GetEx method. Even the [GetEx](http://msdn.microsoft.com/en-us/library/aa746348%28VS.85%29.aspx) method doesn’t always return the desired result as the property might not be in the Cache in which case we need to call the [GetInfoEx](http://msdn.microsoft.com/en-us/library/aa746350%28VS.85%29.aspx) method first and then Get or GetEx.

Active Directory has the nasty habit of failing when a an attribute is not set, so if you are reading a eg string attribute you probably expect an empty string but Active Directory returns a failure in such a case. And since Delphi declares Get(Ex) as [SafeCall](http://en.wikipedia.org/wiki/X86_calling_conventions#safecall) it will raise an Exception so you need to wrap it in try..except.

If we *have obtained* a value it will always be a variant that we probably need to convert to another type such as a string, datetime or an integer.

My results with implementing IADs interfaces in my Active Directory unit were bad: I wrote a test program that mimics Active Directory Users &amp; Computer and enumerating a Container or OU with about 70 users takes 2-3 seconds. If you need to wait that long when expaning a Tree Node this in simply not acceptable. So I decided to completely drop the IADs interfaces and used the interfaces that are meant for higher level languages such as [IDirectoryObject](http://msdn.microsoft.com/en-us/library/aa746355%28VS.85%29.aspx) and [IDirectorySearch](http://msdn.microsoft.com/en-us/library/aa746362%28VS.85%29.aspx). And guess what? Now my Delphi program, even when running in the debugger, is actually <span style="text-decoration: underline;">much faster</span> than Active Directory Users &amp; Computers!

To be fair to Microsoft, in the documentation of IDirectoryObject is the following note: *The IDirectorySearch interface is a pure COM interface that provides a low overhead method that non-Automation clients can use to perform queries in the underlying directory*.

In the next posts I will talk about IDirectoryObject and IDirectorySearch.