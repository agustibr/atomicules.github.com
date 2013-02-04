---
layout: page
type: text
title: Lastpass to PWman for Good
categories: 
- program
---
My Lastpass premium membership is/was up for renewal and at a measly $12 a year it was a no-brainer. Well, for anyone with anything approaching a sane life, I'm sure. For me, I had to weigh up that ~£7.60 very carefully and decide that I couldn't justify it and could ill afford it - that's quite a few bags of porridge.

Since I no longer use a supported web browser, nor even any browser that supports bookmarklets and have long since no longer owner a smart phone (well never, but I did have an iPod Touch once) I don't really need or use Lastpass. I've nothing against the service and think it's great, but I've decided to delete my account (because I don't want to be a freeloader) and just use PWman full-time (which is pretty much what I do anyway).

As far as providing myself with the same benefit of Lastpass - which is secure access pretty much from any computer to all my passwords, I feel I can mimic the functionality quite well: Carrying my private key with me is similar to using SESAME combined with the Lastpass master password, and as long as I can access the internet I can either access PWman itself (via a portable version of Putty) or at minimum access my PWman database file. And since the PWman man database is just GPG encrypted plain text I just need to make sure I have [GPG available](http://gpg4usb.cpunk.de/index.html).

In the process of moving, I made a [few more tweaks](https://github.com/atomicules/lastpass2pwman/commit/77ff43681e32fb1f9124fd55e306fab0e20d3beb) to my lastpass2pwman export script as I realised that the password field in PWman is limited to 64 characters so I've stuck Secure Notes in the Launch field instead.
