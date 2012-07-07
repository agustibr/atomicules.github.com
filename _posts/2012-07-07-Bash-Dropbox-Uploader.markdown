---
layout: page
type: link
title: Bash Dropbox Uploader
link: http://www.andreafabrizi.it/?dropbox_uploader
categories: 
- code
---
This is mega handy! It only needs Bash and cURL. 

I'm still trying to de-googlify my life as much as possible and decided I rarely use Google Docs, but wanted to backup what I had on there to Dropbox, so after exporting everything I wanted a way to upload the documents _and file structure_. I.e. I didn't just have one folder full of files, rather a load of subfolders and files and I wanted to maintain that structure on Dropbox. I really didn't want to set-up Dropbox properly and synchronise a load of files I didn't need on this computer. I just wanted a one time upload.

So thanks to the above tool, a simple bash oneliner uploaded everything as desired:

{% highlight bash %}
find . -type f -exec /path/to/dropbox_uploader.sh upload {} "Documents/"{} \;
{% endhighlight %}

This gets a recursive list of all the files in the current directory and below and for each of them executes the `dropbox_uploader` script. The `{}` characters get replaced with the result from the find command, i.e. the path to the file from the current directory. The `"Documents/{}"` bit is the target path on Dropbox, i.e. the same relative path as I have locally, but in the Documents folder I have on Dropbox.

Bash is ace when you know what you are doing. Shame I don't most of the time!
