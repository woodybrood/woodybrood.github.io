---
title: Still Testing, Developing, and More Importantly, Living
layout: post
category: articles
date: 2016-04-15
tags:
- cancer
---
Just thought I would drop a note. Last year I posted about my health[^1] and then I went silent on this site for quite a while. 

First off, I'm doing fine. I continue to be in remission with the Myeloma numbers looking great. Additionally, the regular blood work I have done shows that my other blood cells and my immune system are stable. Maybe on the low side for most people, but nothing to be concerned about.

This means I've been fortunate to be working full time (minus the occasional trip to the clinic for the infusion of a bone strengthener). My job remains challenging and interesting. I've continued my work with Cucumber[^2] and grown our suite of tests nicely. Along the way I've improved the infrastructure of our setup and while we have fewer tests/checks running than in the old approach, these tests are more stable and a lot easier to diagnose.

Due to the focus I have on automation, I've been doing less testing of features, though I still do some. Mostly I mix my time with monitoring the bug queue from the customer. This has me investigating a wide range of things. One of the highlights of that process has been the opportunity to grow my knowledge of the code and diagnose or answer some questions w/o a developer.

I've also started a new project to build a framework for benchmarking certain parts of the system. It's a very simple tool, it just allows for us to create short scripts and then send the timings or other values identified by the scripts into a database and then report on when the timing or value falls outside an expected range. My research didn't find anything like what I needed available, so I'm building it out. It's also an opportunity to learn Wicket,[^3] the web development framework we use.

So, I'm still here and I'm still kicking. And I'm still learning and growing.

[^1]: [A Tester Should Have Known Better](articles/2015/05/12/a-tester-should-know-better.html)
[^2]: [http://cukes.info](http://cukes.info)
[^3]: It's really Java intensive. I guess I might have preferred something else, but there is a lot in it that I don't have to do myself. And, since it is what our product is written in, it gives me a better understanding of what is possible and where things can fail.