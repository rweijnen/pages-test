---
id: 1378
title: 'Using Synctoy in Data Migrations'
date: '2011-01-31T11:39:03+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1378'
permalink: /2011/01/31/using-synctoy-in-data-migrations/
short-url:
    - 'http://bit.ly/fERSL0'
views:
    - '4690'
categories:
    - General
---

In every project I do I will have to migrate data at some point. This usually involves three types of data:

- <span style="color: #35383d;">Home Directory Data</span>
- <span style="color: #35383d;">Workgroup Data (eg office documents)</span>
- <span style="color: #35383d;">Application Data (not database but flat file data belonging to applications such as templates and documents).</span>

Home Directory Data is usually a flat copy although I tend to filter out the garbage (temp files and such).

Workgroup data usually needs to be cleaned up so it involves some kind of data mapping (folder x goes to place y).

Application Data is usually a flat copy from old to new location but often there are things like ini files that are adjusted and we don’t want to overwrite that.

I have always used the [RoboCopy](http://en.wikipedia.org/wiki/Robocopy) tool for this because it allows me to do an initial synchronisation and then keep the data in synch.

This approach has several advantages:

- During the testing phase the data is available and current but can be edited, deleted and so on
- We can schedule it via the Task Scheduler
- In the final migration I only need to do a final synch which means it needs less time

<span style="color: #4c4c4c;">Before the first run I do a pre-flight check that ensures all files are readable. Amongst other things, this pre-flight check contains:</span>

- <span style="color: #4c4c4c;">A special tool that forces admin access to all files (sometimes even admin accounts don’t have permissions to all files)</span>
- Check for really long filenames (like the complete contents of a word document as file name) that might prevent the files from being copied.

<span style="color: #4c4c4c;">I have also written some scripts that parse the RoboCopy log files for errors and do some checks (like compare number of files) to confirm that the copy went ok.</span>

<span style="color: #4c4c4c;">But still there is some manual work involved in checking the final log files and ensuring that the data copy is complete and up to data.</span>

I stumbled upon [Microsoft’s SyncToy](http://www.microsoft.com/downloads/en/details.aspx?familyid=c26efa36-98e0-4ee9-a7c5-98d0592d8c52&displaylang=en) tool and it seems that it can do the same with some added advantages:

- <span style="color: #35383d;">It has a preview button that shows us which files will be overwritten and how many files will be copied</span>
- <span style="color: #35383d;">Offers a central view of all the copy operations and a central report with the number of errors etc</span>
- <span style="color: #35383d;">Supports different modes: Synchronize, Echo and Contribute</span>
- <span style="color: #35383d;">GUI tool but also commandline support (SyncToyCmd)</span>

So I did a test with SyncToy (only for Application Data)and here are my observations:

By default the “Save overwritten files in the Recycle Bin” option is enabled. Not needed in this scenario so disable it:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb26.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image26.png)

By default Synctoy does not take a hash of the file but checks on size and last saved date/time.

This is ok for lots of data types and will improve the copy speed, but sometimes a file can be modified without it’s size or date/time being changed (eg flat file databases).

So carefully choose when you need to Enable the “Check file contents”

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb27.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image27.png)

Use the Include and Exclude options where needed:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb28.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image28.png)

Carefully choose the Synch mode, sometimes you need to copy files from different sources into the same destination tree.

So carefully choose between Echo and Contribute:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb29.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image29.png)

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb30.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image30.png)

Use the Preview All option to verify before Running!

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb31.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image31.png)

Finally after the Run check the results, note that the screenshot below was not the first run so already present files were not overwritten (but SyncToy nicely indicates that):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb32.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image32.png)

A final observation:

SyncToy generates some files in %APPDATA%\\Local\\Microsoft\\SyncToy\\2.0 that become quite large:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb33.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image33.png)

So be sure to have enough free disk and perhaps you want to make a backup of these files after you have configured everything.

I did not test if SyncToy can handle very complex and/or large datasets (although I assume it can) or to what size it’s DAT files can grow.

The largest single dataset in my test was almost 6 GB with 98.000 files and 1439 folders. The DAT file was 111 MB after a full run.

I am very interest in your feedback, do you like this approach? Or maybe you use different methods or tools?