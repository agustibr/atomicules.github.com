---
layout: page
type: link
title: Snownews Keybindings Patch
link: https://gist.github.com/atomicules/5078912
categories: 
- program
---
A small patch I made to the [Snownews](https://kiza.eu/software/snownews/) keybindings so I could move through menus more like I can in mutt, i.e: Space acts as Enter in list views, but when reading an actual item Space acts as Page Down.

It was [bugging me]({{ site.base_url }}2013/02/09/sort-lines-in-vim-based-on-a-pattern.html) that even though I'd set Space as Enter in `~/.snownews/keybindings` it would do page down and then enter. I didn't realise at the time that was because some of the keybindings are hard coded in.
