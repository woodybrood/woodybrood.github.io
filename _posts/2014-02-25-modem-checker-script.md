---
layout: post
title: Modem Checker Script
category: articles
tags:
- ruby
- tinkering
---

A little while back I described how my internet connection problems led me to
figuring out how to track when my cable modem rebooted. The post was pretty
light on read details. This post exists to flesh out some of the details. The
code you will see here is not the flashiest, but it works to do the basic task
of recording the modem logs in a database and alerting me when a reboot
happens.

## First Objective: Parse the Log Page

Motorola SB6141 cable modems include a web server that shares diagnostic
pages. The list includes a page for for checking overall modem status. Another
page shows the signal strength. And yet another lists the open source software
that goes into the device. And then, you will find the log page.

The log page is an interesting thing. Tech support at Time Warner wanted
nothing to do with it, and neither did Motorola. Apparently, the people that
would use the page don’t talk to customers. Below is the log page from my
modem:

![Modem Log Page](https://dl.dropboxusercontent.com/u/127679/whotestedthis/Mod
emLogPage.png) Modem Log Page


Each row contains the time, severity, code and diagnostic message for events
that the modem detects. When I looked at this page with on my modem, it was
filling up every day with a variety of diagnostic events, including the reboot
events.

All-in-all, it is a simple HTML table. Using open-uri and
[nokogiri](http://nokogiri.org) I wrote some code that could fetch the page
and extract the the rows from the table.



``` ruby
doc = Nokogiri::HTML(open(url))
doc.xpath(&quot;//table/tbody/tr[not(th)]&quot;).reverse_each do |row|
```

I know XPath is not the always nicest way to do things, but with the lack of
styling in the table and the simplicity of the page, it seemed a reliable
choice.

## Logging the data

My script logs each row from the table to a SLQite3 database. Note that I
iterate over the rows in reverse order, as I want to pull them in from the
bottom to the top. This lets me log the entries to the database in order. I
can compare the time on each row to the last logged time in the database, to
make sure I don’t double log.

I assume the modem gets the time from the internet, as every time it reboots,
you see the date set to the start of the unix epoch (1970), then the modem
updates to the current date and time. The modem fixes it’s internal clock
before it is officially running again, so you can trust the first row with a
current date.



``` ruby
if newer?(get_highest_timestamp(db), timestamp)
   db.execute(&quot;insert into log (timestamp, level, code, message) values(?,?,?,?); &quot;, timestamp, level, code, message )
    # push happens here
else
   # do nothing
end
```


Each every entry, except those stamped 1970, gets logged to the database.
That information supplies the data in the [Sinatra](http://sinatrarb.com)
application I wrote before I added push notifications.[1]

## The Push

When I first implemented the logger, I could check the Sinatra app to see if
the network had a problem when I wasn’t around to see it. That was cool and is
still useful for analyzing trends, but we live in the iPhone age. Push
notifications provide near instant updates wherever you are.

I found [Pushover](https://pushover.net) after hearing it mentioned on the
[Systematic][systematic] podcast. It’s push notification service with an API
you can use in your own apps or scripts.

Fortunately, it is not necessary to key off the date change; a specific code
for a reboot event and the script just looks for that. The followng code
triggers a push notification when the script detects a “Z00.0” code (the
reboot code) while saving the log page table contents to the SQLite3 database.



``` ruby
if code == 'Z00.0'
       pusher.push_notification(&quot;The modem rebooted at #{timestamp}.&quot;, &quot;Modem reboot alert&quot;)
end
```

## The Pusher

So, what is the pusher you see there? I’m using the
[Rusover](https://github.com/bemurphy/rushover) gem to communicate with
[Pushover.net](https://pushover.net). Rushover makes the process of using
Pushover a little easier. I’m sure I could have written my own code to call
the api directly, but rushover does make it just a little simpler.

However, while rushover does make it easier, I prefer to wrap rushover in my
own code. This allows me to make changes later (for example if it changes in a
way I don’t like, or if I decide to change push provider). I created
PushNotifier to ecapsulate the interactions with Rusover. The following code
demonstrates how I send the push notification:

``` ruby
def push_notification_impl(message, title, current_time)
    unless is_during_quiet_period?(current_time)
        resp = @rushover_client.notify(@user_key, message, :priority =&gt; @priority, :title =&gt; title, :sound =&gt;@sound)
        resp.ok? # =&gt; true =
    end
end
```

You get the user_key from Pushover.net. You also need to create an application
key for each application or script that you want to send distinct
notifications from. I’ve hidden the keys to my applications in the following
example of the pushover configuration page:

![Pushover Home Page](https://dl.dropboxusercontent.com/u/127679/whotestedthis
/Pushover_Home.png) Pushover Home Page

## Running the Script

I’ve got a spare Mac Mini set up in the basement to run the script. I bought
the Mini off a friend a number of years ago. It’s not too fast, but it just
keeps ticking; making it the perfect box for the job.

A little bit of research and experimentation enabled me to set up a launchd
job to run the script every two minutes. Initially, I wrote it with an
infinite loop, but when I thought about it, I realized there were far more
things that could go wrong with that. Launchd ensures that the script will run
when the computer reboots, something my original version wouldn’t do, as I was
running it in a terminal.

## And What Did I Learn?

I learned a lot.

First off, I learned how to do push notifications; a useful trick to have. It
enabled me to create a second push notification for work, where I’ve been
working the incoming bug queue. It isn’t the biggest pipe, so issues come in a
couple times a day. Now I get an alert whenever the modem reboots itself.

Second, I learned how to create a LaunchDaemon on OS X. While not something I
need to do a lot, it is one more skill that I didn’t have a month ago.

Finally, I learned that sometimes the journey is more important than the
destination. Soon after I got the notifications working, I switched out the
modem with a replacement from Motorola. Since that time, the modem has not had
a random self-reboot. Besides, sometimes we don’t have the time or the
bandwidth in our day jobs to explore things that interest us. And sometimes,
the things we learn can help us later on the job.

I made the entire script available in a
[gist](https://gist.github.com/woodybrood/9044212). Feel free to make it your
own.



* * *

  1. In a future post, I will talk more about my love of Sinatra.


