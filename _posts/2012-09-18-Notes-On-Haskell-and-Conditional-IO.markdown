---
layout: page
type: text
title: Notes on Haskell and Conditional IO
categories: 
- program
---
From the department of "It's obvious when you know how". I'd finally got around to working on [this issue on Haskerdeux](https://github.com/atomicules/HaskerDeux/issues/1) and I'd consolidate the duplicate Curl post requests into one function and had this working code:

{% highlight haskell %}
curlpost [todays_date, curlpostdata, apiurl, okresponse, username, password] number = withCurlDo $ do
	tdsf <- curlget [todays_date, username, password]
	let itemid = Main.id $ tdsf!!(read (fromJust number)::Int)
	let curlpostfields = if isJust number
		then CurlPostFields ["todo_item["++(show itemid)++"?]"++curlpostdata]
		else CurlPostFields ["todo_item[todo]="++curlpostdata, "todo_item[do_on]="++todays_date]
{% end highlight %} 

But I only wanted to do the `tdsf` and `let itemid` bits if needed. When I had my three separate (practically duplicate) Curl post requests I could just omit this bit for the function in question, but now I needed some way to handle it conditionally. I had hoped I could do something like this:

{% highlight haskell %}
curlpost [todays_date, curlpostdata, apiurl, okresponse, username, password] number = withCurlDo $ do
	let curlpostfields = if isJust number
		then CurlPostFields ["todo_item["++(show itemid)++"?]"++curlpostdata]
		else CurlPostFields ["todo_item[todo]="++curlpostdata, "todo_item[do_on]="++todays_date]
		where itemid <- do
			tdsf <- curlget [todays_date, username, password]
			return itemid = Main.id $ tdsf!!(read (fromJust number)::Int)
{% end highlight %}

But that doesn't work and just gives a ``parse error on input \`<-'`` for the `where itemid` line. I probably also tried things like:
		
{% highlight haskell %}
curlpost [todays_date, curlpostdata, apiurl, okresponse, username, password] number = withCurlDo $ do
	let curlpostfields = if isJust number
		then do 
			tdsf <- curlget [todays_date, username, password]
			itemid = Main.id $ tdsf!!(read (fromJust number)::Int)
			return $ CurlPostFields ["todo_item["++(show itemid)++"?]"++curlpostdata]
		else return $ CurlPostFields ["todo_item[todo]="++curlpostdata, "todo_item[do_on]="++todays_date]
{% endhighlight %}

But that gets you:

	Couldn't match expected type `CurlOption'
		   against inferred type `IO CurlOption'
	In the expression: curlpostfields

Which is obvious really as `return` is an IO type. But I was desperate and clutching at straws. However, thanks to [this Stackoverflow question](http://stackoverflow.com/questions/9216940/get-input-and-pass-variable-from-an-if-statement-with-haskell) I found out you can do this:

{% highlight haskell %}
curlpost [todays_date, curlpostdata, apiurl, okresponse, username, password] number = withCurlDo $ do
	curlpostfields <- if isJust number
		then do
			tdsf <- curlget [todays_date, username, password]
			let itemid = Main.id $ tdsf!!(read (fromJust number)::Int)
			return $ CurlPostFields ["todo_item["++(show itemid)++"?]"++curlpostdata]
		else return $ CurlPostFields ["todo_item[todo]="++curlpostdata, "todo_item[do_on]="++todays_date]
{% endhighlight %}

Problem solved. I had no idea you could bind if statements in that way. If only if also worked for the `where` statement option I tried as that is probably a bit cleaner and is along the same lines of binding the `if` statement.
