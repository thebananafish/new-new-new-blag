---
title: home file server re-do
---
With this extended weekend I finally had a bit of time to start and finish a project I have been meaning to get on for awhile.  This project was moving my home fileserver over to FreeBSD, in order to take advantage of ZFS.

With this move I had to move some hard disks around as well, so that the final product was a FreeBSD 9.1 Server with 1 zfs pool of 4 1TB drives and another of 4 2TB Drives.


Some back story:
I use usenet to get all the media my girlfriend and I watch.  This media is pulled automatically with sickbeard and couchpotato, and runs on a seperate slackware virtual server.  Right now this server dumps and post-processes the media on smb shares from mine and my gf's local computers (depending on the show or movie).

This was cumbersome and dumb, and with the summer coming we would have to leave out PCs on to watch media from other computers or the RasPis I have hooked up to the TVs in the house.

At this time I also have a fileserver running that stored the software, roms, and ISOs I have collected over the years.  This server stored this data on a mdadm RAID5 array across 7 1TB drives running Debian.

I also have had a small amount of zfs experience before and realy wanted to get my fileserver back to being able to use that.

Plan:
My overall plan was to reassign the fileserver to store our media instead of all my other stuff.  In my own computer I had another RAID5 array of 4 2TB drives storing my own media like ebooks and video and music.  This set of disks I would move into the fileserver and remove the OS drive and 3 1TB drives out of the server.  I would then store the OS for the new server setup on a 32Gb flash drive.  This would leave the fileserver setup with 1 Flash drive for the operating system, and then 8 disks totalling ~12TB of storage.

Obviously before moving all this around I backed up my media off the array in my computer and the previous fileserver files.

Issues:
FreeBSD is a very nice operating system.  I only experienced 2 major issues with this whole migration.  The first issue was installing FreeBSD, I was trying to install off a USB CD-ROM drive which kept causing a kernel panic on boot.  I resolved this by temporairly plugging in a spare E-IDE DVD-ROM drive.

Next was adding the spare disks to zpools. This was not a real issue with FreeBSD but rather something with my disks.  I tried dban and gparted but both would crash when I would attempt to clear them (to clear the previous RAID 'stuff').  I resolved this by a bit of a round-about method; 1.  booted off a windows 7 disc and formatted the spare drives as NTFS 2. rebooted and booted off gparted, then this allowed me to finally delete the partition table.  Once I had these two side-steps done I was able to cleanly add these drives to their respective zpools.

Conclusion:
In the end everything was done in about a day, and it took an additional day to get all my media back on to the new server setup off of backup disks.
All I had left to do was finish tweaking my samba config (for the network shares) and adjust the slackware usenet server and the raspis.

I am very pleased with the new setup and even more pleased with ZFS.  

![](http://bananafish.in/files/img/blog/file-work1.jpg)

