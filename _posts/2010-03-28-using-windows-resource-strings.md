---
id: 560
title: 'Using Windows Resource Strings'
date: '2010-03-28T10:45:05+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=560'
permalink: /2010/03/28/using-windows-resource-strings/
views:
    - '2455'
short-url:
    - 'http://bit.ly/fLndaK'
categories:
    - Delphi
    - Programming
---

A few days ago I wrote about Using [Windows Dialogs](http://192.168.40.25:8081/2010/03/24/using-windows-dialogs-from-delphi/) in your own programs, wouldn’t it be nice to be able to use Windows Resource Strings for the same reasons?

Loading a resource string is not difficult, let’s look at some examples:  
“&gt;  
This uses the [LoadString](http://msdn.microsoft.com/en-us/library/ms647486%28VS.85%29.aspx) api to load a Resource String from an Executable or Dll by it’s resource Id. An Example might call might be:  
“&gt;  
This loads the string with ResourceId 226 from dsadmin.dll(.mui):  
“&gt;  
As you can see in this example, some resource strings have identifiers such as %1 and %2 which are used in the [FormatMessage](http://msdn.microsoft.com/en-us/library/ms679351%28VS.85%29.aspx) Api. How can we use that from Delphi?

I wrote a very simple wrapper for it:  
“&gt;  
And here is a usage example:  
“&gt;  
The Result of this is:

Windows cannot complete the password change for John Doe because:  
the password doesn’t meet complexity requirements