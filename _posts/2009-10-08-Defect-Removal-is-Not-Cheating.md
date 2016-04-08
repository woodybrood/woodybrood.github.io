---
layout: post
title: Defect Removal is Not Cheating
date: 2009-10-08
category: articles
tags:
- testing
---
Recently while watching my Twitter feed I ran across this:

> testobsessed: AACK! RT @dhemery I recently heard of an Agile team accused of cheating for running tests of their own before handing off to QA group.

There is so much wrong with this.&nbsp; And this was shortly after a similar tweet from Elisabeth:

> .@ [andrew\_paradigm](http://twitter.com/andrew_paradigm) Srsly? You've seen Test Mgrs shun FitNesse tests to increase bugs for sys tstrs to find? OMG. Malpractice. Wow. I'm sorry.

I ranted a bit as a result. And ranted again.&nbsp; And Elisabeth patiently asked, “What can we do to&nbsp; fix this?”

And I was stuck.&nbsp; I’ve had a number of test leadership roles in three different companies.&nbsp; So my breadth of environment experience isn’t huge, but it isn’t super narrow. I’ve never run into anyone who put their turf that far ahead of the quality of the product.&nbsp;

While I don’t have a reason to doubt Dale or Andrew, I’m pretty sure an exchange like this on twitter would be considered hearsay in a court of law.&nbsp; Never the less, I can imagine it happening.&nbsp;

Back to the question Elisabeth asked, “What can we do to fix it?”&nbsp; I don’t know if I know how to fix it, but I have some thoughts on how to challenge these Test Managers.&nbsp; And I’m going to use some classic QA/Testing concepts to do it.

### Cost of Quality

First off, the classic cost of quality graphic.&nbsp; This is the classic graph that we’ve seen in testing for a really long time now.&nbsp; The dollar values you assign at each level can change, but the message is always the same.&nbsp; The cost of removing a defect goes up as the product moves through the process.&nbsp; Defects discovered in requirements are cheaper to fix than defects discovered in coding.&nbsp; Defects discovered when coding are cheaper to fix than the ones found in system test.&nbsp; And the defects removed in system test are cheaper than those discovered in production.&nbsp; The slope is significant and the cost gets pretty crazy.&nbsp;

Agile is all about the cost of quality.&nbsp; The time between requirements, coding and testing is minimized.&nbsp; The customer is involved more and the process is focused on building the right thing all the time.&nbsp; TDD, Agile Testing, Exploratory Testing, Executable specifications; all of them are focused on improving the quality of the product and finding problems earlier.&nbsp; The QA group in both tweets is working at a higher cost of quality than the agile team.&nbsp; It doesn’t make them less valuable, but it does mean that they should want fewer bugs to make it to them to test.&nbsp; This means the project will cost less and the customer will be happier with the product.

### Why Do I Always Have to Unit Test the Developers Code?

I can’t tell you how many times I’ve found testers complaining about the code being “thrown over the wall” or “didn’t they even try this?” So often testers complain about the what they perceive as low quality code.&nbsp; They are frustrated that they can’t even get at the real bugs because they have to wade through all of these little bugs.&nbsp; I’ll be willing to bet that those same Test Managers have commiserated with their team about the quality of the code that they were getting before the team went Agile.

The Agile team is most likely removing the issues that the testers would find right away, and complain about.&nbsp; Sure those defects would have made their defect discovery numbers higher, but were keeping those testers from getting to something more sinister that could have made it past the team.&nbsp; Sure that hidden bug might have been more work, but that’s the job.

### Defect Removal Effectiveness Metric

What QA/Test Manager in the 90s didn’t at some time pick up a copy of [Metrics and Models in Software Quality Engineering](http://www.amazon.com/Metrics-Models-Software-Quality-Engineering/dp/0201729156/) by Steven H. Kan?&nbsp; I had one (two jobs ago).&nbsp; One of the metrics he discusses is Defect Remove Effectiveness.&nbsp; The idea behind the metric is that you should be able to measure the number of issues identified at each phase of development.&nbsp; This will let you understand what portion of the defects are removed at each level.&nbsp; Knowing this lets you understand where you need to focus your efforts and which processes are the best at eliminating problems.&nbsp; This is classic process improvement.&nbsp; Measure what you’re doing and see where you need to improve.

This last one is the hardest and maybe is me going a step to far.&nbsp; It’s a hard metric to get, unless you track everything in agile or in waterfall.&nbsp; That much tracking and measurement isn’t actually very agile and wouldn’t go over very well with the agilists.&nbsp; The catch is, I think those test managers wouldn’t like this either.&nbsp; It might point out that they aren’t that effective.&nbsp; I might point out that they need to evolve.

### Where Does This Leave Us?

I don’t know.&nbsp;

I doubt that my thoughts here are going to be enough to change the minds of the Test Managers Dale and Andrew talk about in their tweets.&nbsp; So often when people go into defensive mode, they aren’t going to listen to a reasoned debate. At the same time, I don’t know that these folks cannot be convinced.&nbsp; Maybe they are up for the challenge of finding those tricky bugs that make it through unit tests, TDD, executable specifications, and agile testers.&nbsp; Maybe not.

I do know this.&nbsp; I work at a place where we are becoming more agile (and the small ‘a’ is intentional).&nbsp; We’ve got a ways to go, but we’re on a good path.&nbsp; We still have what we call the “Traditional QA Team” who are outside of development and we have our Dev Test team that is integrated into development (as are our BAs).&nbsp; Their manager and I get along really well.&nbsp; We’re talking and trying to figure out how to make the whole process more effective.&nbsp; He’s never once told me that we were “cheating”.&nbsp; He’s never once told me that we removing too many bugs.&nbsp;

Of course, I did say we’ve still got a ways to go, didn’t I?

