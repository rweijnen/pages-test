---
id: 3330
title: 'PNAgent Icon Data Algorithm'
date: '2013-08-08T17:34:05+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3330'
permalink: /2013/08/08/pnagent-icon-data-algorithm/
views:
    - '628'
short-url:
    - 'http://bit.ly/13mpWFF'
    - 'http://bit.ly/13mpWFF'
categories:
    - Citrix
    - Programming
tags:
    - Citrix
    - PNAgent
    - 'Reverse Engineering'
---

Some time ago I wrote about the [PNAgent data](http://192.168.40.25:8081/2012/02/13/scripting-citrix-online-plugin-settings/) that is stored in the registry in XML format.

After that post [Andrew Morgan](http://andrewmorgan.ie/) asked me if I could extract the PNAgent icons from the XML data.

That got me interested so let’s look at this data!

If you look at XML from PNAgent the icondata as in the AppData.Details.Icon node you’ll see something like this:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image5.png)

Seems like the icon data is stored/encrypted in a proprietary format.

**<span style="text-decoration: underline;">Histogram</span>**  
When reversing binary data, a histogram is often a very good method to visualize the data and draw some first conclusions. This is the Histogram of my Icon data:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image6.png)

We can see that only the characters A..P occur and that A has by far the highest occurrence (40%).

- ‘The data consists of 1280 bytes which gives us room for a 32\*32 (1024 bytes) and a 16\*16 icon (256 bytes)
- 
- It’s likely that the data is encoded in printable ASCII characters (‘A’..’P’) where ‘A’ means colour 0, ‘B’ colour 1 and so on.
- The range A..P gives room for 16 colours
- A’ might be the background colour or white because it occurs the most.

<span style="color: #4c4c4c;">So assuming there were both a 16×16 icon and a 32×32 icon in there but I didn’t know in what order!</span>

<span style="color: #4c4c4c;">So I wrote some testcode to draw a 32×32 icon from the data starting at offset one and at offset 256. Offset 0 clearly produced an incorrect image:</span>

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image7.png)

<span style="color: #4c4c4c;">So the 16\*16 icon comes first and the 32×32 icon second. But I didn’t yet know the correct colours.</span>

I did some experiments by presenting a full black icon, a full white icon and a checkerboard icon. After some experimenting I found out that PNAgent uses the [Windows default 16-colour palette](http://en.wikipedia.org/wiki/List_of_software_palettes#Microsoft_Windows_default_16-color_palette):

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image8.png)

That look’s better:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image9.png)

I implemented drawing the icon by declaring the palette in an array indexed by the ASCII character:  
“&gt;  
This makes drawing the icon very easy and fast:  
“&gt;  
To my disappointment the high resolution png icons are not in the xml data but can be requested from PNAgent by XML.

See [Nicholas Dille’s](http://www.sepago.de/d/nicholas) post on the [XMLServiceExplorer](http://www.sepago.de/e/research-development/downloads/xmlserviceexplorer) for more info.