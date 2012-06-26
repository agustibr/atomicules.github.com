---
layout: page
type: link
title: In Vim, prevent caret moving back when I leave edit mode?
link: http://superuser.com/questions/230081/in-vim-prevent-caret-moving-back-when-i-leave-edit-mode
categories: 
- code
date: 2012-06-26 14:27:00:00 UTC+1
---
A quicky. This has bugged me for ages, but I never thought to go and look for a solution; is there a secret vim config setting, etc? Anyway after a lot of searching today, I eventually stumbled upon the linked question on SuperUser and it was *exactly* the same question I had. Unfortunately no great answers, but it did inspire me to come up with [my own answer](http://superuser.com/a/441719/76332). Although very simple it works fine for me and now means I can escape out of insert mode and have the behaviour I want (and expect):

{% highlight vim %}

inoremap ii <ESC>l

{% endhighlight %}

It just means that when I press `ii` in quick succession it is the same as pressing `<ESC>` followed by `l`, which is exactly what I do manually. One thing to be careful of is making sure there are no space characters on the end of the line of the above `inoremap` as otherwise they can get interpreted too. 

I went for `ii` as opposed to `jj` like most people as it makes the most sense to me: `i` for entering insert mode, so having `ii` to exit just seems more natural.
