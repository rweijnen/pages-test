---
id: 2404
title: 'Scripting Citrix Online Plugin Settings'
date: '2012-02-13T13:55:45+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2404'
permalink: /2012/02/13/scripting-citrix-online-plugin-settings/
short-url:
    - 'http://bit.ly/xb1u2X'
views:
    - '7563'
categories:
    - Citrix
    - PowerShell
---

The Citrix Online Plugin has a number of settings that can be changed. This includes things as Window Size and Color Depth:

[![Session Options | Window size | Default | Full Screen | Requested Color Quality](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML84f9096_thumb.png "Options - Citrix online plug-in")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML84f9096.png)

In my case I wanted to preset the Window size to Full Screen so using Process Monitor I checked where the Online Plugin writes this setting. I Used a Filter that includes only the Online Plugin (PNAMain.exe) and the RegSetValue Operation:

[![Filter on Process Name is PNAMain.exe | Operation is RegSetValue](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML8593f5c_thumb.png "Process Monitor Filter")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML8593f5c.png)

This yielded only few results:

[![RegSetValue Results](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML85ab4c9_thumb.png "Process Monitor")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML85ab4c9.png)

I changed the setting back and compared the registry, that made clear that the settings was written to “Configuration Model 000”.

Unfortunately the key is a REG\_BINARY and I don’t like blindly importing this key into other systems since we have no idea what else we are importing.

However when editing the value in Regedit we see that the data looks like XML:

[![Edit Binary Value](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML861137c_thumb.png "Regedit")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML861137c.png)

I wrote a small PowerShell script to read this data into a string:  
“&gt;  
However the string is hard to read because it’s not formatted and indented so I tried to cast it to an XML Object but this errors because there is no Root element and because some element names have a space.

So let’s fix this:  
“&gt;  
And now we can load the data into an XML Object:  
“&gt;  
And finally we have readable data:  
“&gt;  
To change an item we can use the following code:  
“&gt;  
To change the Window Size from Default to Full Screen I needed to add an Item with Key UserDisplayDimensions and Value “fullscreen”. This can be done like this:  
“&gt;  
Before we can write the new data to the registry we need to get rid of the dummy root node and replace the underscores in the element names with a space again:  
“&gt;  
And the final step, write it back to the registry:  
“&gt;