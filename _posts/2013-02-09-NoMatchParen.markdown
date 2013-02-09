---
layout: page
type: quote
title: 
quote: ":NoMatchParen"
categories: 
- program
date: 2013-02-09 00:49:00 UTC
---
A little Vim performance tip when editing in a remote session. It turns off highlighting of matching parentheses (I would stick it to off by default with `let g:loaded_matchparen=1`, but it isn't possible to then toggle it on). I'd noticed that when moving the cursor along a line it would lag heavily when moving across parentheses. I'd assumed it was something to do with having a slow computer (it probably is), a slow network connection (it isn't) and one of the plugins I use such as [delimitMate](https://github.com/Raimondi/delimitMate) (it isn't). Turns out it's the built-in, and on by default, matching parentheses highlighting. 
