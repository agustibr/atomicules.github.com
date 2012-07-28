---
layout: page
type: text
title: Scrobbling to libre.fm - Update
categories: 
- programming
date: 2012-06-04 13:15:00:00 UTC+1
---
Another little update post. Annoyingly, as soon as I got a [nice working solution]({{ site.baseurl }}2012/03/31/CMUS-and-offline-scrobbling-to-lastfm-and-librefm.html), scrobbling to [libre.fm](http://libre.fm) via [Scroblr.net](http://scroblr.net/) seems to have stopped working. I'm not sure why. I contacted the guy who runs it, but as of right now it is still not scrobbling to libre.fm for me. 

So rather than go back to producing [two separate log files via my post-fm modification](https://github.com/atomicules/post-fm/blob/47831cd231c7d561d2f28a0586174f2b79927543/cache2offlineimport.pl) I've tweaked the Libre.fm [libreimport.py](https://gist.github.com/2862764) file to work with the scrobble.log format. So I am now back scrobbling once again.

_(As an aside, I'm not entirely sold on the usefulness of Libre.fm. I was hoping it would recommend me free artists in a similar vein to how Last.fm provides recommendations in general, but it doesn't seem to do this. Although weirdly does suggest free artists my [neighbours](http://libre.fm/user/ruunix) might like - perhaps it is because I haven't listened to any free artists yet?)_
