---
layout: page
type: text
title: A Voyage of .netrc Discovery (and parsing in Haskell and Vim Script)
categories: 
- program
---
Even though I've been using \*nix based OSs for about twelve years I only recently came across [.netrc](http://linux.die.net/man/5/netrc) when reading through the man pages for OfflineIMAP and msmtp - I wanted to start synchronising and backing up my `.offlineimaprc` and `.msmtprc` files via [snose](https://github.com/atomicules/snose), but didn't really want to do that with my passwords in the file, so was reading up on how I could reference a password from some external source.

So I thought it would be great if snose could also do this, as I liked the idea of consolidating passwords into `.netrc` if I was going to have to store them in a file; up until now, snose required it's own `.snoseauth` file with username and password in json format. Adding this was easy, thanks to the [netrc file processing](http://docs.python.org/library/netrc.html) standard library:

{% highlight python %}
if not options.username or not options.password:
	#Check to see if stored somewhere
	try:
		options.username = netrc.netrc().authenticators("simple-note.appspot.com")[0]
		options.password = netrc.netrc().authenticators("simple-note.appspot.com")[2]
	except IOError as e:
		print 'Username and password must be supplied or exist .netrc file under domain of simple-note.appspot.com'
		exit()
{% endhighlight %}

And I also  thought it would be great if [Haskerdeux](https://github.com/atomicules/HaskerDeux) could use `.netrc`. Before this, username and password had to be supplied as command line arguments. This implementation "works well enough for me". As more of a note to myself on the code then anything else: it reads the lines of the `.netrc` into a list; drops items off from the front of the list until it gets to one containing the domain/machine of interest; then checks if that list item also contains "login", if it does the format of the `.netrc` entry is all on one line so it uses the `getcred` function to split up that line into words (`(words $ head netrc')`) and removes items from that list until it finds "login", etc, at which point it gets the succeeding list item; otherwise it assumes the next two lines (`netrc' !! 1`, etc) contain the login and password and simply splits up these lines into lists of words and gets the last item from each list.

{% highlight haskell %}
readnetrc = do
	home <- getHomeDirectory
	netrc <- fmap lines $ readFile (home ++ "/.netrc")
	let netrc' = dropWhile (not . isInfixOf "teuxdeux") netrc
	let (username, password) = if "login" `isInfixOf` head netrc'
		-- if entry is on one line	
		then (getcred "login", getcred "password") 
		-- if entry is on multiple lines
		else (last $ words $ netrc' !! 1, last $ words $ netrc' !! 2)
		where getcred c = dropWhile (not . isInfixOf c) (words $ head netrc') !! 1
	return (username, password)
{% endhighlight %}

Whilst I was at it, I thought I should try to implement similarly for Vim as well so I could do things like this in my `.vimrc` (this example for [Simplenote.vim](https://github.com/mrtazz/simplenote.vim):

{% highlight vim %}
let g:SimplenoteUsername = GetCredFromNetrc("simple-note.appspot.com")[0]
let g:SimplenotePassword = GetCredFromNetrc("simple-note.appspot.com")[1]
{% endhighlight %}

As until now, I either had to include my password in my `.vimrc` or source it from yet another separate password file. This uses exactly the same approach as the Haskell one, except it isn't possible to "drop" items off a list so it just keeps track of the index as it is going through the lines of the `.netrc`. 

{% highlight vim %}
function! GetCredFromNetrc(machine)
	let filename = $HOME.'/.netrc'
	let login = ""
	let password = ""
	if filereadable(filename)
		let lines = readfile(filename)
		let i = match(lines, "machine ".a:machine)
			if lines[i] =~ "login"
				"get username and password"
				let login = split(split(line[i], "login")[-1], " ")[0]
				let password = split(split(lines[i], "password")[-1], " ")[0]
			else 
				"Assume on next two lines. It will be for me
				let login = split(split(lines[i+1], "login")[-1], " ")[0]
				let password = split(split(lines[i+2], "password")[-1], " ")[0]
			endif
	endif
	return [login, password]
endfunction
{% endhighlight %}
