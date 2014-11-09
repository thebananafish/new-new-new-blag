---
title: migrating from Citrix Xen server
---
Last night I noticed all of my home virtualized servers were not responding.  Uh-oh.  I have 6 small virtualized servers running on a Citrix Xen Server and something is up.  Xen Center (the console) isn't responding so after a few reboot attempts and flaky connections to the server I finally decide to connect a monitor to see what is up.

Turns out I lost the disk to the server.  Now if I really cared about this server I would have used some form of RAID to prevent this from totally going down.  That's another story and since I have another server that does nightly backups nothing was really lost.

But now that I lost my Citrix Xen install I am taking the time to reevaluate if I really want to go down the same road again.  Let me fill you in on the basics of this server of mine, this one was built with mostly spare parts and served as my 24/7 bitcoin miner several months prior.  At some point I added vmware workstation to the server so I could virtualize services as well as continue to mine bitcoins (needed windows for those radeon drivers).  Well I finally stop mining entirely and decide I want a "real" hypervisor on this machine.  It just so happens Citrix Xen GPL'd their distribution of Xen and the tools for it.  Great! So that is what I went with and have been running for around 6 months now.

For the most part it has been okay, not great, not bad but nothing amazing.

Things I liked about Citrix Xen:

-  Performance was very good.

-  Pre-made templates for some distributions were very nicely tuned

-  Not a blackbox hypervisor, Citrix Xen is based off Redhat so the hypervisor OS has some tools built right in (like iptables).

- Lots of unique features, enterprise features (great but didn't need most at home).

 

Things I did not like:

-  Some things are overly complicated, like adding another storage group.

-  All those unique features require paying Citrix or having a service contract.

-  If you aren't paying for Citrix Xen, everything is harder.  Instead of using the update tool in XenCenter to install updates, you have to manually work with the hypervisor shell.

-  Open Source - more like trialware, despite being free as in freedom you still feel limited with their base version.

-  If there wasn't a default template for the big 3 (Ubuntu, Redhat, Windows Server) Xen would struggle with it, lots of kernel panics for BSD.

 

So why didn't I initially just use esxi, kvm, virtualbox?  Well Citrix Xen seemed really nice at first, and it was new I wanted to give a whirl.  Basically Citrix Xen has a lot of neat features but be ready to pay for them even though its GPL.

 

So where am I going from here....? I'm still thinking.  I am leaning toward a small distro (gentoo or debian) with KVM/QEMU setup, but I am not yet ruling out going to esxi.  I will make a follow up post once I have decided and got it setup.

