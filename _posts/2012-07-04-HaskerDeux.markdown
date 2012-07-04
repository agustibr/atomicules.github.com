---
layout: page
type: link
title: HaskerDeux
link: https://github.com/atomicules/HaskerDeux
categories: 
- code
---
My second little command line project of the year. This has taken me months because I just don't have a lot of spare time anymore and it is very difficult to learn new languages and code anything when you keep having to dip in and out for a few minutes at a time separated by weeks apart. I suppose, when looked at that way, the fact that I manage to get anything like this done is a remarkable achievement. 

Since I wanted a tool I could use at work and home, authenticated proxy support was a must and so that seemed to knocked out some of the [easier](http://hackage.haskell.org/package/http-conduit)(?) [libraries](http://hackage.haskell.org/package/HTTP-4000.2.3) and left me with [Network.Curl](http://hackage.haskell.org/package/curl-1.3.7). Long story short is that after I finally [figured out coding proxy support](https://github.com/atomicules/HaskerDeux/commit/1844824289593f9698b56ebdde7af4a65d60d0de), I realised I [didn't need to](https://github.com/atomicules/HaskerDeux/commit/1844824289593f9698b56ebdde7af4a65d60d0de#commitcomment-1476439) because it automagically picks it up from the environment variables. This seems to be a [recurring theme](https://github.com/atomicules/simplenote.py/commits/defunct-proxy/) for me!

The other annoyance was figuring out how to use Network.Curl in general and any of the JSON parsing libraries. Haskell libraries seem to be very sparse on useful documentation. Fortunately I found this blog post which [helped me figure out how to use Network.Curl](http://flygdynamikern.blogspot.it/2009/03/extended-sessions-with-haskell-curl.html) and this one which [explained Text.JSON](http://www.amateurtopologist.com/blog/2010/11/05/a-haskell-newbies-guide-to-text-json/). I'd never have got anywhere if it wasn't for them.

I've released it at this point as it "works for me", but I'm well aware it's not super duper code. I'd like to eventually get it so it's a Haskell version of [Badboy's Ruby API library](https://github.com/badboy/teuxdeux).

Lastly, of course I never thought to check I'd actually be able to run the thing on OSX PPC when I started, did I? So far, I have found a [version of Haskell that works (G4, OSX 10.5)](http://www.haskell.org/ghc/download_6_10_1), and I got cabal-install built, but no luck with Network Curl at the moment. Doh!
