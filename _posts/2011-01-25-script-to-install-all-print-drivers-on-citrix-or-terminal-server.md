---
id: 1292
title: 'Script to install all print drivers on Citrix or Terminal Server'
date: '2011-01-25T15:24:14+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1292'
permalink: /2011/01/25/script-to-install-all-print-drivers-on-citrix-or-terminal-server/
short-url:
    - 'http://bit.ly/eejuXr'
views:
    - '10534'
categories:
    - Altiris
    - Citrix
    - PowerShell
    - 'Terminal Server'
---

I wrote a PowerShell script to install all printer drivers on a Citrix or Terminal Server.

Actually the script isn’t specific to Citrix or Terminal Server but on such environments we need to preload all drivers because users do not have the permissions to do that.

I have chosen for PowerShell because you can do it in a one-liner which makes it easy to run this script from my Altiris server on all Citrix Servers.

The idea is that we enumerate all the shared printers on a Printer Server and make a connection to each printer. This will make sure that the driver is installed if it wasn’t already present.

The script could even be scheduled to enforce that newly added printer drivers are added to each Citrix Server.

To enumerate all printers we can use the [Win32\_Printer](http://msdn.microsoft.com/en-us/library/aa394363(v=vs.85).aspx) WMI class like this:  
“&gt;  
It’s possible that some printers are not shared so we are going to filter that out using the -filter parameter:  
“&gt;  
Our next step is to make a printer connection and MSDN shows that the win32\_printer class has an [AddPrinterConnection](http://msdn.microsoft.com/en-us/library/aa384769(v=vs.85).aspx) method.

So I first tried:  
“&gt;  
I am not sure of the exact reason but obviously there is no access to the AddPrinterConnection method.

This works though:  
“&gt;  
Now we can use the For-Each Object to make a connection to each printer using the \_\_SERVER property for the servername and the ShareName property for the Printername:

Once again I met a PowerShell oddity: I couldn’t use AddPrinterConnection(“\\\\$\_.\_\_SERVER\\$\_.ShareName”) so I used:  
“&gt;  
And finally we put the whole thing in a one liner, replacing double quote (“) with (‘) and using the gwmi alias to make the line shorter:  
“&gt;  
<span style="color: #ff0000;">**EDIT:**</span> In order to load new driver we need to enable the SeLoadDriverPrivilege. I have corrected the code above, see [Enabling Privileges for WMI in PowerShell](http://192.168.40.25:8081/2011/01/27/enabling-privileges-for-wmi-in-powershell/) for an explanation.