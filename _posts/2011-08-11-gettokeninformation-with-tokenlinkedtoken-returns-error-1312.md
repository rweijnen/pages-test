---
id: 1977
title: 'GetTokenInformation with TokenLinkedToken returns error 1312'
date: '2011-08-11T11:30:47+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/08/11/gettokeninformation-with-tokenlinkedtoken-returns-error-1312/'
permalink: /2011/08/11/gettokeninformation-with-tokenlinkedtoken-returns-error-1312/
views:
    - '9075'
short-url:
    - 'http://bit.ly/pe62rJ'
categories:
    - Programming
    - Vista
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 7'
---

The [GetTokenInformation](http://msdn.microsoft.com/en-us/library/aa446671(v=vs.85).aspx) function can be used with the TokenLinkedToken Information Class on Windows Vista and higher to the linked (Elevated) token.

This is useful when User Account Control is enabled and you want to launch an elevated process e.g. from a service.

This example code fails however when User Account Control is disabled:  
“&gt;  
GetLastError() returns 1312 which is defined in winerror.h as ERROR\_NO\_SUCH\_LOGON\_SESSION with description “A specified logon session does not exist. It may already have been terminated.”

So you should [check if User Account Control is enabled](http://192.168.40.25:8081/2011/08/11/programmatically-check-if-user-account-control-is-enabled/) in such cases (or make this error non critical).