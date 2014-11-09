---
title: re-rooting my Nexus 7
---
I recently did a write up on how I revived my borked Nexus 7.  The restore I did on it removed everything including the root I had on the device.  Here I will explain how I added it back so I could do some nice things like utilize an OTG cable and install some nice apps like adaway.  Also note prior to this I had already unlocked the bootloader on my Nexus 7 if you are doing this from a brand new Nexus 7 you will have to search and find how to do this too.

First I needed the tools for android again, I used the laptop that had ubuntu and the tools again for this step but if you are on windows you can search for or set up ADB and SDK drivers like [here](http://www.teamandroid.com/2012/07/30/how-to-set-up-adb-fastboot-with-android-sdk/).  

Or like I said on ubuntu I used the tools for android by running:

<pre>
sudo add-apt-repository ppa:phablet-team/tools
sudo apt-get update  
sudo apt-get install phablet-tools android-tools-adb android-tools-fastboot 
</pre>

Next we just have to enable USB debugging in Android system preferences.  If you are on 4.2 through 4.4 you have to go to: Settings > About phone > Build number > Tap it 7 times > Go back > Developer options.

Next download the SuperSu app from [here](http://bananafish.in/files/UPDATE-SuperSU-v1.80.zip) and put on preferably the root of your storage on the Nexus device.

And download the clockword recovery mod from [here](http://bananafish.in/files/recovery-clockwork-6.0.4.3-grouper.img) if you have another Nexus device or don't trust my download you can also search clockwork mod's site for it.

Okay now with the SuperSu app zip on the Nexus 7 storage and the device has debugging enabled we are going to reboot it into fastboot mode, *volume down and power down*.

Now at the fastboot screen plug the Nexus 7 back into your computer and open a terminal and `cd` to the directory that contains the clockworkmod img and run.

```
fastboot flash recovery recovery-clockwork-touch-6.0.4.3-grouper.img
```

This should go successfully and quickly.  Once complete we are going to make a backup of the current rom (in case you want to install a different one) by selecting *backup and restore > backup*  this will take some time.

Next we need to install that Su app we put on the Nexus 7 storage earlier, to do so we select *flash zip from SD card > choose zip from SD card > find the SuperSU update zip > confirm installation*  

After that finishes we can go back to the main menu and select reboot.  Ta-da Nexus 7 all rooted and ready to go.
