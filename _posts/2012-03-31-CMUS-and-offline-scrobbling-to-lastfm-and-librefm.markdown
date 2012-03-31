---
layout: page
type: text
title: CMUS and offline scrobbling to Last.fm and Libre.fm
categories:
- code
---
As [mentioned here]({{ site.baseurl }}2012/03/17/To-the-command-line-batman.html) I've (somewhat) recently moved to using CMUS instead of Rythmbox. Ever since my iPod died completely I'd been relying on a desktop player and offline scrobbling (since I am most likely to listen to music at work, but can't scrobble because our proxy server blocks it). Rhythmbox supports offline scrobbling (hurray!), but is a bit crap about it, (boo!): For unknown reasons it doesn't log all tracks and - although it did successfully scrobble them to Last.fm when back on line - when I decided I'd move to Libre.fm (but then reconsidered and kept my last.fm account as well) it was even crappier at scrobbling logging a different number of tracks for each service and then losing all tracks for Libre.fm during submission!

I'd checked (before getting carried away with the geek attractiveness of it) and [CMUS supported scrobbling](http://cmus.sourceforge.net/wiki/doku.php?id=status_display_programs#audio_scrobbling_to_eg_lastfm_or_librefm) via a few external plugins/scripts and a couple of them also provided offline support. The only one I could get working, however, was [Post.FM](http://nex.scrapping.cc/post-fm/). Well, I say working, but I'd declared it as working after a quick online test. After a weeks offline scrobbling, and months of scrobbling downtime (I always think of these things such as moving from Rhythmbox to CMUS and Last.fm to Libre.fm as little weekend tasks, but they always take me MUCH longer) I was full of anticipation for getting going with scrobbling again. But for whatever reason it wouldn't scrobble with a big backlog of offline scrobbles. I almost just gave up, but then figured I'd try to get my head around Perl enough to convert the Post.FM cache to a format that I could use another programme or service to scrobble with, such as the [libreimport.py](http://bugs.foocorp.net/projects/librefm/wiki/LastToLibre) tool. 

And that is kind of where I am today:

1. Using Post.FM (unmodified until I figure out Perl more) to cache offline scrobbles
2. Using a [complementary Perl script](https://github.com/atomicules/post-fm) (work in progress) to convert the Perl Storable cache format to a [scrobble.log format](http://www.audioscrobbler.net/wiki/Portable_Player_Logging).
3. Submit scrobbles to Last.fm and Libre.fm via [Scroblr.net](http://scroblr.net/) (for the time being. I'd prefer to use a command line tool, but Scroblr is quick and painless so I'm in no rush; my initial plan was to use `libreimport.py` and forward scrobbles to Last.fm from Libre.fm, but it didn't work out right and I hit [this bug with multiple scrobbles](http://bugs.foocorp.net/issues/765) appearing on Last.fm, hence finding Scroblr.net)
