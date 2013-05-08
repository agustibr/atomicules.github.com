---
layout: page
type: text
title: Switching from bash to mksh
categories:
- program
---
I stumbled across mention of [mksh](https://www.mirbsd.org/mksh.htm) on the [suckless mailing list](http://dir.gmane.org/gmane.comp.misc.suckless) and because I just *cannot* help myself with things like this, thought I'd see if I could switch to it from bash. This should be pretty easy for me as it's only fairly recently I'd bothered to get my `.bashrc` in a proper shape and [usuable across platforms](https://twitter.com/#!/atomicules/status/245987167468408832) so it's not like I'm a power-user of bash; which is also the reason why I never bothered jumping on the zsh bandwagon - the extra functionality would have been lost on me. So pretty much switching for me was never going to require much more than `chsh`.

There were only three things I wanted:

1. Vi editing mode. Which it comes with; although is orphaned (i.e. not actively developed). But still works fine for me.
2. History search up/down in Vi mode. Which is also built it, but took me about ten reads of the man page to spot it (It's `/` or `?` in command mode, type a bit of a command, `Enter` and then `n` or `N` to search back/forward through results).
3. That fancy Git PS1 thing

##Git PS1 in mksh

I tried sourcing [git_prompt.sh](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh), but it doesn't work in mksh. I then took a look at the code and thought "that's a lot of script when all I want is the current branch in the prompt" (and also "no way I'm able to convert that to mksh") so did a simpler approach:

	$(git branch 2>/dev/null; )

since error is redirected to `/dev/null` if it's not a git directory it displays nothing, if it is it displays the current branch. That's all I need. I don't know if the `git_prompt.sh` did more than that (I suspect so), but all I ever used it for was to show the current branch (I found it really helped me not do stupid things on the wrong branch). I added a couple of other bits in as well. I wanted to display the current working directory (`pwd`) because it's quite handy to know where the hell you are it you have a lot of shells open; I don't care however about my username or host:
	
	$(local WD=${PWD/$HOME/~}; if (( ${#WD} > 23 )); then print ${WD:0:10}...${WD: -10: -1}; else print $WD; fi)

I didn't want half my line taken up with the path though, so first the `$HOME` path is replaced with `~` (`WD=${PWD/$HOME/~}`) and then if the resultant path is still more than 23 characters long it is trimmed in the middle so I end up with something like: `/usr/pkgsr.../mksh/work` if I happen to be at `/usr/pkgsrc/shells/mksh/work`. I figured being able to see a bit of the start and a bit of the end of the path would be better than one or the other.

Last of all I nicked a bit from the [mksh man page](https://www.mirbsd.org/htman/i386/man1/mksh.htm) to display `$` or `#` depending on whether I'm me or root (assuming I get around to using the same shell and `.mkshrc` for root).

All added together I ended up with:

	PS1='$(local WD=${PWD/$HOME/~}; if (( ${#WD} > 23 )); then print ${WD:0:10}...${WD: -10: -1}; else print $WD; fi) $(git branch 2>/dev/null; )$(if (( USER_ID )); then print \$; else print \#; fi) '

_EDIT (2013-05-08): Fixed the Git branch bit, wasn't clearing after cd'ing to a non-git directory_
