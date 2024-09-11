---
id: 3385
title: 'Run a Process when Citrix Receiver Exits'
date: '2013-09-24T16:11:59+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3385'
permalink: /2013/09/24/run-a-process-when-citrix-receiver-exits/
views:
    - '1286'
short-url:
    - 'http://bit.ly/14DdjtA'
    - 'http://bit.ly/14DdjtA'
categories:
    - Citrix
tags:
    - Citrix
    - Receiver
---

A while ago I was doing some research for [Magic Filter](http://192.168.40.25:8081/2013/08/10/magic-filter-preview/) when I stumbled upon something interesting within Receiver.

Inside wfica32.exe is a function called *\_Eng\_RunExecutableOnExit.* That name caught my interest, I’ve made it a little more readable with Ida Pro:

  
“&gt;  
**<span style="text-decoration: underline;">So what’s the conclusion?</span>**

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/09/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/09/image1.png)We can create the following registry values in HKLM\\SOFTWARE\\Wow6432Node\\Citrix\\ICA Client\\Engine\\Lockdown Profiles\\All Regions\\Lockdown\\XenDesktop:

| **XDControlRunOnExit** | DWORD: *0 for off 1 for on* |
|---|---|
| **XDControlExe** | String or Expandable String: *Executable including path to run, maximum length 260 characters. Environment variables can be used* |
| **XDControlArgs** | String: *Commandline arguments, maximum length 260 characters* |

**<span style="text-decoration: underline;">How is that useful?</span>**

An example is automatically locking the workstation when the Citrix session ends. Use the following settings:

| **XDControlRunOnExit** | 1 |
|---|---|
| **XDControlExe** | rundll32.exe |
| **XDControlArgs** | user32.dll,LockWorkStation |