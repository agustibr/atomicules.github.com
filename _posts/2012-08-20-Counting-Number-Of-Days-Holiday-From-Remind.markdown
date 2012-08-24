---
layout: page
type: text
title: Counting Number of Days Holiday from Remind 
categories: program
---
Like a lot of folk I need to keep track of how many days holiday I've booked off from work over the year. When I used Google Calendar I just searched for "holiday" and manually counted up the occurrences. I had hoped moving to [Remind](http://www.roaringpenguin.com/products/remind) would give me a more nifty way of doing this, such as displaying a running count of days holiday booked next to the holiday event similar to how you can do clever things like display someone's age next to their birthday. But after a lot of searching of the mailing list archives and reading of the manual I couldn't figure out anyway of doing this. However, just using the simple output from Remind and `grep` on the command line it is easy to count of the number of days I've booked off so far this year as follows:

{% highlight bash %}
remind -s12 .reminders-work jan 2012 | grep Holiday | grep Public -vc
{% endhighlight %}

Piping two greps together seems to be the cleanest way to do this to avoid a messy regex. The first bit of the command just outputs all of my work calendar for the year in simple format. This is then piped into `grep` to filter for just holidays and then piped into another grep that filters out Public holidays and returns a count.
