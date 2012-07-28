---
layout: page
type: text
title: OS and Network Conditionals in Vim Configuration
categories: 
- programming
---
A bit of a follow-up to [this vim post]({{ site.baseurl }}2009/05/30/vim.html) since - as happens to all Vim uses - my .vimrc file has grown a bit in complexity. It's still nowhere as near insane as some people's, but has changed enough to make that post redundant. Oh, and so has the number of plugins I'm using.

I am still very much cross platform (as much Linux as possible, OSX when I get the chance and Windows when I have to; too frequently) so finally moved to using [git submodules and pathogen to synchronise](http://vimcasts.org/episodes/synchronizing-plugins-with-git-submodules-and-pathogen/) across platforms. I'm not making my dotvim repository public though, as - well - who the hell actually wants to read through someone elses .vimrc?

What is worth mentioning though is how you can use conditional statements in the .vimrc to really have just one file you can use anywhere.

## OS Specific Configuration

This is a little more obvious and I'm sure lots of people do this. I'm using this [approach by Mikolaj Machowski](http://objectmix.com/editors/149466-operating-system-checking-vimrc-files.html#post517594) to set OS specific items, mainly font sizes, but also different settings for TwitVim. Note: OSX is "Darwin" and `uname` doesn't work on Windows so have this as my final `else` statement. 


## Network Specific Configuration

Not exactly tricky, but amazingly useful, is extending the above to make settings based on the network you are on. I.e. for me this is whether I'm at work or at home. So in the OS specific sections I'm looking to find the IP address (it's OS specifc since a different command is needed on each OS):

{% highlight vim %}
"OS Specific
"-----------
let os = substitute(system('uname'), "\n", "" ,"") "thanks to http://objectmix.com/editors/149466-operating-system-checking-vimrc-files.html#post517594
if os == "Linux"
	...
	let ip = match(system('ip addr show'), "10.0.1")
elseif os == "Darwin"
	...
	let ip = match(system('ipconfig getifaddr en1'), "10.0.1") "Or whichever enX interface you need
else "Assume windows as uname doesn't work on Windows	
	...
	let ip = match(system('ipconfig'), "10.0.1")
endif
{% endhighlight %}
[Link to gist](https://gist.github.com/1190099)

(I know at home my IP address will be in the range "10.0.1.X")

Then I can set proxies for [TwitVim](http://www.vim.org/scripts/script.php?script_id=2204) and [SimpleNote.vim](http://www.vim.org/scripts/script.php?script_id=3582) just for when I'm at work:

{% highlight vim %}
"IP Address specific
"-------------------
"(i.e. Home or Work)
if ip == -1
	"Set proxies, Python urllib2 (TwitVim, Simplenote.vim) will automatically pick
"these up	
	let $http_proxy = 'http://<username>:<password>@<proxy-url>:<proxy-port>'
	let $https_proxy = 'http://<username>:<password>@<proxy-url>:<proxy-port>'
endif
{% endhighlight %}

And that's a [whole interesting thing in itself...](https://github.com/mrtazz/simplenote.vim/issues/13)
