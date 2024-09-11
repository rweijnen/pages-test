---
id: 2301
title: 'From Jailbreak to Jailbreak'
date: '2012-01-04T22:02:04+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/01/04/from-jailbreak-to-jailbreak/'
permalink: /2012/01/04/from-jailbreak-to-jailbreak/
views:
    - '3020'
short-url:
    - 'http://bit.ly/zvUgB8'
categories:
    - Apple
    - iPhone
tags:
    - iOS
    - Jailbreak
---

[![Cydia Icon](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb2.png "Cydia Icon")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image2.png)A few days ago I decided to update my iPhone which was still running iOS 4.3.1 to iOS 5.0.1. I delayed this update for a while because I had Jailbreaked my iPhone. Unfortunately an update is much more work when you have Jailbreaked because you also have to restore Cydia settings such as the repositories and Cydia installed Apps.

This blog post is not a guide on how to Jailbreak but more a collections of tips to go from a Jailbreak iOS 4.x to iOS 5.01.

If you notice any extra steps while doing your update *please send them to me so I can add them to this post*.

<span style="text-decoration: underline;">**Backup** </span>Before starting it would be probably be a good idea to create a backup of your device in iTunes. I would also advise to backup your SHSH signatures which you can do using [TinyUmbrella](http://192.168.40.25:8081/2012/01/02/system-process-pid-4-is-listening-on-port-80/). There are lots of guides on how to do this so I will not repeat that here.

**<span style="color: #ff0000;">Without saving your SHSH signatures you cannot restore to an older iOS version if anything goes wrong</span>**!

You may also want to have the IPSW for your current iOS version (preferably the IPSW that you used to do the Jailbreak last time, else you also need the Jailbreak tool for that iOS version).

<span style="font-weight: normal;">I used </span>[<span style="font-weight: normal;">xBackup</span>](http://apptapp.thebigboss.org/mobileweb/onepackage.php?bundleid=uk.co.itsys.xbackup&db=)<span style="font-weight: normal;"> to create a backup of my Cydia settings, including repositories and Apps that were installed through Cydia. xBackup costs $1.50 (about ?1,20) which I found a very reasonable price. </span><span style="font-weight: normal;">  
</span>

| I enabled the Cloud option which stores the backup on a remote server and also opted for backup of App Settings and Icon Layout: | [![xBackup Settings](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0463_thumb.png "xBackup Settings")](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0463.png) |
|---|---|
| Press Backup. | [![Backup & Upload](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0464_thumb.png "xBackup")](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0464.png) |
| When it’s finished, the backup is uploaded to “the Cloud” | [![Uploading Backup to the Cloud](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0465_thumb.png "xBackup")](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0465.png) |
| Optionally pick up the local backup if you want to be extra sure (or create another iTunes Backup) | [![Backup was Uploaded and Saved](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0466_thumb.png "xBackup")](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0466.png) |

**<span style="text-decoration: underline;">Update to IOS 5.01  
</span>**Next step is the actual update to iOS 5.01, note that we do not need to hack the IPSW and restore it, so there’s no need to download the IPSW. *Just use iTunes to update to iOS 5.01*.

**Warning: if you have unlocked your iPhone or are using a Gevey SIM you will probably want to preserve the baseband.** In which you do need the IPSW and use the Extras option in Redsn0w. Thanks to Don Hellwich for pointing that out.

[![A new versions of the iPhone software is available (version 5.0.1) | To update your iPhone with the latest version, click Update.](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb3.png "iTunes")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image3.png)

You may get a warning indicating there are purchased items on your device that have not yet been transferred to iTunes yet:

[![There are purchased items on the iPhone that have not been transferred to your iTunes library. You should transfer these items to your iTunes library before updating this iPhone. Are you sure you want to continue?](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb4.png "iTunes")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image4.png)

To resolve this go to your device, right click and select Transfer Purchases:

[![Transfer Purchases](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb5.png "iTinues")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image5.png)

Wait until the Transferring Purchases has finished:

[![Transferring Purchases | Progress Bar](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb6.png "iTunes")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image6.png)

iTunes will warn you that the update to iOS 5.0.1 will delete all Apps, don’t worry they should be in your iTunes backup.

[![Updating to iOS 5.0.1 will delete all the apps on your iPhone. To preserve your apps, apply this update on the computer where you sync them.](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb7.png "iTunes")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image7.png)

If you get a backup error, like the screenshot below, you probably have an entry in your hosts file (that was put there by TinyUmbrella) such as “127.0.0.1 gs.apple.com”:

[![An error occurred while backing up this iPhone (-43). | Would you like to continue to update this iPhone? | Continuing will result in the loss of all contents on this iPhone.](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb8.png "iTunes")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image8.png)

The update may take a while.

**<span style="text-decoration: underline;">Jailbreak</span>**  
After the update has been finished, restore your iTunes backup after which you are ready to Jailbreak again. I did this using [Redsn0w for Windows v0.9.10b3](https://sites.google.com/a/iphone-dev.com/files/home/redsn0w_win_0.9.10b3.zip?attredirects=0&d=1). This Jailbreak is Untethered, based on geohot’s limera1n exploit and was created by [@pod2g](http://twitter.com/pod2g).

Please do consider a [donation](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=VLSHU7DG68H52) to pod2g.

The Jailbreak is very easy, just launch Redsn0w, boot into DFU mode and follow the on screen instructions. I will not describe the actual Jailbreak process here.

**<span style="text-decoration: underline;">Restore</span>**

When the Jailbreak has finished, open Cydia, then install and launch xBackup.

| Go to the Restore Tab and click Download &amp; Restore: | [![Restore \| Download & Restore](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0467_thumb.png "xBackup")](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0467.png) |
|---|---|
| xBackup will first restore the reposiories. | [![Restoring Sources](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0468_thumb.png "xBackup")](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0468.png) |
| And then re-install your packages: | [![Installing Packages (mins)](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0469_thumb.png "iTunes")](http://192.168.40.25:8081/wp-content/uploads/2012/01/IMG_0469.png) |

[![Installous Icon](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML14220de_thumb.png "Installous Icon")](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML14220de.png)The only application that was not reinstalled by xBackup (by design??) was *Installous* and it’s components. If you were using Installous previously you will need to **reinstall this one manually**.

**<span style="text-decoration: underline;">Synchronize Apps with iTunes  
</span>**Final step is to synchronize your Apps with iTunes to get all your Applications back. If you installed *Installous* in the previous step then you already have AppSync which is required to synchronize non signed applications with iTunes. If not then first install AppSync for iOS 5.0+.

Good luck!