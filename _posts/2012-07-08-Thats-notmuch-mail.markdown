---
layout: page
type: quote
title: 
quote: Found 25944 total files (that's not much mail)
categories: 
- code
date: 2012-07-08 21:10:00:00 UTC+1
---
_Actually, I think you'll find it is_

> Processed 25944 total files in **1h 50m 39s** (3 files/sec.).

_See, told you so_

Ok, so my ageing 1GHz G4 was never going to be the quickest at indexing all those emails. However, the fab thing is that it could. As far as I can recall I had no trouble building [notmuch](http://notmuchmail.org/) mail at all, which always comes as a nice suprise on PPC architecture and an OS that is now mostly forgotten.

The only stumbling block I had was getting [mutt-notmuch](http://upsilon.cc/~zack/blog/posts/2011/01/how_to_use_Notmuch_with_Mutt/) (now [notmuch-mutt](http://upsilon.cc/~zack/blog/posts/2012/03/mutt-notmuch_is_dead/)) working, which had worked fine for me on Linux.

1. First of all I needed to install [Term-ReadLine-Gnu](http://search.cpan.org/~hayashi/Term-ReadLine-Gnu/) for Perl. This is mentioned in the [README](http://git.notmuchmail.org/git/notmuch/blob_plain/HEAD:/contrib/notmuch-mutt/README), but for those of you who don't read READMEs properly, like me, it is easy to miss (and not understand the resultant error).
2. The verison of `xargs` which ships with OSX 10.5 is too old it seems as it doesn't support the `--no-run-if-empty` option. So I installed [findutils via macports](https://trac.macports.org/browser/trunk/dports/sysutils/findutils/Portfile) for a more recent version, but it gets called `gxargs`. 
3. The next problem is with the version of `ln` that ships with OSX as it doesn't have the `-t` target directory option. Installing [coreutils via macports](https://trac.macports.org/browser/trunk/dports/sysutils/coreutils/Portfile) gets you `gln` which does. 
4. I then just edited the [notmuch-mutt script](http://git.notmuchmail.org/git/notmuch/blob_plain/HEAD:/contrib/notmuch-mutt/notmuch-mutt) to use these macports versions.

All of this may be incredibly obvious and easy, but it took me weeks to figure out each little bit because of limited debugging time and so I'm bloody well writing it down so I remember for next time.

The good news is that once that massive amount of indexing was done, incremental indexing is fine and searching is really amazingly fast (even on a Powerbook). Mutt plus Notmuch is a great combination.
