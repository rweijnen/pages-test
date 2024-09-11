---
id: 2354
title: 'Bypassing RES/Appsense Application Security'
date: '2012-01-27T16:15:38+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2354'
permalink: /2012/01/27/bypassing-res-application-security/
short-url:
    - 'http://bit.ly/wXnD9x'
views:
    - '6337'
categories:
    - Citrix
    - RES
    - 'Windows 2003'
---

The video below shows a Proof of Concept of bypassing Application Security in RES Workspace Manager .

Please note that at this time the code is not publicly available so please don’t ask for it.

**EDIT 2**: I added a video that I received from someone who tried my Excel Sheet with AppSense Application Manager.

---

**EDIT:** I wanted to clarify a couple of things regarding this post.

First of all I would like to explain why I wrote this code and why I choose to test it with RES WM.

I had the idea about this approach a long time ago but I never got around to actually do it. The main reason was that I needed to convert Delphi code to VBA and especially converting some Windows headers was a lot of work. Then suddenly I noticed that someone had already converted the headers, so I all I had to do was rewrite the code that used it to VBA.

The choice for RES was made because of two reasons:

1. If you want to beat something, you want to beat the best and I most certainly consider RES WM to be one of the top products.
2. At the time I wrote the POC code I had access to an enviroment with RES in it.

I would like to emphasize that RES contacted me very quickly after publishing this blog. I’ve had contact with RES and they showed a very constructive approach with their primary goal being a fix or guidance for their customers. Hats of to RES taking a constructive approach and I will be working together with RES on this issue.

Finally I would like to state that I didn’t expect this post to draw this much attention, if I did I would have probably taken another approach.

---

<div class="wlWriterEditableSmartContent" id="scid:5737277B-5D6D-4f48-ABFC-DD9C333F4C5D:b589a7f7-312a-4fcc-8f47-3c2f1c8862c2" style="margin: 0px; display: inline; float: none; padding: 0px;"><object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0" height="252" width="448"><param name="src" value="http://www.youtube.com/v/ZdEzV1R2yBs?hd=1"></param><param name="wmode" value="transparent"></param></object></div>### Same demo but now with AppSense:

<div class="wlWriterEditableSmartContent" id="scid:5737277B-5D6D-4f48-ABFC-DD9C333F4C5D:621b380a-4f9f-43ba-becd-7f38384e55bd" style="margin: 0px; display: inline; float: none; padding: 0px;"><object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0" height="252" width="448"><param name="src" value="http://www.youtube.com/v/UJvptlKxEEk?hd=1"></param><param name="wmode" value="transparent"></param></object></div>