---
id: 856
title: 'Writing Environment  Variables to the Registry from a Script'
date: '2010-12-13T20:52:59+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=856'
permalink: /2010/12/13/writing-environment-variables-to-the-registry-from-a-script/
views:
    - '3041'
short-url:
    - 'http://bit.ly/h116Ds'
categories:
    - Altiris
    - Citrix
    - PowerShell
    - script
    - 'Terminal Server'
---

I usually change the text below the “This Computer” icon to reflect the current username and servername:

![UserOnComputer](http://192.168.40.25:8081/wp-content/uploads/2010/12/useroncomputer.png)

This is an ancient trick, just set the the *LocalizedString* Value of the following key:  
“&gt;  
to “%USERNAME% on %COMPUTERNAME%”.

It get’s a little more complicated if you want to set this from a script, because the environment variables are replaced with the actual value BEFORE they are entered in the Registry.

So I always did this with a small VBScript but I figured we should be able to do it with a simple oneliner.

I first tried the Reg command because the help describes that you can use the caret sign (^) and even gives an example:  
“&gt;  
It actually works if we do this:  
“&gt;  
But it fails if we try this:  
“&gt;  
My next try was to do it in PowerShell but if using the “&amp; {&lt;command&gt;}” syntax it fails because of the same reason.

So I came up with the following code:  
“&gt;  
Basically I use an environment var to set a readable string where the % sign is replaced by the @ sign.

And in the PowerShell statement I use the -replace operator to replace the @ with a % again.

I used \[char\]0x25 (0x25 is the Hex value of the ASCII code for %) because a single % sign is removed in a bat file.

Basically I use an environment var to set a readable string where the % sign is replaced by the @ sign.

And in the PowerShell statement I use the -replace operator to replace the @ with a % again.

I used \[char\]0x25 (0x25 is the Hex value of the ASCII code for %) because a single % sign is removed in a bat file.