---
title: b-b-backing up
---
Okay so it has been awhile since I have made a post here, about a month, and lot of crazy shit has happened since then.  
Some of you may have known about the large incident a little more than a week ago involving a lot of VPS providers, and especially one in particluar [Ramnode](http://ramnode.com).
Basically what happened someone managed to erase nearly all of the virtual hosts via a code exploit in SolusVM. Of course this affected nearly all of the projects I run.  This is due to the fact that despite myself knowing it was a bad idea, I proceeded to host most of my "stuff" off 10 ramnode VPS.  I went againjst my gut feeling of doing this because the prices were so good and so was the service.  Anyway TL;DR Ramnode had recent backups of nearly all of the VPS they had though having 10 boxes from them the odds weren't in my favor so after spending hours on their IRC awaiting the verdict it turned out I had lost 3 servers, that would not be restored.

Well I must have kept my OWN backups... right? Wrong, sadly, I was in the process of moving backups away from the paid solution called [Tarsnap](http://tarsnap.com).  Not knocking their service it is great check it out, but I have enough servers and storage that I should be hosting my own backups not paying monthly... so I had recently cleared my backups from Tarsnap so it would stop charging me and in the mean time contemplating what I should use for my backups.  I don't think I have ever "lost" data like this before so I figured I would do a little write up on how I decided on backing up my several VPS now. The method I use is really quick, easy, and pretty standard for backing up *nix servers so if you are happy with your method of backing up now, this post may be of little value to you.

Ok first the backup software/services I have used before:

*  Tarsnap.com - I like this one, it encrypts, it stores on Amazon S3, its a FreeBSD project

*  Crashplan - All these "unlimited" ones are shit 

*  Duplicity - Ran my own servers to store backups using the program duplicity, it encrypts and its nice.

*  Bacula - An enterprise standard, but way to much overhead for my goals.

*  Rsync + SSH - the standard, I started here and ended up coming back

 
-I'm sure there are more that I have tried but cannot recall at the time of writing this.

Anyway so I came back to rsync over ssh for my backups.  Why? Several Reasons:

*  The standard, as in its packaged and available for nearly every platform.

*  I don't need encryption, encryption has that "cool" factor but these projects I am backing up are not that sensitive.

*  It's quick running and to setup.

So here is the backup script I have running nightly on all my servers now:


<pre>
#!/bin/bash
#quick and dirty backup script by bananafish

OFFSITE=/var/offsite/
STAGING=/var/dump/
DATE=$(date +%Y-%m-%d)
SSHPORT=2222
SQLPASS=(yourmysqlpassword)


mysqldump -u root -p$SQLPASS --all-databases > "$STAGING"mysqldumpall.sql
tar -pczf "$OFFSITE"mysqldump.$DATE.tar.gz "$STAGING"mysqldumpall.sql

tar -pczf "$OFFSITE"banana_blag.$DATE.tar.gz /var/banana_blog
tar -pczf "$OFFSITE"banana.$DATE.tar.gz /var/banana
tar -pczf "$OFFSITE"cloud.$DATE.tar.gz /var/cloud
tar -pczf "$OFFSITE"emacs.$DATE.tar.gz /var/emacs
tar -pczf "$OFFSITE"wiki.$DATE.tar.gz /var/wiki
tar -pczf "$OFFSITE"bind9.$DATE.tar.gz /var/bind9/chroot/etc

rsync -avz -e "ssh -p $SSHPORT" $OFFSITE bananaback@127.bananafish.es:/home/bananaback/web1/
rsync -avz -e ssh $OFFSITE bananaback@seed.bananafish.es:/home/bananaback/backups/web1/

rm "$STAGING"*
rm "$OFFSITE"*
</pre>
 
So what does this script do? 
First let me explain the variables OFFSITE is the temp folder that this script is going to dump all the tar.gz archives it makes prior to sending to the backup location.  STAGING is the folder that mysqldump is going to dump the database file to.  The rest of the variables are self explanatory I think.  So the script dumps all your databases to one sql file tars that up, tars any other folders up, see in the example these are all the web projects on one of my servers.  
Once these are all "tarred" up they are sent over ssh to a server at my house (see here in the example my backup server at home isn't running ssh on the standard port, so you can remove that variable entirely for your use) and then sent to my dedicated server (seedbox because lots of storage).  Once it has succesfully sent these backups it cleans out the temp folders used since some of my VPS do not have a lot of disk space.

I then just added it to a nightly cronjob and that's it.

Some prereqs:

Okay a few things to keep in mind, both servers are going to need to have rsync and ssh and something you want to backup.  SSH should already be installed so that is fine but you may have to install rsync via your package manager if it isn't already installed.  

```
apt-get install rsync #for debian/ubuntu
emerge rsync #for gentoo
yum install rsync #for centos, fedora, redhat
```


Then create a user on the source server with permissions to make all the backups (don't run this script as root, pls) and then the same on the destination server.  Lastly we need to generate and copy the public ssh key of the SOURCE server to the DESTINATION server's authorized_keys file, this will allow the servers to rsync the backups over ssh to the destination server without entering a password; this is required for making the script automatically as nobody will be around to enter the password. So:


```
ssh-keygen -t rsa  #on the server with the backups
cat .ssh/id_rsa.pub   #this will output your pubkey, copy this
```

Then shell into the destination server and paste to `~/.ssh/authorized-keys`

This whole process can also be done with ssh-copy-id, but I guess I am a little old fashioned.

Anyway now manually start your script and your backup process will begin.

