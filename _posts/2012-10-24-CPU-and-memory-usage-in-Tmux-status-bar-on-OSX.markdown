---
layout: page
type: text
title: CPU and Memory Usage in tmux Status Bar on OSX
categories: 
- program
---
(Bear in mind I'm stuck on 10.5.8 so this post is not very relevant for anybody).

At work, in Linux, I use XMonad and [xmobar](http://projects.haskell.org/xmobar/) and have it display the CPU, RAM and Swap activity like so:

	Cpu: XX% | Mem: XX% * Swap: XX% | Day Mon DD YYYY HH:MM:SS

This comes [very easily](http://simp.ly/publish/q1wkKc) thanks to the [System Monitor Plugins](http://projects.haskell.org/xmobar/#system-monitor-plugins). I wanted to do the same in tmux (since that's what I use when logged into my home machine via ssh), but there's no built in support for this kind of thing. I did find this [nifty little programme](https://github.com/thewtex/tmux-mem-cpu-load), but it's Linux only due to the absence of `/proc/stat` on OSX (at least on 10.5.8 anyway - and I'd hazard a guess it remains this way on later OSXs).

So the only option left is to use standard OSX command line tools and `grep` and `awk`:

	set -g status-right "#[fg=green]# #22T | #(top -l 2 -FR -n 0 | egrep 'CPU|PhysMem' | awk 'FNR==3{print $8+$10}FNR==4{print $8}' ORS=%%/ | sed 's/..$//')/#(du -hs /var/vm | awk '{print $1}') | %a %b %d %Y %H:%M"

Breaking this down:

	top -l 2 -FR -n 0 | egrep 'CPU|PhysMem' | awk 'FNR==3{print $8+$10}FNR==4{print $8}' ORS=%%/ | sed 's/..$//'

1. Calls `top` in logging mode (i.e. non-interactive). In an attempt to speed it up a bit I used the `-F` and `-R` options to disable calculating the statistics on shared libraries and reporting memory object maps for processes, respectively; I've no idea if this helps, but it can't do any harm. The `-n 0` sets the number of processes to zero since I just want the summary/header lines. I call `top` with `-l 2` samples as the man page states: "Note that the first sample displayed will have an invalid %CPU displayed for each process, as it is calculated using the delta between samples".
2. Searches (`egrep`) for lines containing "CPU" or "PhysMem"
3. Then uses `awk` to extract the relevant bits of information; sums and prints the eighth and tenth columns (the user and system CPU percentages) from the third row and then the eighth column from the fourth row (the physical memory usage). I'm just printing out the memory usage rather than calculating and displaying a percentage like xmobar. A hacky kludge then ensues to work around the fact that the tmux `status-right` string needs to be enclosed in quotes and therefore I can only use single quotes within it. Summing fields `$8` and `$10` unfortunately removes the handy "%" suffix from the CPU reading that is already in `top`. I can't simply add this back in within the `awk` print statement because I can't use double quotation marks (And I don't think any of the [quotation escape tricks](http://www.gnu.org/software/gawk/manual/gawk.html#Quoting) will help in tmux) so the output record separator (`ORS`) is set to "%%/" (the double percentage requirement is a tmux thing) so that the result is something like `64.22%/1129M%/`
4. Hacky kludge continued: `sed` is then used to delete the last two characters (`..$`)  since I don't want the "%/" on the end of the RAM usage. Note that using a match like `%\/$` doesn't work, presumably because of how tmux interprets it (although obviously that would work fine directly in bash).

To get the swap usage rather than use the value in `top` - which is a load of bollocks really and not "real" in that the swap space hasn't necessarily been created - I use:

	du -hs /var/vm | awk '{print $1}'
	
Which just sums up the swap files in `/var/vm` and prints the result. And this leaves me with a status bar that looks like:

	Powerbook.local | XX.X%/XXXXM/X.XG | Day Mon DD YYYY HH:MM

The default fifteen second refresh provided by tmux seems to be good enough for now. If anything, I might consider increasing it out to sixty seconds.

*Note: I'm not mega keen on using `top` as I'm fairly certain calling it causes a temporary CPU spike which means the values reported aren't mega accurate. But I don't know any other way to do it; using `iostat` gives the same "spike" and also only gives the CPU percentage, not physical memory at the same time. Also, as I suspected, the CPU and RAM are pretty much maxed out constantly due to Mail.app and the two instances of Safari that are always running - we really need a new computer or more old computers.*
