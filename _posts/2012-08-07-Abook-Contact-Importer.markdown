---
layout: page
type: link
title: Abook Contact Importer
link: https://github.com/atomicules/Abook-Contact-Importer
categories: 
- program
---
After much umming and erring I decided to use [abook](http://abook.sourceforge.net/) as a command line tool to manage contacts. Even though it has long been out of active development I figure not many advances are really required in an addressbook program so it should work just fine, plus it works well with Mutt. This is another step away from Google for me and - in fact - it means the only Google product I'm relying on is just the email part of GMail. So hopefully one day I'll get around to switching that away as well. 

Anyway, I digress, I thought about writing my own Google contact to abook converter (I couldn't get the [vCard import patch](http://abook.sourceforge.net/patches/abook_vcard_import.patch) to build so am sticking with the Macports version of abook which is at 0.5.6) as a good exercise to learn clisp, but then I stumbled across this [python Google contacts to abook script](https://code.google.com/p/gmail-abook-contact-converter/) (since [found on Github](https://github.com/gavcos/Abook-Contact-Importer), and so forked). I decided to use that with a few tweaks. In an entire week off from work the only code I managed to find time to complete was my modifications to that script, so although a learning exercise for clisp would have been nice, realistically it could well have taken until the end of the year to finish; basically they only way I get any code written is by skiving off at work (I'll get caught eventually) or staying up late at home after everyone else is bed and sacrificing sleep. 

So there they are, [my pissy little changes](https://github.com/atomicules/Abook-Contact-Importer/commits/master/adr_conv.py) (I just changed the Python 2.7 script):

- Imports all email addresses for each contact, rather than just the last one (well, upto the five allowed by abook).
- Imports physical /building addresses. By the looks of it the author amended the Python 3 version on Github to do this, but I only found the Github version after doing my changes on the Google Code version, also I was only concerned about the Python 2.X script. This was a big missing feature for me - it is an addressbook afterall. Since abook just has one address field, it puts other addresses in custom fields 2 to 5.
- Puts notes in the note field.
- Puts nickname in the nickname field. Originally the script confused these fields and put both (or the last one parsed for each contact) in the nickname field.
- Puts the organisation in the custom 1 field and where contacts don't have names (businesses, etc) also uses this data for the name field.
