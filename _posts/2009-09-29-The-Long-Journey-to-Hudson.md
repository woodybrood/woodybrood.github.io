---
layout: post
title: The Long Journey to Hudson
date: 2009-09-29
category: articles
tags:
- fitnesse
- hudson
---
I've been working with FitNesse for over a year now.&nbsp; I'm on my second incarnation of fixtures, and now my second incarnation of a Hudson implementation.&nbsp; Technically, it is the third, but I'll get to that.&nbsp; I thought I might share a little bit about this evolution and the final result.

#### **Incarnation One**

While I was getting started with my Fit based fixtures (incarnation one of my fixtures), one of the development leads I work with was working to make Hudson our CI tool.&nbsp; He decided to help out and had a team member write a Hudson plug-in for it.&nbsp; Unfortunately I wasn't ready for this and so it sort of got abandoned.&nbsp; When time came that I really needed to get FitNesse working under Hudson, I partly forgot about it and was partly hoping for an official plug-in that was mentioned on the FitNesse yahoo group.&nbsp; So I sort of skipped it and went looking.

#### **Incarnation Two**

So with my goal to get FitNesse running under Hudson, I went searching and found two things that set me on the path.

1. [Showing FitNesse Test Results in Hudson](http://andypalmer.com/2009/04/showing-fitnesse-test-results-in-hudson/) 
2. A post by Gregor Gramlich which had an xslt for translating the XML result from test runner to HTML (sorry, can't find the post any more) 

This was great.&nbsp; A few tweaks to the ant task from Andy Palmer, and I was up and running.&nbsp; Well, there was some work in Gant to get our stuff running under Hudson due to some environment configuration stuff we need, but otherwise it was working.&nbsp; I was getting ready to start moving this into use when...FitNesse updated with some changes I needed, and it broke.&nbsp; It was a minor break, so I tweaked the ant script again to launch FitNesse via a jar, rather than the class.

#### **Incarnation Three**

So i was all set.&nbsp; Things were working great and I was getting both HTML page results and JUnit style results in my local Hudson Instance.&nbsp; It was great.&nbsp; It worked just fine, until I ran a lot of tests.&nbsp; I kept getting Java Heap errors when I ran my whole suite.&nbsp; No problem when I ran a test or two, but run them all and boom.&nbsp; I started investigating and I found the memory was blowing out in the same place every time.&nbsp; It was always blowing up as new results streamed back from the server were appended to the string that was eventually going to be saved out.

I contacted Uncle Bob and he was having the same problem.&nbsp; Due to the addition of Slim, the amount of data being sent back when a suite runs increased significantly.&nbsp;&nbsp; This was causing the memory problems.&nbsp; Uncle Bob thought about it and decided that he needed to trim the fat in the protocol.&nbsp; So he changed the XML that is returned to the TestRunner to be simpler, smaller and more efficient.&nbsp;

So here I was, I had an approach for running the tests.&nbsp; They ran.&nbsp; They returned results without blowing things up.&nbsp; But Something was missing.&nbsp; The XML transform from Gregor still ran, but the output was all wrong; it was missing the HTML version of the page results.&nbsp; To make things more efficient, Uncle Bob had to stop returning the content, as the page content was the source of all our memory problems.&nbsp; This would have been a real problem, but Uncle Bob didn’t do this without a backup plan.&nbsp; Earlier this summer, he added the ability to keep the history of a test run and view it.&nbsp; Leveraging that work, he now returned the path to the history for the specific test run of each page in that execution.

Now I had to do some thinking.&nbsp; I have the results and I can transform them into JUnit results for Hudson.&nbsp; But my boss wants to see the HTML pages.&nbsp; So I think about this.&nbsp; I have an XML file that has the page address to the history.&nbsp; It’s not a full URL, just a page reference with a link to the history responder.&nbsp; So what do I do?&nbsp; What if I take the XSL transform and modify it.&nbsp; So I did some quick Googling and tinkering and I ended up with some changes to the XSL file to take the Page reference, build it into a proper HTML link and change the “format=xml” to be “format=html”.&nbsp; Now I had a HTML page and links.&nbsp; (I’ll post this file very soon.&nbsp; Waiting on permission, as I modified the original.)

However, how do I have a persistent FitNesse server running on my Hudson server?&nbsp; I probably over thought this, but it seemed to me that I needed to have an instance of FitNesse running as a service on the computer just to serve up the history pages when someone goes exploring a failure from Hudson.&nbsp; I’ll post my version of this for everyone soon in a FitNesse Tip very soon.

Today all I have the energy for is the story.&nbsp; Very soon I’ll post information on the following things:

1. The XSL transform that I got to work.
2. An ANT task that runs FitNesse and transforms the XML results TestRunner saves into an HTML page
3. My version of the trick of getting FitNesse running as a service on Windows.
