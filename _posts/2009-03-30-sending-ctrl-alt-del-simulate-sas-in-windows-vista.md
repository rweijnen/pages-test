---
id: 333
title: 'Sending Ctrl-Alt-Del / Simulate SAS in Windows Vista'
date: '2009-03-30T21:51:48+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/03/30/sending-ctrl-alt-del-simulate-sas-in-windows-vista/'
permalink: /2009/03/30/sending-ctrl-alt-del-simulate-sas-in-windows-vista/
views:
    - '18223'
short-url:
    - 'http://bit.ly/fSOtct'
categories:
    - Citrix
    - General
    - Programming
    - 'Terminal Server'
    - Vista
tags:
    - SasLibEx
---

Existing [code](http://192.168.40.25:8081/2008/12/13/locking-a-workstation-part-1/) to simulate the Secure Attention Sequence (SAS),which most people refer to as control alt delete or ctrl-alt-del, no longer works in Windows Vista. It seems that Microsoft offers a library that exports a function called SimulateSAS(). It is not public and one is supposed to request it by sending a mail to <saslib@microsoft.com>. Mails to this address remain unanswered though.

I researched how other people (including Microsoft) have solved this task and was unhappy with the results: some solutions work only with (or without) UAC, most solutions work only for the current or console Terminal Server sessions or need a kernel mode driver.

So I decided to create my own Saslib with the following goals:

- Should work both with and without User Account Control (UAC)
- Should support current, console and any Terminal Server session
- Does not need a driver
- The calling application does not need to be signed or have a special manifest
- Support multiple programming languages

I have succeeded and thus SasLibEx was born: not only can it successfully simulate the SAS sequence it can do this for any/all Terminal Server sessions. It can also lock the workstation (again for all sessions) and switch between the normal desktop and the secure desktop (the desktop that UAC runs on). SasLibEx was successfully tested both with and without User Account Control (UAC).

In the future I will place SasLibEx on it’s own website. Meanwhile you can contact me if you are interested in it at the following mail address: ![mail](http://192.168.40.25:8081/wp-content/uploads/2009/03/info_at_simulatesas_com1.png)

Please note that I have spend lots of time into this project and therefore I cannot give it away for free

**Update: I have added new features to SasLibEx, see here:** <http://192.168.40.25:8081/2009/04/07/saslibex-updates/>