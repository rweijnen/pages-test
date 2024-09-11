---
id: 868
title: 'Packaging an Application that uses the BDE'
date: '2010-12-14T21:12:58+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=868'
permalink: /2010/12/14/packaging-an-application-that-uses-the-bde/
views:
    - '4083'
short-url:
    - 'http://bit.ly/ibyS40'
categories:
    - Altiris
    - Delphi
    - Packaging
    - Programming
    - 'Unattended Installation'
---

Today I needed to package an application that uses the Borland Database Engine (BDE).

The BDE is a database engine/connectivity component commonly used in Delphi and C++ Builder applications. It has been deprecated since 2000 when it was replaced by dbExpress.

But it’s still widely used so you may still find applications that require the BDE.

In my environment I already have a package for the BDE itsself. But the application I needed to package today, needs to have an Alias addded.

This is usually done manually by going into the BDEADMIN control panel applet or by copying the file where BDE saves the aliases (IDAPI32.CFG).

Here is a screenshot of the Alias my application needs (it uses an Interbase database):

[![BDE](http://192.168.40.25:8081/wp-content/uploads/2010/12/bde-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/bde.png)

I didn’t like the copy approach and didn’t even consider the manual option so I wrote 2 little tools.

The first tool can export an Alias to a config file. First it will read all current Aliases and displays them in a list:

[![BDEExport](http://192.168.40.25:8081/wp-content/uploads/2010/12/bdeexport-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/bdeexport.png)

As you can see it doesn’t have a fancy GUI but it does exactly what it needs to do.

Creating the Config file is very simple, just select the desired Alias and press the Save button:

[![SaveAs](http://192.168.40.25:8081/wp-content/uploads/2010/12/saveas-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/saveas.png)

and save the Alias to a configuration file.

The configuration file is a simple, ini like, text file which in my example has the following lines:  
“&gt;  
Now we can import this configuration file into another machine using the second tool.  
This tool requires 3 parameters: the Alias name, the Driver name and that path to the Config file.

In my example:

[![BDEImport](http://192.168.40.25:8081/wp-content/uploads/2010/12/bdeimport-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/bdeimport.png)

So in my package I just include the AddAlias tool and the SOCR.cfg file and call it.