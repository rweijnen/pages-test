---
id: 63
title: 'Delphi and Terminal Server Aware'
date: '2007-11-24T23:21:37+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/11/24/delphi-and-terminal-server-aware/'
permalink: /2007/11/24/delphi-and-terminal-server-aware/
views:
    - '8289'
short-url:
    - 'http://bit.ly/gVLOmL'
categories:
    - Citrix
    - Delphi
    - Programming
    - 'Terminal Server'
---

When an application is not Terminal Server aware (also known as a legacy application), Terminal Server makes certain modifications to the legacy application to make it work properly in a multiuser environment. For example, Terminal Server will create a virtual Windows folder, such that each user gets a Windows folder instead of getting the system’s Windows directory. This gives users access to their own INI files. In addition, Terminal Server makes some adjustments to the registry for a legacy application. These modifications slow the loading of the legacy application on Terminal Server and require up to 8 MegaBytes extra memory. This behaviour can be avoided if the TSAware flag is present in the PE header of an executable as can be read [here](http://msdn2.microsoft.com/en-us/library/microsoft.visualstudio.vcprojectengine.vclinkertool.terminalserveraware(VS.80).aspx) at MSDN.

But how do we set this property in Delphi?

In Windows.pas we can see that the constant is defined:  
“&gt;  
But how to use this in your application?  
Add the line  
“&gt;  
somewhere below the uses clause and we’re done!

Offcourse you are now responsible for making your application Terminal Server compliant which according to Microsoft means: If an application is Terminal Server aware, it must neither rely on INI files nor write to the HKEY\_CURRENT\_USER registry during setup.