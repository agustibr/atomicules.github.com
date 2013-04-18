---
layout: page
type: text
title: In Lieu of GPG Agent Forwarding
categories:
- program
---
[ssh agent forwarding](https://help.github.com/articles/using-ssh-agent-forwarding) is really neat, and since I've now moved to my "main OS" being remote and "in the cloud" (more on that in another post soon - that's what I've been busy doing this past couple of weeks) it makes a lot more sense than leaving private keys on a remote server; when I was using my Powerbook in the same way, at least I still had physical access to it.

However, as neat and clever as ssh agent forwarding is, I don't really care that much about a private ssh key to Github. I'm *much* more concerned with my private GPG key being on the same remote machine as the password file it's encrypting.

Unfortunately, GPG Agent forwarding doesn't seem to exist. This is the [nearest thing I've found to a solution](http://superuser.com/questions/161973/how-can-i-forward-a-gpg-key-via-ssh-agent).

I thought about doing some kind of reverse sshfs mount (Yay for NetBSD, it comes with [mount_psshfs](http://netbsd.gw.com/cgi-bin/man-cgi?mount_psshfs+8+NetBSD-6.0+i386) built in) so I could have my `.gnupg` directory symlinked back to my local machine, but it really hurt my head thinking about it and I couldn't get a reverse ssh tunnel to work anyway (probably something to do with my firewall on the NetBSD box - I'm purposefully being restrictive with it, but I do seem to making things difficult for myself as result).

So a quicker, easier way for me to achieve what I want is a little script I can run locally to copy across my GPG key when needed. After copying it across the script pauses and waits for my input so I can do whatever I need with it on the remote side and then as soon as I press return it deletes it. I have it set up as an alias in bash:

	alias cpkey="scp ~/.gnupg/secring.gpg remotemachine.com:/home/me/.gnupg/; awk 'BEGIN{getline}'; ssh remotemachine.com rm /home/me/.gnupg/secring.gpg"

The `awk 'BEGIN{getline}'` is a little trick I found somewhere on the internet as a way of pausing a bash script. On a related note, it should mean it'll work when I eventually figure out switching from bash to [mksh](http://mirbsd.de/mksh) (just because).
