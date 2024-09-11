---
id: 567
title: 'Random Active Directory Notes #4'
date: '2010-03-30T15:04:51+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=567'
permalink: /2010/03/30/random-active-directory-notes-4/
views:
    - '2900'
short-url:
    - 'http://bit.ly/fIEf7C'
categories:
    - 'Active Directory'
    - Delphi
    - Programming
---

Previously I discussed IDirectoryObject, today I will show how to change a user’s password with IDirectoryObject.

I didn’t find any documentation except a [kb article](http://support.microsoft.com/kb/269190) describing how to use pure ldap to do it. Of course I could have used IADsUser::SetPassword but I decided not to because of the following reasons:

- IADs interfaces are terribly slow (although for one use you probably wouldn’t really notice).
- IADsUser::SetPassword tries 3 different methods to set the password (ldap over ssl, kerberos and finally NetUserSetInfo) which makes it even slower (most domain controllers do not have an ssl certificate) and unpredictable.

All example code I found was .NET based using the .NET wrappers for Active Directory and seemed to be meant for use in Adam rather than full Active Directory (it set port number to 389 and password mode to cleartext).

In the end it’s not very difficult but nonetheless it took me a while before I got it right.

We can write to the unicodePwd attribute which wants the password as a double quoted unicode string. If you look at this attribute with AdsiEdit you’ll see that the type is Octet String and that it can be written only.

I was tricked with Delphi’s QuotedStr function for a while because it doesn’t return a double but single quoted string 😉

Below a small snippet from the upcoming JwsclActiveDirectory that shows how to use it:  
“&gt;