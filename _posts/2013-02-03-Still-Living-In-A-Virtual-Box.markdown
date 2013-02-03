---
layout: page
type: text
title: Still Living in a Virtual Box
categories: 
- program
---
I am still [living in a virtual box]({{ site.baseurl }}2011/09/11/It-wasnt-meant-to-be-this-way.html), but a couple of weekends ago finally made the move from Fedora to [Arch Linux](http://archlinux.org/). I'd no real complaints with Fedora, but since first discovering XMonad and then all the [Suckless](http://suckless.org/) stuff I no longer needed a distrubution that came with unecessary "weight" and wanted to do things more minimally and cleaner. Also, if you are going to use the [best Linux wiki](https://wiki.archlinux.org/) out there for troubleshooting, you might as well also use that distro. Really I just need X11 and then the only things I run in that are:

- dwm
- st
- tabbed/surf
- gvim (cus if I'm writing VB code for work gvim makes for easier copying and pasting to the Visual Basic IDE).

Everything else I run in the console:

- R
- vim
- cmus
- ranger

etc.

So, when my hard drive died on my work's laptop and I lost my virtual box install (and so was starting from scratch) I thought that was an ideal excuse to move off an easy distro. I'd actually prefer to use a BSD, but the only BSD supported in VirtualBox is FreeBSD. I did give this a go first of all, and got X11 and dwm up and running, but then chickened out because the version of webkit-gtk was out of date and so I couldn't keep up to date with surf - perhaps a silly reason, but I was pushed for time. So then I tried Arch but the continual distractions at home mean that I actually ditched that for a week and I went with a minimal Fedora net-install (which at least means you don't have to install the Gnome GDM bloat). But then the following weekend I had another stab at Arch and actually managed to find enough uninterrupted time to get it installed an up and running.

I suspect I will like Arch greatly until that time when a system upgrade borks my install and I'll be scuppered at work (because everyone just assumes I run Ruby, R, etc in Windows - no I'm being too kind they haven't a clue how I do what I do). Oh well. I have my home directory in a separate virtual disk, I can run back to Fedora if I get desperate. Already had my first system update and got booted into safe mode because of filesytstem errors. 
