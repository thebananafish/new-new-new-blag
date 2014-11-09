---
title: migrating domains with puppet
---
I use puppet to manage configurations and deployments on nearly all of my servers.  It's a great utility and the one I have found to work the best compared to ansible and chef. 

migrating domains with puppet

With my recent move of bananafish.es to bananafish.in I had to move my puppet setup to the new domain.  This posed a challenge as this process is not very documented as most people will probably never need to do this.  So I am outlining what I did...

**First on the puppet master:**  
I revoked all the client certs... *in hindsight I probably did not need to do this, though its nice to start fresh I suppose*.  And then stopped the puppet master daemon.
```
puppetca --revoke --all
puppet cert clean --all
/etc/init.d/puppetmaster stop
```
Now I had to clean up all the old certs out for the puppet master that still referenced the .es domain.
```
find $(puppet master --configprint ssldir) -name "$(puppet master --configprint certname).pem" -delete
```
Finally I need to generate a new puppet master cert by running:  
**Note:** *You should stop the temporary puppet master with ctrl-C after you see the “notice: Starting Puppet master version 2.x.x” message.*
```
puppet master --no-daemonize --verbose
```
Now the puppet master daemon can be restarted
```
/etc/init.d/puppetmaster restart
```

**Now onto the clients:**
First I am going to change the domain in the main config:
```
sed s/puppet.bananafish.es/puppet.bananafish.in/g /etc/puppet/puppet.conf > /etc/puppet/puppet.conf-new
mv /etc/puppet/puppet.conf-new /etc/puppet/puppet.conf
```

clean out the certs:
```
rm -rf /var/lib/puppet/ssl/*
```

Then ask for a new cert
```
puppetd --test --server puppet.bananafish.in --noop
/etc/init.d/puppet restart
```

**Back to the server**  
Now with all these servers sending cert requests to the puppet master we have to accept them:

```
puppet cert sign --all
```

And that's it, everything else with puppet fell back into place.

*further reading*  
[[1]](http://fvtool.wordpress.com/2013/04/15/exiting-no-certificate-found-and-waitforcert-is-disabled-installing-puppet/)
[[2]](http://docs.puppetlabs.com/guides/troubleshooting.html#agents-are-failing-with-a-hostname-was-not-match-with-the-server-certificate-error-whats-wrong)
