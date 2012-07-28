---
layout: page
type: text
title: Until I can afford Linode
categories: 
- programming
---
I liked this post awhile back about [using an iPad and Linode as a "main" computer](http://yieldthought.com/post/12239282034/swapped-my-macbook-for-an-ipad), especially the Linode bit. I really like the idea of having my computer be a remote one (that'll stop the kids and wife hogging it!) that I can connect to from wherever, and I'm more than happy using command line apps for everything. But since I can afford neither an iPad nor Linode I just filed it away in the depths of my memory under the "if I win the lottery" category.

However, when my work machine recently died, I realised that although using VirtualBox to enable me to use an OS of my choice was not such a bad idea, treating the VirtualBox instance as a personal machine was not such a clever move. I'd been using Mutt and OfflineIMAP on there and had _ALL_ my personal emails on there. They were backed up elsewhere - that wasn't the issue, rather that I had to then give my machine to someone in IT to fix and failed hard drive or not, storing that much personal info on a work machine is STUPID.

So, recalling the Linode/iPad article, I decided it made far more sense to do that kind of thing on a "server" and that server is now my home machine. SSH and [Tmux](http://robots.thoughtbot.com/post/2641409235/a-tmux-crash-course) are great. I can connect in and I have Mutt running in one pane, TTYtter in the other and I can even do a spot of bypass-the-work-proxy browsing via elinks/w3m in another window. There are just a couple of weird things about this: 1) Is having to "ssh localhost" (and have a key!) when sat on the local machine itself to connect to the tmux session. 2) is ssh-ing to my home machine to reply to emails from my wife who at that moment in time is sat on that machine and yet the emails will be sent half way round the world (probably) via Gmail. Both of which seem a bit silly, but it does work.

_Still trying to find time to write proper blog posts_ 

