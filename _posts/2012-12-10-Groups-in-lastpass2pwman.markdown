---
layout: page
type: link
title: Groups in lastpass2pwman
link: https://github.com/atomicules/lastpass2pwman/commit/029e7e57b7bfbe81b8678cb7b37d044326a0102f
categories:
- program
---
It was only with my recent tinkering with the PWman source code that I realised PWman supports sublists (or groups) so I've tweaked my Lisp lastpass2pwman script to use sublists as per the group settings in the Lastpass export.

This was easy-ish to do. Unfortunately the Lastpass CSV export is not ordered by group, and since the PWman database is an XML file with sublists defined as child nodes it was no longer possible to write the XML file as I read the CSV file. Lisp has hash-tables so I knew what I wanted to do was to read the CSV file and create a key for each group, collating all the entries under the appropriate key. So I started with this:

{% highlight lisp %}
(csv-parser:map-csv-file infile
	(lambda (ln)
		(if (gethash (sixth ln) *lastpass*) ;Group
			(setf
				(gethash (sixth ln) *lastpass*)
				(append (gethash (sixth ln) *lastpass*) (list ln))) ;append key
			(setf
				(gethash (sixth ln) *lastpass*)
				(list ln)))) :skip-lines 1) ;create key
{% endhighlight %}

`(sixth ln)` is the sixth entry on the current line of the CSV file, which indicates the grouping in Lastpass. This code actually runs and appears to work, but something bizarre was going on with the keys in the hash table, because when I then did:

{% highlight lisp %}
(loop for key being the hash-keys of *lastpass*) do (print key))
{% endhighlight %}

It listed multiple keys with the same name. I knew something was up as the keys were all quoted strings and thus could only be queried by doing something like this:

{% highligh lisp %}
(defparameter gkey "Secure Notes")
(gethash gkey *lastpass*)
{% endhighlight %}

and even then, that only returned `NIL` as if the key didn't exists. Even more oddly though, if you then did:

{% highlight lisp %}
(setf (gethash gkey *lastpass*) (list 1 2 3))
{% endhighlight %}

It would then set the key properly and allow you to modify and retrieve the value. From tutorials I knew the [normal form for keys was not quoted strings](https://en.wikibooks.org/wiki/Common_Lisp/Advanced_topics/Hash_tables#Traversing_a_Hash_Table), but I couldn't figure out how to convert the strings from the CSV file to the correct form until I found this Stackoverflow question: [common lisp symbol matching](http://stackoverflow.com/questions/12420240/common-lisp-symbol-matching). The secret was `read-from-string`:

{% highlight lisp %}
(csv-parser:map-csv-file infile
	(lambda (ln)
		(if (gethash (read-from-string (substitute #\- #\Space (sixth ln))) *lastpass*) ;Group
			(setf
				(gethash (read-from-string (substitute #\- #\Space (sixth ln))) *lastpass*)
				(append (gethash (read-from-string (substitute #\- #\Space (sixth ln))) *lastpass*) (list ln))) ;append key
			(setf
				(gethash (read-from-string (substitute #\- #\Space (sixth ln))) *lastpass*)
				(list ln)))) :skip-lines 1) ;create key
{% endhighlight %}

The `(substitute #\- #\Space ` just replaces spaces in any group names (such as "Secure Notes") with hyphens. Without this `read-from-string` will remove anything from the first space character encountered in the group name, so not vital, but makes it a bit nicer.

Writing the hash table out to XML was very similar to when I was writing the XML file as the CSV file was read, with the addition of another loop (looping through each key/group and then looping withing all the entries of that group). I'm pretty sure I could be converting to XML in a more clean, lispy way, especially after reading [The Nature of Lisp](http://www.defmacro.org/ramblings/lisp.html), [but...](https://rstat.us/updates/50b38cd71e393d000200b679i)
