---
layout: page
type: text
title: SMILE QIF Generator in Vim Script
categories: 
- program
---
Since as of late I've been playing about with [web browsers that don't support bookmarklets](http://mason-larobina.github.com/luakit/) (at least as far as I can tell; there's quickmarks - I don't know what they are about) I revisited my [Smile QIF generator]({{ site.baseurl }2011/06/30/Smile-Bank-QIF-Generator-Bookmarklet.html}) and changed it into a Vim Script so I could use it in the same vein as [Dave Small's original Qifinator](http://www.web-development.co.uk/smile), i.e. copy and paste in from the Smile web page and "click" to process into Qif format:

{% highlight vim }}

"Uses tlib http://www.vim.org/scripts/script.php?script_id=1863
function! SmileQIF()
	"Delete any blank lines
	silent! g/^$/d
	"If previous statement (not recent items page) reverse the lines
	if getline(1)=~"BROUGHT FORWARD"
		g/^/m0
	endif
	let linenum = 1
	while linenum <= line("$")
		let curr_line = getline(linenum)
		if curr_line=~"BROUGHT FORWARD" || curr_line=~"LAST STATEMENT"
			execute linenum 'delete _'
			let linenum = linenum - 1
		else
			let parts = split(curr_line, "\t")
			"Date
			let new_line = "D".parts[0]."\t"
			"Transaction
			if tlib#string#Strip(parts[2]) == ""
				let new_line = new_line."T-".strpart(tlib#string#Strip(parts[3]),2)."\t"
			else
				let new_line = new_line."T".strpart(tlib#string#Strip(parts[2]),2)."\t"
			endif
			"Payment
			let new_line = new_line."P".parts[1]."\t\^"
			call setline(linenum, new_line)
		endif
		let linenum = linenum + 1
	endwhile
	"Insert Header
	let header = "!Type:Bank\t".getline(1)
	call setline(1, header)
	"Replace tabs with new lines
	%s/\t/\r/g
endfunction
command! SmileQIF :call SmileQIF()

{% endhighlight %}

This is actually better than my bookmarklet (although more verbose) as it also reverses the date order of previous statement pages to match recent items - since patching together statements across pages is often necessary because for some unfathomable reason Smile don't generate statements on clean date break boundaries (i.e. you'll find transactions from the same day in both recent items and previous statements).

I've just placed the above in my `.vimrc` and I call with `:SmileQIF`
