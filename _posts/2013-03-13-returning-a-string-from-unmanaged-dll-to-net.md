---
id: 3091
title: 'Returning a string from unmanaged dll to .net'
date: '2013-03-13T10:01:31+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3091'
permalink: /2013/03/13/returning-a-string-from-unmanaged-dll-to-net/
short-url:
    - 'http://bit.ly/YZngd7'
views:
    - '4028'
categories:
    - .NET
    - 'C#'
    - C++
    - Delphi
---

![](http://www.vbmigration.com/images/Whitepapers/DllFile.png)I write most of my code in unmanaged languages such as Delphi and C/C++. Sometimes customers ask me to interface my code to their .net code in which case I create a dll for them.

A recurring thing is that I need to return string to .net.

There are many ways to do this of course but in all cases we need to manage memory: who will allocate the memory for the string and who is responsible for freeing it?

**<u>Windows API Method  
</u>**Windows API often requires the caller to pass in an allocated buffer and it’s maximum size and returns the actual size. An example is the [GetComputerName](http://msdn.microsoft.com/en-us/library/windows/desktop/ms724295%28v=vs.85%29.aspx) API:  
“&gt;  
A typical call to this API looks like this in .net (VB.NET example):  
“&gt;  
As you can see we need to allocate the string before the API call and set the correct length after the API call. For that reason I used the StringBuilder class because the regular .net string class does not have a way to change the string length.

**<u>BSTR Method</u>**

For this reason I prefer to use the [BSTR type](http://msdn.microsoft.com/en-us/library/windows/desktop/ms221069%28v=vs.85%29.aspx) because it has much easier memory allocation and deallocation.

An example using ATL’s CString class in C:  
“&gt;  
And an example implementation in VB.NET:  
“&gt;  
Below an example in Delphi for the ReturnString implementation (VB code stays the same):  
“&gt;  
**Note**: In all examples I am not passing the string as the function return value because of compiler implementation differences regarding the calling convention for the return value. A good description can be found [here](http://stackoverflow.com/questions/9349530/why-can-a-widestring-not-be-used-as-a-function-return-value-for-interop) on stackoverflow.