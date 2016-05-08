---
title: Keyboard Maestro as a Test Tool Part 2 - Browser Agnostic Form Filling
layout: post
category: articles
tags:
- keyboard maestro
- testing
- tools
- cybernetic testing
---
As testers, we all find ourselves having to do a lot of repetitive work. That's just the nature of the job. To test something to the level we think necessary, we might have to do the same steps dozens, or even hundreds of times. 

Recently I had a case of excessive form-filling-itus. I was testing a feature that required me to fill out the same form over and over again, just to get data into the state I needed it in. It was necessary, but it didn't matter too much what was in the form.

So, there are a few ways I could have addressed this. For example, I could have used, Selenium to automate it. I already have scripts and page objects to do that. I could have also used Selenium IDE to record the steps and then play them back. I also could have used a browser extension to do it. None of those solutions seemed like the most flexible way to go.

Instead I called upon Keyboard Maestro again and utilize it's ability to send keystrokes and run JavaScript functions. Here's a section of the macro:
![Form Filler Example](/images/form_filler_macro_example.png)

My previous example[^1] was pretty much straight forward; set a variable, type in one thing and copy that variable to the clipboard. There were a couple of things that I needed to do that were different this time. This involves multiple fields and and dealing with dropdowns. And when filling out these forms, it was OK if some of the data was the same, but having a different first name, last name and actual street address would be enough. To do this, I put to use the ability of Keyboard Maestro to run JavaScript in an action. So I built macros that selected a first name and last name randomly from a list of each, and another macro that picked a random number, street name and designator (St., Dr., Ave., etc.). Each of these saved the values in a Keyboard Maestro variable. Then it was just a matter of using "insert by typing" and "Type the zzz Keystroke" actions to fill in the forms and navigate between fields (sending tab keys as needed).

Here is an the action that produces the random street name. It's not fancy, but it works: 
![Random Street Name JavaScript Action](/images/random_street_address.png)

And since I created that as a separate macro, I now have it as a reusable action that I can integrate into future scripts. Right now I have three of them, Get Random Make First Name, Get Random Last Name, and Get Random Street Address. 

I chose JavaScript as the language for these macros, mostly because I was just generating data. Since this all on on a Mac, a macro can also call shell scripts or AppleScript. And those scripts can interact with the operating system, not just generate strings (technically JavaScript can be used for that too in the two most recent versions of OS X).

There are plenty of tools and options for automation on the Mac, and I use several of them. Each of them has their strengths and weaknesses. Keyboard Maestro is one of the strongest for this sort of interaction. The ability to build reusable blocks and send keystrokes made it a good fit. I have a few more examples of how I use Keyboard Maestro coming. So far they don't drift too far from a certain them, but maybe they will inspire someone.

[^1]: [Keyboard Maestro as a Test Tool Part 1 - Throw Away Emails](/articles/2016/04/28/keyboard-maestro-as-a-test-tool-part-1-throw-away-emails.html)