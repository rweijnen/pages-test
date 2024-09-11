---
id: 3229
title: 'Programmatically set Excel LinkedCell property with VBA'
date: '2013-05-22T11:36:48+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3229'
permalink: /2013/05/22/programmatically-set-excel-linkedcell-property-with-vba/
short-url:
    - 'http://bit.ly/1a52eQN'
    - 'http://bit.ly/1a52eQN'
    - 'http://bit.ly/1a52eQN'
    - 'http://bit.ly/1a52eQN'
    - 'http://bit.ly/1a52eQN'
views:
    - '2018'
categories:
    - Programming
tags:
    - Excel
    - 'Excel 2010'
    - Office
    - 'Office 2010'
    - VBA
---

Yesterday I was working with an Excel document that contained Combobox form controls.

I wanted to count the number of cells containing a particular value using the [COUNTIF](http://office.microsoft.com/en-001/excel-help/countif-function-HP010342346.aspx) formula. However the count returned 0 because the LinkedCell property of the Combobox was not set to the Cell that contained the Combobox.

To set the LinkedCell Ctrl-Click the Combobox to select it, right-click and select Format Control. Then set the correct Cell in the Cell link field:

[![SNAGHTML18f5a4ba](http://192.168.40.25:8081/wp-content/uploads/2013/05/SNAGHTML18f5a4ba_thumb.png "SNAGHTML18f5a4ba")](http://192.168.40.25:8081/wp-content/uploads/2013/05/SNAGHTML18f5a4ba.png)

My sheet contained about 150 Comboboxes, so obviously I was going to do this using a script. I couldn’t find anything useful with Google so I wrote my own Macro.

The Macro assumes that the sheet containing the Comboboxes is the active sheet and only Comboboxes are touched. It also assumes that Comboboxes are places within Cells an do not overlap multiple Cells.

“&gt;

While the script runs, progress is indicated in the Application StatusBar in the bottom left corner:

[![SNAGHTML18fc08c9](http://192.168.40.25:8081/wp-content/uploads/2013/05/SNAGHTML18fc08c9_thumb.png "SNAGHTML18fc08c9")](http://192.168.40.25:8081/wp-content/uploads/2013/05/SNAGHTML18fc08c9.png)