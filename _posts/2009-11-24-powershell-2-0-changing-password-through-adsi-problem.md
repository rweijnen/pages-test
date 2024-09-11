---
id: 465
title: 'PowerShell 2.0: Changing password through ADSI problem'
date: '2009-11-24T11:50:08+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=465'
permalink: /2009/11/24/powershell-2-0-changing-password-through-adsi-problem/
views:
    - '8772'
short-url:
    - 'http://bit.ly/eKs3Gf'
categories:
    - PowerShell
---

I needed to do a mass password change on imported accounts and decided to do this with Powershell. For some reason (maybe because I am using PowerShell 2.0) I got an unexpected error when using the Password property or the SetPassword method (RandomPassword is a function I wrote that generates Random passwords the meet the Complexity Requirements):  
“&gt;  
<span style="color: #ff0000;">The following exception occurred while retrieving member “CommitChanges”: “Logon failure: unknown user name or bad password.</span>

I first thought that my Password Generator function created a password that didn’t meet the Complexity Requirements but settings the same password through Active Directory Users &amp; Computers was no problem.

I also tried the SetPassword method but it has the same result. The only working way was to use the Invoke method of PSBase:  
“&gt;  
I also verified if using a simple password gave another error:

<span style="color: #ff0000;">Exception calling “Invoke” with “2” argument(s): “The password does not meet the password policy requirements. Check the minimum password length, password complexity and password history requirements. (Exception from HRESULT: 0x800708C5)”</span>

I don’t know why the Password Property and SetPassword Method do not work but I guess it’s a problem in PowerShell’s implementation of the ADSI provider. If someone has the answer please leave a comment!