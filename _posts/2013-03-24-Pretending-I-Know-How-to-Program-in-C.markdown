---
layout: page
type: link
title: Pretending I Know How to Program in C
link: https://github.com/atomicules/pwman/commit/bbe299d048f3ba16de092fb6f83ac04f7071463b
categories: 
- program
---
It might look like a simple change, but this has taken me ages to figure out (well, on and off, it's not like I've solely been sat puzzling this for months). On thing that has bugged me about PWman is how easy it is to erase bits of data, just by accidentally editing a field. 

When you pull up an entry to view the password this also immediately places you in an edit mode. By pressing a number equal to one of the fields (I seem to do this accidentally quite often) you edit that field. Due to the way ncurses works (as far as I can tell there is no way to avoid this behaviour in ncurses) you aren't given the existing string to edit, rather you starting with a blank entry to replace the existing string. There is no way to escape out of this editing once you've gone into it so the only way of "cancelling" is copying and pasting the old value back in.

This small change means that if I just press the return key (and type nothing else) nothing is changed.

Things I didn't/don't understand:

- With the above linked commit I thought I'd understood what I'd done, but was confused as to why I hadn't had to `malloc` the `oldinput`, until I twigged that I had been a fool - I wasn't doing dynamic allocation by doing [strlen(input)](https://github.com/atomicules/pwman/commit/bbe299d048f3ba16de092fb6f83ac04f7071463b#L0R465), all that I was doing was taking whatever length `input` is initialised to which must be one of `STRING_SHORT`, `STRING_MEDIUM`, etc, although I'm not entirely clear which or where this happens. And I assume this also means that if it isn't initialised as `STRING_LONG` it would truncate a `STRING_LONG` field.
-  The other thing that threw me for a bit was [mvwgetnstr(bottom, 1, x, input, len)](https://github.com/atomicules/pwman/blob/bbe299d048f3ba16de092fb6f83ac04f7071463b/src/ui.c#L479): I wasn't looking closely enough and didn't realise that `input` here is the output that is being modified. I should have realised as there's `no xxxx = mvwgetnstr`. [The docs cleared this up](http://www.delorie.com/gnu/docs/ncurses/man/curs_getstr.3x.html) for me.

Whilst writing this post I [fixed the dynamic allocation](https://github.com/atomicules/pwman/commit/98593a8587ac129a32f1f978e119123897cec987#src/ui.c) and also extended this technique to cover the password field as I hadn't done that initially and it's arguably the one where I really want this functionality. Although I'm not entirely convinced dynamic allocation is good approach - it might make more sense just to set `oldinput` to `STRING_LONG` and be done with it. 

Although now I'm actually wondering whether a better approach would be to check for the escape key being pressed and cancel out of editing the field that way? I'm not exactly sure how that would be done, but since [CTRL+g](https://github.com/atomicules/pwman/blob/98593a8587ac129a32f1f978e119123897cec987/src/actions.c#L272) ([0x07](https://en.wikipedia.org/wiki/Talk:Control_key)) is used to [trigger password generation](https://github.com/atomicules/pwman/blob/98593a8587ac129a32f1f978e119123897cec987/src/ui.c#L578) it must be possible. That's the next job then.
