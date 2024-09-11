---
id: 150
title: 'Executing a Fast User Switch programmatically &#8211; part 2'
date: '2008-11-26T16:45:46+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/11/26/executing-a-fast-user-switch-programmatically-part-2/'
permalink: /2008/11/26/executing-a-fast-user-switch-programmatically-part-2/
views:
    - '13155'
short-url:
    - 'http://bit.ly/hBadob'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
---

[Part1](http://192.168.40.25:8081/2008/11/26/executing-a-fast-user-switch-programmatically-part-1/)

Let’s write our own Credential Server implementation.

At first, we need to create a named pipe with a unique name. Let’s construct the pipe name using a GUID – this should be unique, but we can do it in a cycle to be absolutely sure:  
“&gt;  
We need to save the pipe name in a registry key *Name* under the hive *HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon\\Credentials*. By default, only system has access to this key. But if you want to make FUS available to some user(s) and/or group(s), you can modify this registry key permission (but don’t forget about possible security flaws).

  
“&gt;  
Ok, now the pipe and the registry key have been created. The data format of the pipe is:  
“&gt;  
Note that all the UNICODE\_STRING structures are normalized: their Buffer members should be point to memory inside the block, and it should be not a pointer, but an offset from the beginning of the record. What is the Seed field? The Lower byte of it is used to encrypt the Password\_U using the undocumented RtlRunEncodeUnicodeString function (check [http://www.d–b.webpark.pl/reverse01\_en.htm](http://www.d--b.webpark.pl/reverse01_en.htm) for description). The original msgina.dll uses the lower byte of GetTickCount for it, let’s do this as well. Now we can write the function which will create the memory block for the pipe:  
“&gt;  
Ok, now we can write the data to the pipe. We need to do it in another thread since we are using FILE\_FLAG\_WRITE\_THROUGH while accessing the pipe (which means that write function does NOT return until the client has read our data). So first the total number of structures is written; then the whole structure is transmitted (yes, with total size again!).  
“&gt;  
Now, while our data is being written, we disconnect the console and wait for the thread to finish writing to the pipe:  
“&gt;  
Hurray, we’ve done it! So, now, if you’ll adjust the permission of the gina key (*HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon\\Credentials*) to give everyone write access (read is not needed), we can use this function from any account!

P.S.1. x64 part

The x64 version of Windows XP does the FUS in exactly the same way; however, there are 2 factors which will prevent this program for working:

1\) This program is 32 bit. 32 bit programs are accessing the *HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node* registry key instead of *HKEY\_LOCAL\_MACHINE\\SOFTWARE*; So we can add KEY\_WOW64\_64KEY flag to force native registry key access.

2\) UNICODE\_STRING is 16 bytes long instead of 8. So we need to write some small procedure which will adjust the created record for x64:  
“&gt;  
Now this program is working for x64 Windows XP as well 🙂

P.S.2. – TODO list

1\) Adjust the pipe security to protect it from read by anyone except SYSTEM.

2\) Change the pipe write function for using overlapped structure – terminating a thread is not really a good way!

3\) Validate passed credentials before writing them to the pipe

4\) Refactor the code as class

[ SwitchUser.zip (2118 downloads ) ](http://192.168.40.25:8081/download/switchuser-zip/?tmstv=1726048918 "Version 1.1")