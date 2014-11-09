---
title: new home fileserver
---
Okay, first I realize I have not posted in awhile and that of course is due to the fact that I am so damn busy as of late.  I still have a few more blog posts I know need to get done but still need to find time to write them.

I thought it would be fun to write a little article and post some pictures regarding some new parts I bought to replace/upgrade my primary home file server.  First let me explain the background on some issues I was experiencing.

Background & Issues
First off this server was starting to show its age, I built this originally as a freenas box in 2009 with just a few drives and a cheap intel CeleronD and 512Mb of RAM.  I kept upgrading it and adding drives, ram, and cheap sata addon cards until it became what you see below...

![](http://bananafish.in/files/img/blog/oldfs-1.jpg)

Over the years this server ran FreeNAS, Debian, and FreeBSD.  It was always purely a fileserver as I used other servers to run other things.  

This fileserver has stored many things but now typically served media files like TV and Movies (gathered off of usenet from another server) over SMB to the pair of raspberry pi in our living room and bed room.  This worked very well as we only suffered very occasional buffering from who knows what.  At its latest state this server ran FreeBSD 9.1 and had two zfs pools to store media and ISOs/programs.

This server has 8 drives in it currently 4 1TB drives and 4 2TB drives bouth in raidz pools.

Up until about a month ago this server would routinely lock up where I would have to come in and reboot the machine and/or perform a fsck on the root drive (a usb flash drive) well recently I tried to fsck this drive and it was shot, could not read from any machine and all that.  So I set it up with another flash drive still, frequent lock ups and nothing in the logs, I noticed some of the other usb ports on this server were acting strange as well.

So I decided it was time to retire this beast after a good four years in service.  I was ready also for a better system to better utilize ZFS' advanced features.

Speccin' it out

So for the past week I have been looking for what I would replace this with and decided on a few things: definetly more ram, something inexpensive it would only be used for file serving, and something with 8 sata ports as I did not want to have to keep using these addon cards.

What I decided on.

[Motherboard - AsRock Z77 Pro4M](http://www.amazon.com/LGA1155-Z77-CrossFireX-Motherboard-PRO4-M/dp/B007RS70YW/ref=sr_1_1?ie=UTF8&qid=1376264966&sr=8-1&keywords=asrock+z77+pro4+m)  
[CPU - Intel i3 3220T](http://www.amazon.com/Intel-i3-3220T-Dual-Core-Processor-Cache/dp/B0093H8JCM/ref=sr_1_1?ie=UTF8&qid=1376264948&sr=8-1&keywords=Intel+Core+i3+I3-3220T)

Preperation

ZFS is great all I had to do on the original server was zpool export each drive and they were good to go.

Finished product

Photos of the finished server "move".  All I had to do was move out the old motherboard and drop in the new one. Was easy and I know its a bit messy but maybe one day I will get in there and clean it up a bit more.

![](http://bananafish.in/files/img/blog/newfs-1.jpg)
![](http://bananafish.in/files/img/blog/newfs-2.jpg)
![](http://bananafish.in/files/img/blog/newfs-3.jpg)
![](http://bananafish.in/files/img/blog/newfs-4.jpg)
![](http://bananafish.in/files/img/blog/newfs-5.jpg)
![](http://bananafish.in/files/img/blog/newfs-6.jpg)
![](http://bananafish.in/files/img/blog/newfs-7.jpg)

All I had to do was install FreeBSD 9.1 to the new flash drive I bought, copy over my configs and install samba.


Other News:
I got my gpu back from nvidia about 2 or 3 weeks ago, so far so good.

