---
id: 512
title: 'Reading accountExpires attribute from Active Directory (in Delphi)'
date: '2010-02-10T15:11:48+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=512'
permalink: /2010/02/10/reading-accountexpires-attribute-from-active-directory-in-delphi/
views:
    - '4374'
short-url:
    - 'http://bit.ly/ifRyA5'
categories:
    - 'Active Directory'
    - Delphi
---

I am writing a class that wraps Active Directory into Objects that live in an Objectlist, much like my [Terminal Server class](http://jwscldoc.delphi-jedi.net/index.html?frmname=topic&frmfile=JwsclTerminalServer_pas.html) in the [Jedi Windows Security Library](http://jwscldoc.delphi-jedi.net/index.html?frmname=topic&frmfile=JwsclTerminalServer_pas.html).

One of the classes is TJwADUser that represents an Active Directory user with all kinds of properties. So while I was implementing them I stumbled upon the [accountExpires](http://msdn.microsoft.com/en-us/library/ms675098%28VS.85%29.aspx) attribute which is implemented as an 8 byte integer so I figured I could read it as Int64, cast this to TFileTime (FILETIME) and convert to TDateTime.

This raised an error however (EVariantTypeCastError with message ‘Could not convert variant of type (Dispatch) into type (Double)’.).

So I checked what kind of variant Active Directory returns and it is not the expected varInt64 but varDispatch.  
It turns out that we need the [IADsLargeInteger](http://msdn.microsoft.com/en-us/library/aa706037%28VS.85%29.aspx) interface to obtain the correct values. The code below works for me:  
“&gt;  
Notes: The Get function is a wrapper for IADs(User).Get(Ex) so you can ignore that and my function returns 0 when the value is empty or on read failure.