---
layout: page
type: text
title: To the Command Line, Batman!
categories: 
- code
---
For reasons that I am unable to properly explain (to myself, let alone others), over the past few months I have been unable to resist the compulsion to investigate and set-up command line <s>mail</s> clients (in general). 

It probably has something to do with low memory requirements, well a lot to do with that, and that would be my reasoning now, but it wasn't like I actually had that foresight that prompted all this tinkering. But, yeah, at home I have the old Powerbook that is maxed out at 1.25GB RAM and always has at least one other account constantly logged in with Safari, etc, sucking a blackhole's amount of swap space, and at work I live in a virtual machine and so only have 1GB of RAM to play with. So low memory applicationss do make A LOT of sense.

Oh, and I guess the other obvious thing is command line applications are more likely to be cross platform.

It started with mail clients...

##Mail

Since I like Vim, I kind of figured out that [Pine/Alpine/Re-Alpine](http://en.wikipedia.org/wiki/Alpine_(e-mail_client) were out of the question (although I belatedly understand it is possible to use Vim with them) and that really that only left [Mutt](http://www.mutt.org/), but I just couldn't bring myself to figure it out. [There's](http://pbrisbin.com/posts/two_accounts_in_mutt) [plenty](http://fsk141.com/my-fear-of-mutt-and-why-it-was-all-for-null) [of](http://mutt.blackfish.org.uk/) [help](http://thomas.pelletier.im/2010/10/low-memory-mail-client/) [out](http://hynek.me/articles/my-mutt-gmail-setup/) [there](http://jason.the-graham.com/2011/01/10/email_with_mutt_offlineimap_imapfilter_msmtp_archivemail/), but the more I read the more tricky it looked. Such was my aversion to knuckling down and figuring Mutt out I managed to find some alternatives:

- [Sup](http://sup.rubyforge.org/) is written in Ruby, is a breeze to setup, easy to use, but by default assumes that you don't really care about synchronising local and remote sources. Which might work fine for folk who are always on the same machine all the time and who also aren't forced to use webmail occasionally. As far as I can tell, if you use local mbox files, then it is possible to sync, but then there also does not seem to be any mbox equivalent to the excellent [OfflineIMAP](http://offlineimap.org/) (which uses maildir). There is a [maildir sync branch](http://gitorious.org/sup/mainline/commits/maildir-sync) someone created, but that would require manaully trying to merge and keep in sync with development.
- [Notmuch](notmuchmail.org), found this as a result of Sup. It's meant to be a re-write of Sup taking the best bits, making them faster and leaving the bad bits, but from my point of view it is almost the opposite. I wanted the mail client (Notmuch isn't one) as Sup has a great UI. There are [some kind of interfaces provided](http://notmuchmail.org/frontends/), but the developers seem to prefer [emacs](http://git.notmuchmail.org/git/notmuch/history/HEAD:/emacs) over [vim](http://git.notmuchmail.org/git/notmuch/history/HEAD:/vim). Poop. 
- [Cone](http://www.courier-mta."rg/cone/bk01-toc.html). Another Pine derivative. I think, well, it must at least be influenced by it, hence its name. Easy to setup, but just was never going to work for me if I couldn't use Vim. 
- [Vmail](http://danielchoi.com/software/vmail.html), and then I found this. Wow! I don't understand how I missed this one. This is the kind of thing I'd been searching for, for ages. I.e. I want a twitter client in Vim, find TwitVim, I want a mail client in Vim. Nada. I just never found this.
Was very excited about this, but it assumes Gmail (in theory [does other IMAP](https://github.com/danchoi/vmail/wiki), but I couldn't get that to work) which isn't so great as I'm trying to de-googlefy and so have moved one email account away from Gmail. Also, I don't want an online only approach and since it needs to connect to an IMAP server (as far as I can tell) I ended up looking at Offlineimap + Dovecot + Vmail. And I started going down this route and trying to get Dovecot configured and then I just decided THIS IS UTTER INSANITY. Running my own IMAP server locally, just to read my mail that is already on an IMAP server somewhere else? It just doesn't seem right, or easy. 
- [Mutt](http://www.mutt.org/) And there we are. Right back to the start and the thing I was trying to avoid all along. It is true what it says on the tin: *"All mail clients suck, this one just sucks less"*. As to configuration, <s>I'll cover that elsewhere, perhaps, but</s>\* it's similar to Vim: you just have to suck it up, figure it out and get on with it.

\* I never could be bothered with writing about my configuration. I just have two points to make: 

1. I wish there was some way to highlight messages marked as deleted with different colours depending on where the messages are being moved to. i.e. really being deleted or just being archived.
2. The most fiddly bit of "Mutt configuration" wasn't mutt at all, but offlineimap. I've saved [some useful bookmarks on offlineimap](http://pinboard.in/u:atomicules/t:offlineimap/); the critical settings for me being `maxsyncaccounts` and the idle folder settings.


##Twitter

I've been [using Twitvim for a little awhile now](https://twitter.com/#!/atomicules/status/83146601383727104) since it makes a lot of sense if you like Vim, plus its unobtrusive at work. I'm now <s>also</s> using [TTYtter](www.floodgap.com/software/ttytter/) (2, currently Beta). <s>They work well together as [TwitVim](http://www.vim.org/scripts/script.php?script_id=2204) is better for paging through Tweets if you are so inclined to catch up on stuff you've missed whilst</s> TTYtter does the realtime/streaming side of things and handles message threads better - and since I've started leaving TTYtter running on a "server" in Tmux it is also very easy to page back through tweets. In TTYtter I use a plugin for [spam reporting](https://github.com/reuteras/my-ttytter-addons/blob/master/report.pl) and one to get occasional [timestamps](https://github.com/stormdragon2976/ttytter-extensions/blob/master/timestamp.pl).

##Web (only at home)

[TenFourFox](http://www.floodgap.com/software/tenfourfox/) is an excellent browser, but it's memory requirements aren't exactly low. So for quickly checking some websites (mainly BBC Weather, until I find/write a command line weather app) [elinks](http://www.elinks.cz/) is great; it even does tabbed browsing! I did have a brief foray with [w3m](https://en.wikipedia.org/wiki/w3m), but I much prefer the UI and usability of elinks.

##Music (only at work)

[CMUS](cmus.sf.net) since Rhythmbox has memory issues for me. CMUS is fab, but I've found getting scrobbling working tricky. And I'll come to that separately, probably, eventually, maybe.

##Calendars

I've switched from Google Calendar to using [Remind](http://www.roaringpenguin.com/products/remind). It works well enough for me. I like the idea of having a plain text file for storing and editing calendar entries. And the simple calendar outout (`-c`) is good enough for me; I don't really feel the need to use [Wyrd](http://pessimization.com/software/wyrd/) as a UI<s>, apart from viewing previous months as that doesn't seem possible with Remind - hmm, I can feel some hacking coming on,</s>as you can view any abitary month with `remind -c .reminders May 2008`, for example, you can also do `remind -c2 .reminders May 2008` which would get you the months of May and June in 2008. I would still like to be able to do `remind -c-1` or similar to view backwards, but since my C skills are pretty crap (I've looked at the source code and can't understand it right off the bat) I guess hacking this in will be very low priority for me.

##Feed reading

I've had a bit of flakiness with my hosting for my [Fever](http://feedafever.com/) install this year and it prompted me to go and look at command line feed readers. [Canto](http://codezen.org/canto/) and [Newsbeuter](http://www.newsbeuter.org/) are both nice. I prefer the UI of Canto, but it is much, much slower and at the time that was on my work machine, so I'm guessing it would be unusable on my G4. However, Canto is the one I got closest to running on my Powerbook, although unfortunately I seem to have neglected to make any notes about what the final issue was that I came across (I think a platform Python incompatibility). With Newsbeuter I couldn't get beyond STFL and since my Fever install started working again I never [persisted with trying to get it working](http://pinboard.in/u:atomicules/b:4f4fa82e045f). So at the moment, I'm still web based for feed reading.

##File Browser/Manager

Almost forgot: [Midnight Commander](http://www.midnight-commander.org/). Although, actually I'm unsold on it's usefulness. Unfortunately the Macports version doesn't seem to have mouse support and driving it purely by keyboard, well - I might as well just do my file management in the shell.

I probably use it more at work as it is much lighter weight than Nautilus, and in Linux the mouse support is fine.
