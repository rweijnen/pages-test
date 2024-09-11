---
id: 20
title: 'How rdp passwords are encrypted'
date: '2007-10-18T21:23:17+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/10/18/how-rdp-passwords-are-encrypted/'
permalink: /2007/10/18/how-rdp-passwords-are-encrypted/
title_tag:
    - 'Remote Desktop Password Encryption & Decryption Tool'
views:
    - '189251'
short-url:
    - 'http://bit.ly/fixB7L'
categories:
    - Programming
    - 'Terminal Server'
---

![mstsc icon](http://192.168.40.25:8081/wp-content/uploads/2007/10/mstsc1.png) Ever wondered how mstsc saves passwords? If you open an RDP file with a text editor like Notepad you can see the encrypted password. In this article I will show you how to encrypt and decrypt these passwords. Besides password recovery this enables you to create rpd files programmatically or perhaps update the password in many rdp files with a batch file.

[](http://192.168.40.25:8081/wp-content/uploads/2007/10/rdpscreenshot1.png "RDP Screenshot")So if we save a password with mstsc what does it look like? To see that open an rdp file with a text editor (eg Notepad) and you will see something like this:  
“&gt;  
As you can see the password is somehow encrypted before it was saved and automatically decrypted when you open the file with mstsc. .

I did some analysis on mstsc.exe and mstsc uses the functions [CryptProtectData](http://msdn2.microsoft.com/en-us/library/aa380261.aspx) and [CryptUnProtectData](http://msdn2.microsoft.com/en-us/library/aa380882.aspx) which are both exported by crypt32.dll. Since those API calls are documented let’s try to encrypt a password with mstsc and one ourselves and compare the output:  
“&gt;  
So we encrypt the password “secret” and compare this with the same password saved by mstsc  
MSTSC:  
“&gt;  
CryptRDPPassword:  
“&gt;  
As you can see the length of a password encrypted by mstsc is 1329 bytes and the one from CryptRDPPassword is smaller. Actually mstsc generates fixed length passwords and our function’s length is based upon the password. Even more strange is that mstsc password is always 1329 bytes because the input password is unicode which should produce an even size length. It doesn’t really matter though because mstsc.exe accepts the smaller sized passwords without any problems.  
For now I will leave with a demo tool I wrote to encrypt and decrypt rdp passwords. But in a followup article I will show you how te encrypt the passwords to the full length 1329 bytes the same way mstsc does.

[![RDP Screenshot](http://192.168.40.25:8081/wp-content/uploads/2007/10/rdpscreenshot.thumbnail1.png)](http://192.168.40.25:8081/wp-content/uploads/2007/10/rdpscreenshot1.png "RDP Screenshot")

[ Remote Desktop Password Encryption &amp; Decryption Tool (50636 downloads ) ](http://192.168.40.25:8081/download/remote-desktop-password-encryption-decryption-tool/?tmstv=1726048918 "Version 1.0")

Do you find this article usefull, why not leave a comment?