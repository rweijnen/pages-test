---
id: 3171
title: 'Decrypting Dell vWorkspace .pit files'
date: '2013-04-08T11:53:32+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3171'
permalink: /2013/04/08/decrypting-dell-vworkspace-pit-files/
short-url:
    - 'http://bit.ly/WGA5g1'
views:
    - '2576'
categories:
    - Quest
tags:
    - Decrypt
    - Quest
    - vWorkSpace
---

The Dell vWorkspace (previously Quest vWorkspace) Client can save a connection to a .pit file which is very similar to an .rdp file with one big difference: it is encrypted!

I am not sure why Dell/Quest have chosen to encrypt their files but a while ago I needed to know what was in a particular pit file so I could troubleshoot an issue.

I first created a test .pit file with the client (pntsc.exe version 7.6.305.791).

[![SNAGHTML6c0786d](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML6c0786d_thumb.png "SNAGHTML6c0786d")](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML6c0786d.png)

Using the Save As option I saved it as test.pit and the I opened it again via the Open option while monitoring the process with Process Monitor:

[![SNAGHTML6cae217](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML6cae217_thumb.png "SNAGHTML6cae217")](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML6cae217.png)

In the Process Monitor screenshot you can see that a few moments after opening the .pit file a temporary file tmpACE4.tmp is opened. Perhaps an intermediate file with the unencrypted version?

I opened pntsc.exe in Ida Pro and searched for pit in the strings tab:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb34.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image34.png)

There was only one reference to this string:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb35.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image35.png)

This code verifies if the file has a .pit extension and then copies the file to the temp path with a .tmp extension.

Then it calls into sub\_41C91C with the tmp file, possibly to decrypt the file. To make it more readable I used Ida’s Rename function to rename sub\_41C91C to DecryptPitFile

I tried this using Ida Pro’s [Appcall](https://hex-rays.com/products/ida/support/tutorials/debugging_appcall.pdf) mechanism. Appcall is a mechanism to call functions inside a debugged program from the debugger or your script as if it were a built-in function.

I copied my test.pit file to C:\\Temp and launched pntsc with Ida’s integrated debugger. Once the GUI was shown I Paused the process and openend the Script Command Window (Shift-F2).

I intered the function’s name: sub\_41C91C with the filename as a parameter and pressed the Run button:

[![SNAGHTML6e16a79](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML6e16a79_thumb.png "SNAGHTML6e16a79")](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML6e16a79.png)

There was no output in Ida, however my test.pit file was decrypted:

 “&gt;

My first solution was to build a tool that works similar to Ida’s Appcall. It launched the vWorkspace client but made sure the window was hidden.

Then it allocated and wrote a string (the filename) to the process and created a thread in it using the [CreateRemoteThread](http://msdn.microsoft.com/en-us/library/windows/desktop/ms682437%28v=vs.85%29.aspx) API setting the lpStartAddress to the sub\_41C91C function and the lpParameter address to the allocated string.

Last step is resuming the thread and waiting for it to finish.

I placed all that code in a dll and that was enough to solve my problem. If you are interested in the code, I have included it in the download at the bottom of this post.

This solution depends on the exact same version of the pntsc.exe client though so it would be better to understand the encryption algorithm.

In Ida Pro a large (256) byte array was visible and screaming for attention:

[![Quest Secret Key (large byte array)](http://192.168.40.25:8081/wp-content/uploads/2013/04/image_thumb.png "Ida Pro Screenshot")](http://192.168.40.25:8081/wp-content/uploads/2013/04/image.png)

This byte array that I’ve renamed to QuestKey here is likely an encryption key or salt. In the code can we see that is used in a function that I’ve renamed to KeySchedule

[![Code Snippet showing the encryption](http://192.168.40.25:8081/wp-content/uploads/2013/04/image_thumb1.png "Code Snippet")](http://192.168.40.25:8081/wp-content/uploads/2013/04/image1.png)

I had a chat with [Benjamin Delpy](https://twitter.com/gentilkiwi) about the encryption algorithm and he was very quick to recognise rc4 in the asm, impressive!

So if we look at the code snippet above we can conclude that a double rc4 encryption is used. The pit file is first encrypted with a random, 16 byte key and the output is encrypted a second time using a fixed key (the large byte array).

Finally a sanity check is performed, the content must start with the sequence PIT.

So in order to decrypt a pit file we must do the following:

- <font color="#35383d">Decrypt the the whole pit file using the 256 byte, fixed, key (the large byte array)</font>
- <font color="#35383d">Get the last 16 bytes of the decrypted content and use this for the second decryption</font>
- <font color="#35383d">Verify that the 3 first characters of the 2nd decryption are PIT</font>
- <font color="#35383d">Save the decrypted content to disk, stripping the 3 characters (PIT ) and the 16 byte key.</font>

I decided to do this in PowerShell because it would be a nice exercise (in fact, that’s why I wrote the rc4 function I published [earlier](http://192.168.40.25:8081/2013/04/05/rc4-encryption-in-powershell/)).

The complete script can be downloaded at the bottom of the post and includes the rc4 function, an example pit file and the “Appcall” dll example.

First we need to define the large byte array in PowerShell:

“&gt;

Then the function that decrypts the PIT file:

“&gt;

Usage is very simple:

“&gt;

If you look at the unencrypted pit file you will notice that it’s very similar to the contents of an rdp file. This is logical since vWorkspace is built on top of RDP.

The password is only included when you’ve checked the “Save my password (encrypted) checkbox:

[![Save Password Option](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML6f4d388_thumb.png "Quest vWorkspace Client")](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML6f4d388.png)

The password is encrypted using [CryptProtectData](http://msdn.microsoft.com/en-us/library/windows/desktop/aa380261%28v=vs.85%29.aspx), again similar to RDP passwords (as I described [earlier](http://192.168.40.25:8081/2007/10/18/how-rdp-passwords-are-encrypted/)).

However the password is encrypted before being passed to Crypt(Un)ProtectData.

[ DecryptPITFile.zip (4338 downloads ) ](http://192.168.40.25:8081/download/decryptpitfile-zip/?tmstv=1726048920 "Version 1.0")