---
layout: post
title: FitNesse Tips
date: 2009-09-01
category: articles
tags:
- fitnesse
- tips
---
Recently I attended the 2009 AA-FTT workshop.&nbsp; While I didn’t get to go to the Agile 2009 conference, I enjoyed an opportunity to talk about the tools and techniques people have used to be more successful in their efforts to be Agile (big A or small a).

One of the sessions I attended was Advanced FitNesse Techniques.&nbsp; Well, it quickly evolved into a different session focused on Tips and Tricks.&nbsp; There were two of us who had been doing FitNesse for some time and two folks who were just starting out or evaluating it as an option.&nbsp; Adam Goucher recounted this recently on his blog and summed up a majority of what we discussed (see his post: [http://adam.goucher.ca/?p=1153](http://adam.goucher.ca/?p=1153 "http://adam.goucher.ca/?p=1153")).

So I’m not an expert at FitNesse, merely someone who has been a practitioner and champion in one context.&nbsp; What works for my project won’t necessarily work for you. So your mileage may vary.

I’m hoping to share with folks a number thoughts, tips and tricks that I’ve found in the past year.&nbsp; This first post will be a few quick things.&nbsp; In the later posts, I plan to take more time with a single topic.

So on to some quick tips:

- People often complain about the .zip files under their pages.&nbsp; These zip files are the rudimentary version control built into the system.&nbsp; if you add a “ **–e 0”** argument to the command line when you launch FitNesse, the zip files will go away.&nbsp; Alternately you can leave them there and just ignore them in your source control (you are using real source control aren’t you?). 
- If you do want to make sure that the those zip files never appear, or that the FitNesse server launches on a available port all of the time, create your own .bat file, alias, or shell script. Make sure it launches the way you want it to. 
- If you are in a fixture, learn how to rely on [Graceful Names](http://fitnesse.org/FitNesse.UserGuide.GracefulName) and use the automatic behavior to fix the words.&nbsp; FitNesse will remove the whitespace and let you write more naturally. 
- Use the output. In Java you can use **System.out.println()** to add to the output page.&nbsp; Log what you are doing .&nbsp; Output debug information. _I’m not sure what happens in other languages under Slim, so this might work differently there._ 
- Comment out big sections of your tests easily.&nbsp; {{{ and }}} are a powerful tool to comment out big sections of your tests to let you debug your both your product and your tests. 
- Create your own styles.&nbsp; Use the [!style\_name](http://www.fitnesse.org/FitNesse.UserGuide.MarkupStyle) widget to create your own styles for inserting comments or warnings into your tests.&nbsp; Add them to the fitnesse.css file and check that file into source control with your code (remember the source control). 

It’s a rather random list, but just a start.&nbsp; In the coming days and weeks I’m going to try to explore a number of different topics in more depth.

