---
layout: page
type: text
title: Merging Cells of a Column for a Row Subgroup in R Data.Table
categories: 
- program
---
A very long winded title for what I thought should be short and easy to do and actually was - but only after a couple of hours of early morning brain warm-up though.

Say you have some data like so:

{% highlight R %}
somedata <- data.table(read.table(header=TRUE, text="
	 id     coltomerge
	a1112      red
	a1112      green
	a1112      blue
	a1113      red
	a1113      red
	a1114      green
	a1115      yellow
	a1115      pink
	a1115      pink
"
))
{% endhighlight %}

and you want to group together all the "coltomerge" entries for each id into one cell. You can use `paste` to concatenate the data:

{% highlight R %}
somedata[,list("mergedcol"=paste(coltomerge, collapse=", ")), by=id]
{% endhighlight %}

I did say it was short and easy (I'm writing this mostly as a note to myself. I'm not under any illusion I'm sharing some complicated technique, but on the off chance someone else is having a slow day like I was...). Which gets you:

{% highlight R %}
        id          mergedcol
[1,] a1112   red, green, blue
[2,] a1113           red, red
[3,] a1114              green
[4,] a1115 yellow, pink, pink
{% endhighlight %}

If you don't want duplicates (I didn't) it's easiest to remove them first:

{% highlight R %}
#For this example, setkey again otherwise unique will only consider the id column
setkey(somedata, id, coltomerge) 
somedata <- unique(somedata)
somedata[,list("mergedcol"=paste(coltomerge, collapse=", ")), by=id]
{% endhighlight %}

Ta da!

{% highlight R %}
        id        mergedcol
[1,] a1112 blue, green, red
[2,] a1113              red
[3,] a1114            green
[4,] a1115     pink, yellow
{% endhighlight %}

