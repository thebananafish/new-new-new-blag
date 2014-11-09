---
title: playing with some old sparc boxes
---
So for awhile I have been looking for some old sparcstations to work on, I finally found a pair on ebay in the price range I was willing to pay.

<img src="http://bananafish.in/files/img/blog/sparc1.jpg" style="width: 700px;"/>

Both are circa 1992, and power on and run like champs.  The SPARCstation LX came with a blank hard disk and has a 50Mhz CPU with 32mb of RAM, the other box the SPARCstation Classic has a very old version of SuSE installed which I am still trying to recover the root password as there are not too many Linux distros out there that still support sparc32 architecture.  The SPARCstation classic has 48Mb of RAM and a little bit better CPU too I believe.

Initiall I purchased a serial null modem cable so that I can work on these, as just about every Sun workstation will default over to serial console if there is no keyboard present.

<img src="http://bananafish.in/files/img/blog/sparc4.jpg" style="width: 700px;"/>

Since the SPARCstation LX did not come with an OS I figured I would experiment with this one first.  I grabbed the lastest .fs install for sparc32 from an [OpenBSD mirror](http://openbsd.mirrors.hoobly.com/5.4/sparc/) and used dd to write the .fs file to a floppy disk in a USB floppy disk drive.  After that finished I connected to the serial port on the SPARCstation and entered 
```
boot floppy
```
at the `ok` prompt.  From there I was able to bootstrap an FTP install of OpenBSd 5.4 and withing 30 minutes I had a working OpenBSD 5.4 install on a 21 year old SPARCstation LX!  After that I setup ssh and httpd just for some basic testing.  It took 15 minutes to generate an RSA key for ssh, but other than that this little system is very capable.  It just amazes me that the latest OpenBSD works perfectly on such old hardware.

Anyway I plan on doing a follow up to this post after the special 6 pin Sun Keyboard and Mouse arrive so I can set this up like a desktop with OpenBSD and xorg.  I managed to also purchase an [adapter](http://www.ebay.com/itm/13W3-M-HD15-F-SUN-Ultra-SPARC-workstation-adapter-to-HD15-PC-VGA-monitor-video-/221308853314?pt=LH_DefaultDomain_0&hash=item3387091c42) that lets me hook the special Sun connector to a standard VGA screen.  I am also in the process of hunting down an old SunOS intsall disk for testing on one of these.  So stay tuned for some more old Sun goodness.

Throughout playing with these boxes I found this [OpenBoot command reference](http://docs.oracle.com/cd/E19457-01/801-7042/801-7042.pdf) very useful.
More pics:

<img src="http://bananafish.in/files/img/blog/sparc2.jpg" style="width: 700px;"/>
<img src="http://bananafish.in/files/img/blog/sparc3.jpg" style="width: 700px;"/>

