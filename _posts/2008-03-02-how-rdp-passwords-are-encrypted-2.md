---
id: 83
title: 'How rdp passwords are encrypted 2'
date: '2008-03-02T23:28:57+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/03/02/how-rdp-passwords-are-encrypted-2/'
permalink: /2008/03/02/how-rdp-passwords-are-encrypted-2/
views:
    - '14722'
short-url:
    - 'http://bit.ly/gpux1S'
categories:
    - Programming
    - 'Terminal Server'
---

Several months ago I [wrote](http://192.168.40.25:8081/2007/10/18/how-rdp-passwords-are-encrypted/) about encrypting and decrypting RDP passwords. I left one thing open: encrypting the password up to the full 1329 bytes as mstsc does.

Many people were curious about it so I hope the answer is not a disappointment because it’s actually really simple (but I took me a while to figure that out nonetheless). In what I figure is an attempt to hide the password length mstsc always fills up the password with zeroes until it has 512 bytes length.

Then the password is encrypted like I described earlier which gives us a 1328 bytes password hash. So we have one mystery left, how to reach the 1329 bytes size which still is a strange value since the password is in Unicode which takes 2 bytes per char (so the size should be even).

As it turns out, mstsc just adds a zero!

[![RDP v2 Screenshot](http://192.168.40.25:8081/wp-content/uploads/2008/03/rdpv2screenshot.png)](http://192.168.40.25:8081/wp-content/uploads/2008/03/rdpv2screenshot.png "RDP v2 Screenshot")

[ Remote Desktop Password Encryption &amp; Decryption Tool (50636 downloads ) ](http://192.168.40.25:8081/download/remote-desktop-password-encryption-decryption-tool/?tmstv=1726048918 "Version 1.0")