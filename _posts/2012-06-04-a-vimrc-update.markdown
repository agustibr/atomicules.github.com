---
layout: page
type: text
title: A .vimrc Update
categories: 
- code
date: 2012-06-04 09:00:00:00 UTC+1
---
I thought rather than update a [three year old post]({{ site.baseurl }}2009/05/30/vim.html) I'd just do a quick new one, a "Brief history of my .vimrc" so to speak.

1. I was using [Pathogen](http://www.vim.org/scripts/script.php?script_id=2332) and github for synchronising my dotvim files. And even though I was aware there were perhaps better solutions it worked well enough that I couldn't be bothered to change - [until it didn't](http://stackoverflow.com/q/1777854/208793)...
2. So I'm now using [Vundle](https://github.com/gmarik/vundle) which is much better since everything is contained within one file and it is easier than using submodules for ensuring plugins are up to date and also for initialising on new machines, etc. Since I do not have a `.gitmodules` file any more, nor a bundles directory to keep in sync, using an entire github repository for one `.vimrc` file seemed like tremendous overkill...
3. So I'm now using [Simplenote](http://simplenoteapp.com/) (and [snose]({{ site.baseurl }}2012/03/09/SNose.html)) for synchronising my `.vimrc` (along with a bunch of other dotfiles). I do not really care for history (even though Simplenote does provide this somewhat), rather that I can just synchronise files. 
4. Which leads me to: my current [.vimrc](http://simp.ly/publish/PL9RjF).

