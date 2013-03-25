---
layout: page
type: text
title: How much memory is my Tmux session using?
categories: 
- program
---
	ps -o rss  | awk '{mem += $1} END {print mem/1024 "M"}'

That much. 

(Well, assuming you only run one Tmux session and are on OSX 10.5.8, otherwise: something like that).

I'm using a variation on that theme in my [Tmux status-bar]({% post_url 2012-10-24-CPU-and-memory-usage-in-Tmux-status-bar-on-OSX %}) so that I know how much memory my session is using as well as the overall stats for the machine. This will be quite interesting for me; I'm [window shopping](http://prgmr.com) and wondering what I could get away with (As I write this I'm using about 100MB). 
