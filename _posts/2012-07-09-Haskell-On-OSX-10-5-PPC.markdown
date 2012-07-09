---
layout: page
type: text
title: Some Notes on Getting Haskell Running on OSX 10.5 PPC
categories: 
- code
---
Some brief notes on how I got Haskell up and running on OSX 10.5 PPC. They might not be complete as I'm always overly optimistic about my ability to recall and/or write things up promptly.

1. Yay! Some kind soul (Michael Witten) put together a nice [community edition of GHC 6.10.1](http://www.haskell.org/ghc/download_ghc_6_10_1#macosxppc). I think the only thing I had to do was create a symbolic link in `/usr/local/lib` for `libgmp.3.dylib` to my macports installed version. It's not the latest and greatest version of GHC, but is more than enough for my dabblings at the moment.
2. I then followed the [steps here to get cabal-install](http://book.realworldhaskell.org/read/installing-ghc-and-haskell-libraries.html#id689017) built and installed. The versions I ended up using, that worked well together, were:
	- [Cabal-1.10.2.0](http://hackage.haskell.org/package/Cabal-1.10.2.0)
	- [HTTP-4000.2.3](http://hackage.haskell.org/package/HTTP-4000.2.3)
	- [zlib-0.5.3.3](http://hackage.haskell.org/package/zlib-0.5.3.3)
	- [cabal-install-0.10.2](http://hackage.haskell.org/package/cabal-install-0.10.2) 

	And these I'm not strictly sure I needed:
	- [mtl-2.1.1](http://hackage.haskell.org/package/mtl-2.1.1) (installed for HTTP-4000.2.3, but in hindsight I don't know for sure if I needed this)
	- [transformers-0.3.0.0](http://hackage.haskell.org/package/transformers-0.3.0.0) (Installed for mtl-2.1.1, but same applies as above)

	For `zlib` I had to tweak the `zlib.cabal` file to point at my Macports libraries, i.e. find the `include-dirs:` line and append ` /macports/lib`.

	These were actually found with a bit of trial and error: I started with the most recent versions and tried to build them and any new dependencies since the Real World Haskell article, and when I hit a problem I downgraded (which also meant downgrading other packages I'd already installed because of inter-dependencies). I just [installed them all in my home](http://book.realworldhaskell.org/read/installing-ghc-and-haskell-libraries.html#install.pkg.manual) directory.
3. After getting `cabal-install` built and installed I thought it would be plain sailing installing the rest of the packages I needed for my [HaskerDeux]({{ site.baseurl }}2012/07/04/HaskerDeux.html) app. And it was -  once I learnt to point it in the direction of my Macports libraries, etc: 

	{% highlight bash %}
	cabal install 'split' --extra-include-dirs=/macports/include --extra-lib-dirs=/macports/lib
	{% endhighlight %}

	You can also install specific versions with Cabal, like so:

	{% highlight bash %}
	cabal install 'json-0.4.4'
	{% endhighlight %}

	Older versions of Network.Curl and Text.JSON  seem to work fine with HaskerDeux (I'm not exactly pushing the boundaries). These are the ones I installed:
	- [curl-1.3.5](http://hackage.haskell.org/package/curl-1.3.5)
	- [json-0.4.4](http://hackage.haskell.org/package/json-0.4.4)
	- [split-0.1.4.3](http://hackage.haskell.org/package/split-0.1.4.3)

4. There is no step 4. Althought I suppose the next thing I could do is look to see if I can use this version of ghc to help my build a more recent one, but I don't really see a big need for me to do that.
