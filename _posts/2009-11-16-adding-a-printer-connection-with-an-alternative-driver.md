---
id: 450
title: 'Adding a Printer Connection with an alternative driver'
date: '2009-11-16T10:59:13+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=450'
permalink: /2009/11/16/adding-a-printer-connection-with-an-alternative-driver/
views:
    - '2337'
short-url:
    - 'http://bit.ly/fldqHQ'
categories:
    - Citrix
    - 'Terminal Server'
    - Vista
    - 'Windows 2008'
---

I needed to add a printer connection to a Citrix server but the problem was that this printer had a buggy driver. I wanted to use an alternative driver such as the Citrx Universal Printer driver but on Terminal Server you might want to use the Terminal Services Easy Print driver.

So I decided to make something that could be used in both situations, the result is a small commandline tool called AddPrinter2 (sorry I am not good in finding original names).

It takes 2 parameters: the printername as unc path and the driver name. An example would be:

AddPrinter2 “\\\\server\\printer” “Citrix Universal Printer”.

The output would be something like this:

[![AddPrinter2](http://192.168.40.25:8081/wp-content/uploads/2009/11/addprinter2-small1.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/addprinter2.png)

Please note that the tool works on <span style="text-decoration: underline;">Vista and higher</span>, if you run it on XP or 2003 you will get an error message that the procedure entry point could not be found. It does not require Citrix or Terminal Server, you can use any other (installed) printer driver.

Have fun with it and please leave a comment if you like it!

[ AddPrinter2.zip (2210 downloads ) ](http://192.168.40.25:8081/download/addprinter2-zip/?tmstv=1726048918 "Version 1.0")