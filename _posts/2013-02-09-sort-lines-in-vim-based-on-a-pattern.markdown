---
layout: page
type: text
title: Sorting Lines in Vim Based on a Pattern (And More Adventures with Snownews and Newsbeuter) 
quote: 
categories: 
- program
date: 2013-02-09 15:05:00 UTC
---
I didn't know how to sort lines in Vim by an arbitary pattern, but figured it would be possible and pretty easy. And it is:

{% highlight vim %}
:sort /||\w*|/ r
{% endhighlight %}

I've been playing about with [Snownews](https://kiza.eu/software/snownews/) in lieu of [Newsbeuter](http://newsbeuter.org) (I keep going back and updating [this post about command line apps]({{ site.baseurl }}2012/03/17/To-the-command-line-batman.html)) and I'd sorted my list of feeds alphabetically, but wanted to get them back to being sorted by category. The good thing about Snownews is that it is very unixy: text in/out and text config files. So although Snownews doesn't offer a sort by category feature all you have to do is open up the `.snownews/urls` file and sort it how you want. Snow news uses two `|` characters after the url to mark the start of the categories and another `|` at the end, so all the above does is sort based on that bit of text.

On a related note, I found this [old rant by Zed](http://www.zedshaw.com/essays/i_want_the_mutt_of_feed_readers.html) about Snownews. I thought it was funny because it really doesn't take that much effort to add the "crappy" [Atom xsltproc filter](https://kiza.eu/software/snownews/snowscripts/extensions/script/atom2rss/) to all Atom feeds, especially since you can just open up the urls file and text edit away. 

I would still like to get Newsbeuter running,  just *because*. It also looks like the thing about [NetBSD being unsupported](https://github.com/akrennmair/newsbeuter#readme) is unfounded because [there is a pkg here](http://pkgsrc.se/wip/newsbeuter). Anyway, I think I've finally figured out why it doesn't build on OSX 10.5 PPC: Basically because I'm using an out-of-date version of the C++ standard libraries. I.e. I get [errors relating to the hashtable file](https://pinboard.in/u:atomicules/b:8343c0ccb1b1). Unfortunately the fixes in those links don't solve my issue, but they did make me realise that Newsbeuter is probably using things not available in the version of the C++ standard libs I have; I tried to install gcc46 (to get more upto-date C++ standard libraries) from Macports, but I just don't have enough disk space for the build and install. So with Snownews I shall be sticking for the foreseeable future.

My biggest annoyances with Snownews are the [content encoded](https://kiza.eu/software/snownews/snowscripts/extensions/script/contentencoded/) issue and that I can't seem to reliably add a keybinding for Space that does "enter" and *not* page down as well. But it's hardly a deal breaker for me. 

