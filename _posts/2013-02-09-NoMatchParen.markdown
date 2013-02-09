---
layout: page
type: quote
title: 
quote: "let loaded_matchparen=1"
categories: 
- program
---
A little Vim performance tip when editing in a remote session. It turns off highlighting of matching parentheses (I'm going to stick to off by default and turn on with `:DoMatchParen`). I'd noticed that when moving the cursor along a line it would lag heavily when moving across parentheses. I'd assumed it was something to do with having a slow computer (it probably is), a slow network connection (it isn't) and one of the plugins I use such as [delimitMate](https://github.com/Raimondi/delimitMate) (it isn't). Turns out it's the , ssh tmux, disable match parantheses. Much better then.
let g:loaded_matchparen=1
