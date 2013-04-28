---
layout: page
type: link
title: NetBSD Notes
link: http://simp.ly/publish/vT2pPF 
categories: 
- program
---
Because I'll likely never bother to get it in a proper blog post format, I'll just link to the notes I've made whilst switching to NetBSD. When I say switch, I mean replacing my OSX 10.5 PPC "shell account" with a NetBSD i386 XEN one and getting my base install of software up and running:

- [Mutt](http://www.mutt.org/) (Binary package via Pkgsrc works fine for me, need to ensure `LC_CTYPE` and `LC_MESSAGES` are set to UTF-8 though)
- [Offlineimap](http://offlineimap.org/) (Pkgsrc binary works fine)
- [Notmuch](http://notmuchmail.org/) (Need to do from source. I went for the same version I used under OSX (0.13) as that matches same version requirement for `talloc` that Pkgsrc provides. Got a bit confused with `gmime` as could only find older version for awhile in Pkgsrc)
- [TTYtter](http://www.floodgap.com/software/ttytter/) (No problems, just needed some Perl packages via Pkgsrc for some of the extensions I use)
- [Remind](http://www.roaringpenguin.com/products/remind) (No problems via Pkgsrc)
- [Vim](http://www.vim.org/) (Needed to do from Pkgsrc source for Python and Ruby support)
- [PWMan](http://pwman.sourceforge.net/) (Built fine! Even including my tweaks)
- [Abook](http://abook.sourceforge.net/) (No problems)
- [Elinks](http://elinks.or.cz/) (Built via Pkgsrc source to enable gopher support)
- [Snownews](https://kiza.eu/software/snownews/) (Is proving a bit fiddly due to Curses display glitches, see the linked notes)
