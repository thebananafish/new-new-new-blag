---
title: mining again pt. 2
---
Okay so you saw the [previous post](http://blog.bananafish.in/mining-again/) you know I got back into cryptocurrency mining.  In this post I plan on going into the nitty-gritty details of getting a working mining rig going.

This was a bit complex and I will try to explain all here but if you are not comfortable with compiling software manually on Linux operating systems you may want to just run Windows on your miner.

A couple things to keep in mind:

*   I used Debian GNU/Linux 7 (this is also a virtual server host)
*   You **DO NOT** need to have a monitor or those dummy plugs hooked up to your GPUs
*   I am hosting the files used in this tutorial/post because they are old versions and may disapear from the net, if you do not trust them you can google their file names to find them.

####First
I started with a fresh install of Debian 7 (you can get the .iso [here](http://cdimage.debian.org/debian-cd/7.3.0/amd64/iso-cd/debian-7.3.0-amd64-netinst.iso))
Run through the installer until you get to the "tasksel" part that looks like below:
![](http://bananafish.in/files/img/blog/tasksel.png)

And make certain that only **SSH server** and **Standard system utilities** **AND Debian desktop environment** are checked.

####Next
Okay so you should have Debian all fresh and installed.  Let's install some pre-requisite software and nice things to have.  Also make sure you do all this as the *root* user or with *sudo*

```
apt-get update  
apt-get install git-core build-essential vim htop tmux screen wget curl cdbs dh-make dkms execstack linux-headers-amd64 linux-source fakeroot libtool unzip libcurl4-openssl-dev libncurses5-dev pkg-config automake yasm  
```

*Note: while this is going to be a headless server amd drivers or cgminer do need xorg-server*

Now install the amd proprietary drivers

First we remove the drivers that Debian probably installed:

```
/etc/init.d/gdm3 stop  
modprobe -r radeon  
apt-get purge xserver-xorg-video-radeon libdrm-radeon1 radeontool  
modprobe -r drm  
```

and blacklist the default driver

```
nano /etc/modprobe.d/blacklist.conf
```

and add:

```
blacklist radeon  
blacklist radeonhd  
```

then `reboot`

Once back up we will actually install the driver.
*Note: this is not the latest driver as my rig uses 5850s and this driver works best for me, your results may vary*

```
mkdir working && cd working  
wget http://bananafish.in/files/ltc/amd-driver-installer-12-8-x86.x86_64.zip  
unzip amd-driver*  
chmod +x amd-driver-installer-8.982-x86.x86_64.run  
./amd-driver-installer-8.982-x86.x86_64.run --force
```

The amd driver installer will prompt you for a few questions and then `	reboot` your rig again.

####Now the AMD SDK
Now we will install the next piece of this rig:

```
wget http://bananafish.in/files/ltc/AMD-APP-SDK-v2.7-RC-lnx64.tgz  
tar -zxvf AMD-APP*  
./Install-AMD-APP.sh
```

Then `reboot` again.

Next we will install **cgminer**, this miner can be used to mine basically any type of cryptocurrency.
*Note: recent versions of cgminer have dropped support for everything BUT asic mining devices, this is why this post uses an older version*

```
wget http://bananafish.in/files/ltc/cgminer-3.7.2.tar.bz2  
tar xvjf cgminer*
```

We also need another AMD SDK for cgminer for it to work right.

```
wget http://bananafish.in/files/ltc/ADL_SDK_5.0.zip  
mkdir ADL_SDK  
cd ADL_SDK  
mv ../ADL_SDK_5.0.zip .  
unzip ADL_SDK_5.0.zip  
cp include/* ../cgminer-3.7.2/ADL_SDK  
```

Now we will acctually build cgminer:

```
cd ../cgminer*  
CFLAGS="-O2 -Wall -march=native -I /opt/AMDAPP/include/" LDFLAGS="-L/opt/AMDAPP/lib/x86_64"   ./configure --enable-opencl --enable-scrypt  
make  
```

Put in your `~/.profile`

```
export DISPLAY=:0  
export GPU_MAX_ALLOC_PERCENT=100  
export GPU_USE_SYNC_OBJECTS=1  
export XAUTHORITY=/.Xauthority  
```

Congrats, the hard work is finally done.  You can test **cgminer** by running

```
./cgminer -n
```

To actually start mining you will need to join a pool and start **cgminer** similar to as follows:

```
./cgminer -o http://pool:port -u username -p password --api-listen --api-network -I 9 --gpu-reorder --auto-fan --gpu-powertune 20 --gpu-engine 920,920,920,1125 --gpu-memclock 795,795,795,975  
```

I would recommend running cgminer in a `tmux` or `screen` session so that it can be checked on and easliy restarted.


##Troubleshooting
Now if this didn't work for you that is probably okay, it took several attempts and trial and error to get this right for me.

You can try a few things:

uninstall the drivers and try to reinstall them:

```
sh /usr/share/ati/fglrx-uninstall.sh
```

also see if the driver is detecting all your cards

```
fglrxinfo
```

##Conclusion

I plan on cleaning this article up and checking it a few more times for consistency but it should be pretty close.  Google and syslog are your friends with this sort of thing.  Good Luck!
