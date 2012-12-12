---
layout: page
type: text
title: Adding a Readable Mode (of sorts) to ELinks
categories: 
- program
---
I've been meaning to have a play with Lua scripting in ELinks for quite some time. Lua doesn't look like a hard language, but I've found scripting ELinks a bit fiddly - you don't necessarily get much feedback from ELinks about why scripts haven't worked (sometimes it's as simple as changing the keyboard shortcut) and you can't do much testing in a separate interactive Lua session as ELink's built-in functions aren't available. So this is just the first successful thing I've managed to do. 

The first problem I had was that the Lua Console didn't work in ELinks and so I just assumed for quite some time that the whole Lua integration was bust using the Macports version (which requires Lua 5.0). So I went searching and found that ELinks is actually being [actively developed](http://repo.or.cz/w/elinks.git) and had been patched to build against Lua 5.1 so wasted a whole heap of time trying to get a more recent release to build. I finally figured I'd search the mailing list for info about the Lua Console and that's when I found out a [simple fix is required](http://bugzilla.elinks.cz/show_bug.cgi?id=812), just add the following to `~/.elinks/hooks.lua`:

{% highlight lua %}
-- work around bug 812: Lua console requires lua_console_hook
function lua_console_hook (expr)
	return "eval", expr
end
{% endhighlight %}

So with all that time wasted, because Lua integration was actually working all along, I could finally get on with writing something. Since I do actually use ELinks quite a bit, and especially for reading text heavy articles, I wanted some kind of "Readable" mode that would format all the text into a nice sized single column centered in the page - I tend to use ELinks via a ssh tmux session in full screen and the line lengths are a bit too long that way.

This function uses the built-in `current_document_formatted` function to format the current page to a 80 character width. It writes this out to a temporary file (I think the only way you can return a modified page directly is when using the `pre_format_html_hook` and for that you have to know the URL in advance) using some old-school table centering and opens the temp file up.

{% highlight lua %}
function squeeze ()
	--Use .html extension so can centre the column in the screen
	local tmp = tmpname()..".html"
    local doc = current_document_formatted (80)
    if doc then
		local file = io.open (tmp, "wb")
		--Quick and dirty centering using three table columns
		file:write ("<table><tr><td width=\"25%\"></td><td><pre>")
		file:write (doc)
		file:write ("</pre></td><td width=\"25%\"></td></tr></table>")
		file:close ()
		--Note: need some way of cleaning up these files. Look at hooks.lua contrib samples
	end
    return tmp
end

	bind_key ("main", "Alt-i",
		function () return "goto_url", squeeze () end)
{% endhighlight %}

I borrowed heavily from the examples in the [hooks.lua contrib file](http://repo.or.cz/w/elinks.git/blob_plain/HEAD:/contrib/lua/hooks.lua.in) - well worth checking out if you want to script ELinks with Lua (which in all likelihood, you don't).

Right, now I've got the basics sussed, I want to try and do something else.
