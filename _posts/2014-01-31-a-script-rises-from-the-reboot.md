---
layout: post
title: A Script Rises From The Reboot
category: articles
tags:
- ruby
- tinkering
---
I love to hack around at things. And while I’m not one to do too much hardware
hacking, I will hack around a little bit of Ruby here and there.

I’ve been working from home for over a year now. It’s a great experience for
me. I don’t mind being by myself during the day, and I have plenty of
camaraderie with my coworkers through chat and online meetings.

But working from home requires a stable Internet connection. Stable is key.
Nothing is worse than having your Internet connection drop out just before you
give your report in the virtual stand-up. My connection was dropping out
regularly, and it was looking like it was going to be a big problem.

So I did the first thing you can do, called the cable company. Three times
they sent folks out. They adjusted cables. And they replaced splitter. And
they told me I had a great connection. In other words, they were no help.

A long the way, I figured out how to check the logs on my [Motorola
SB6141](http://www.amazon.com/ARRIS-Motorola-SurfBoard-
SB6141-DOCSIS/dp/B00AJHDZSI/ref=pd_cp_pc_0) modem. I had purchased my own, as
the Time Warner provided unit was a pain. So I started watching the logs to
see what was happening. I found that it was routinely rebooting with [T3 or T4
timeouts](http://volpefirm.com/docsis_timout_descriptions/). I was convinced
that these issues were my provider’s fault and I decided that I wanted to
track this, maybe so I could request a refund or something.

So, opened up [Sublime Text](http://www.sublimetext.com) and started writing a
script. I started out creating a simple scraper script to check the log page
for updates. Using the [nokogiri](http://nokogiri.org) gem, I was able to
write a very simple script that could parse the table. I then added some code
to log that information to a [sqlite3](http://www.sqlite.org) database and
dump the number of modem reboots to the command line. I set this script up to
run in a continuous loop on an old Mac Mini I bought off my previous boss.

I was getting closer to understanding how bad the problem was. I now could
watch for the number going up. I also got a little clue that the number was
going up, as the database and the matching code were in my dropbox account and
I got a notification on my main computer whenever the file changed and a new
version was downloaded.

Still not enough, I turned to another bit of ruby magic to make it easier to
see the reboot data I was gathering. I created a new script using
[sinatra](http://sinatrarb.com) to create a simple web app that would read
data out of my sqlite3 database and show me the count. Still not enough, I
found the GoogleVisualr gem and used that to generate a chart showing the
reboots per day. Now I could watch for trends, though unfortunately I didn’t
see anything I could correlate to the outside world.

![Google Chart of the reboot history](https://dl.dropboxusercontent.com/u/1276
79/whotestedthis/rebootsPerDayChart.png) Google Chart of the reboot history


With my little script running all the time, I could tell when the network went
down, even when I wasn’t home. To my dismay, I looked at the chart and saw
times where the modem had rebooted a lot. The high was 31 times in a single
day. Thankfully that was not a work day.

Still not quite satisfied with my setup, I decided to take it to a new level.
I heard about [Pushover](https://pushover.net) from some site and thought it
sounded interesting. It’s a push notification app and web-service with an API
that you could use. I also wanted to do some push notifications for work, so i
bought the app and began experimenting. I soon found
[Rushover](https://github.com/bemurphy/rushover), a ruby gem for using
Pushover. Then I was in business. I built my own little wrapper around
rushover so I could manage how I used it and added it to my script. Along the
way, I changed my script from an endless loop to run as a LaunchDaemon service
every two minutes (some great tips on LaunchDaemon at
[launchd.info](http://launchd.info)). Now I had a reboot friendly system for
logging all reboots and notifying me within 2 minutes of a reboot.

So, now I had a logging and push notification system in place to keep me aware
of how bad my connection was. The problem was, I still was rebooting multiple
times a day.

So, I finally decided to call Motorola, since my modem was still less than a
year old. And after a frustrating discussion with a tech (who got a earful
from me at one point), a new modem was going to be sent to me. He assured me
that he did not think it was the modem. And after going through the completely
inconvenient shipping options (with me shipping first and go without a modem
for 2 weeks), I agreed to pay $5 for an advance replacement.

A week later, after the modem arrived and I had sufficient time to swap out he
units and call Time Warner to get the new modem authorized, I was up and
running again. Along the way, I took the splitter out of the equation, since
we stopped the TV part of cable. I haven’t rebooted since.

So all along it appears to have been the modem, or maybe the splitter. Either
way, the core problem has been resolved. My connection is back to the normal
quality and not dropping out all of the time.

Along the way though, I used the desire to get my Internet connection working
into a practical excuse to improve my scripting skills by:

  * Learning nokogiri
  * Writing my first sqlite3 database code
  * Implementing push notifications that are under my control
  * Learning how to use launchd to run my modem checker script on a regular schedule
  * Learning how to generate google charts in my sinatra app

I had a lot of fun doing this. It increased my skills and eventually I solved
the problem. In the coming days I will post a version of the scripts I’m using
to my github account. They are fairly utilitarian, but maybe someone will get
something out of them.


