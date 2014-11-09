---
title: reviving my Nexus 7
---
okay so recently I had found my forgotten/abandoned Google Nexus 7 tablet.  I remember perordering this thing on the 4th of July in 2012.  So it is the original model that Google put it out.  After buying it I had come to realize I do not like the tablet form factor and pretty quickly (after rooting) stopped using the Nexus 7 all together.

Anyway I found this thing again and charged it up and decided to do some updates.  Previously I had used an app called [OTA RootKeeper](https://play.google.com/store/apps/details?id=org.projectvoodoo.otarootkeeper), to keep my root between updating android.  

Anyway I ran the first update after it being off for like six months it updated fine but lost root, no big deal I figured I would sort that out after getting it all up to date.  

So I ran the next update, this would bring it up to 4.4 I think kitkat?  It downloaded fine and the tablet was plugged in to power so I ran the update, it rebooted and nothing...

It finally rebooted into a android screen that said "No Command" and continued to just reboot into that.  

![](http://bananafish.in/files/img/blog/nocommand.jpg)

UGH! I thought I finally just killed my Nexus 7.  Some quick googling showed that other people had a similar problem so I  went to work to fix it.

First I installed some android tools on a spare laptop with ubuntu (it had ubuntu because originally I thought I would put ubuntu touch on this Nexus 7 instead of fixing it ((which did not work either))).

To do so I did:


```
sudo add-apt-repository ppa:phablet-team/tools  
sudo apt-get update  
sudo apt-get install phablet-tools android-tools-adb android-tools-fastboot  
```

You can also get these from the [SDK](http://developer.android.com/sdk/index.html) from google but that way you'll have to manually install them.


Now with the tools installed I need to get the factory image to restore my Nexus 7 too.  This is where the stupid thing is actually fixed, but know that this is *going to wipe and destroy all data on the device*.  That is fine for me because this thing is already beyond saving so let's do it.

The first mistake I made was I grabbed the 4.4 factory image from [here](https://developers.google.com/android/nexus/images#nakasi).
After downloading and untarring I put my Nexus in fastboot mode (power and volume down) 

![](http://bananafish.in/files/img/blog/fastboot.jpeg)

I plugged in my Nexus and ran

```
./flash-all.sh
```

This script is in the directory that you extracted from the image downloaded.  Well this kept failing, saying that it was trying some steps but something was wrong with the bootloader.

After some searching I found that the issue might be that 4.4 was expecting a new bootloader my 'failed' updated Nexus 7 currently didn't have.  So I went back [here](https://developers.google.com/android/nexus/images#nakasi) and downloaded this time version 4.2.  As I think that is the version I had before all this updating.

After downloading this new archive and extracting and running:

```
./flash-all.sh
```

It worked!

It ran through all the flashing commands and my Nexus was alive again.  

So that was quite a lot of text, the tl;dr is that to fix a Nexus 7 from "No Command" boot loop...

1.  get fastboot/anrdroid sdk tools
2.  get the factory image from the last good version you had
3.  run

And that should be it.  After restoring I obviously lost my root so I will post in a follow up how I managed to do that.
