---
layout: page
type: text
title: Adventures in Lisp; Lastpass to PWman
categories: program
---
After tinkering with Haskell (still not finished there, this is all actually bizarrely linked), next on my list was Lisp. I found this [excellent Lisp tutorial](http://lisp.plasticki.com) and worked through that one day at work, but as with everything I needed a little stepping-stone task to really have a go at it. And working on Haskerdeux presented an ideal opportunity: As I've mentioned before (and the whole reason behind writing Haskerdeux) - due to the fact that our family shares an old Powerbook that is maxed out at 1.25GB RAM and the propensity for my wife and daughters to leave Safari open for weeks on end and use Flash (security/updates be damned, there is no way they will give up Safari or Flash, even given it's appalling performance on PPC) memory is scarce and swap files are large - at home I live as much as possible on the command line and use elinks for browsing as TenFourFox has huge memory requirements. But all my passwords are in [Lastpass](https://lastpass.com), which is browser based. Yet I need my Teuxdeux password for Haskerdeux. Unfortunately there is no command line based interface for Lastpass, but I did find [PWman](http://pwman.sourceforge.net) a console based password manager which is modelled on abook and since I'm using that, seems like a good idea.

PWman stores it's passwords in a "database" which is nothing more than a GPG encrypted XML file. Therefore I thought I'd use Lisp to write a little script to convert a Lastpass CSV file export to the XML format that pwman uses. And then I could just encode this with GPG and hey presto! I'd have imported all my Lastpass passwords in one fell swoop.

First port of call was figuring out libraries for Lisp. I can't remember where I heard about it now, but fortunately I'd come across [QuickLisp](http://www.quicklisp.org) which is just like gems for Ruby, cabal for Haskell, etc. It really is a doddle to use. I knew I should find some kind of [CSV parser](http://members.optusnet.com.au/apicard/csv-parser.lisp) rather than write my own (because I'd fail at that, not because [you/I shouldn't](http://www.secretgeek.net/csv_trouble.asp)), but initially I wasn't concerned with an XML library as I was generating quite simple markup - it was easy enough to do with strings, etc. However, I forgot about the need to XML escape things - especially passwords since they include odd characters. So I did find and use the [xmls](http://www.common-lisp.net/project/xmls/) library.

There's really not much to the code (this was a lovely first exercise in Lisp). Libraries are loaded as follows:

{% highlight cl %}
(load "~/.sbclrc")
(ql:quickload "csv-parser")
(ql:quickload "xmls")
{% endhighlight %}

To load quicklisp it just sources the `rc` file, where quicklisp will have written the bits and pieces it needs to load itself. I suppose I could include those lines directly. Next there is rudimentary handling of command line arguments to pick up the Lastpass export file and default to a file called `Lastpass.csv` in the current directory if no file is specified. 

{% highlight cl %}
(defparameter infile (if (null (second *posix-argv*)) "lastpass.csv" (second *posix-argv*)))
{% endhighlight %}

Interestingly the first argument is always `sbcl`. I don't get why you'd ever need that as an argument? This is the main bit that would need tweaking for other CLisps as `*posix-argv*` is specific to SBCL. Next comes the actualbody of the programme/script:

{% highlight cl %}
(with-open-file (stream "pwman.txt" :direction :output :if-exists :supersede)
	(format stream "<?xml version=\"1.0\"?><PWMan_PasswordList version=\"3\"><PwList name=\"Main\">")
	(csv-parser:map-csv-file infile 
		(lambda (ln)
			(format stream
				"<PwItem><name>~a</name><host>~a</host><user>~a</user><passwd>~a</passwd><launch></launch></PwItem>"
				(xmls:toxml (fifth ln))
				(xmls:toxml (first ln))
				(xmls:toxml (second ln))
				(if (null (third ln)) ;Secure Notes have no password 
					(xmls:toxml (fourth ln)) ;Use extra field if Secure Note
					(xmls:toxml (third ln))))) :skip-lines 1)
	(format stream "</PwList></PWMan_PasswordList>"))
{% endhighlight %}

All it does is, for each line of the csv file, output an XML string with the appropriate fields from the CSV file. Since secure notes have an entry in the 'notes' field as opposed to the 'password' field it pulls that info into pwman instead. To run the script via [sbcl](http://sbcl.org) (since that seems to be all the rage, although in *should* work in any CLisp with minimal tweaking):

{% highlight bash %}
sbcl --script /path/to/this/script </path/to/lastpass/export>
{% endhighlight %}

The path argument is optional as it'll look for a file called "lastpass.csv" in the same directory. It exports a file called `pwman.txt` which then needs encrypting with your gpg id:

{% highlight bash %}
gpg -r <yourgpgid@domain.com> -o ~/.pwman.db -e pwman.txt
{% endhighlight %}

Done! Well, except from remembering to delete the two plain text files, probably not a good idea to leave them lying around. I almost forgot: [lastpass2pwman on github](https://github.com/atomicules/lastpass2pwman).
