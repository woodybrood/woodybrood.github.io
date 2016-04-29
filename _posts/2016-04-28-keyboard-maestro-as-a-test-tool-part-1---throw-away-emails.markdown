---
title: Keyboard Maestro as a Test Tool Part 1 - Throw Away Emails
layout: post
category: articles
tags:
- Keyboard Maestro
- tools
- testing 
- cybernetic testing
---
## Introduction
I'm rather fond of finding new tools and leveraging them for a variety of tasks. Often it is just a way to make some repetitive task a little easier, even if that task isn't directly testing related. 

For my day-to-day automation, I use Selenium to do automated test/checks of the system. Generally I use Se to do automation that doesn't have a human factor to it. I certainly could use it to automate other interactions with the web, but it's sometimes more work than I want to do for a given task.

A tool I've used for a while now for a handful of tasks is Keyboard Maestro,[^1] an OS X application for a wide range of automation of common tasks. What makes it interesting is the extensive set of tools it has available. It can utilize a range of ways to automate things on your Mac. 

It can:

- Send keystrokes to applications
- Use AppleScript/JavaScript to interact with applications
- Run shell scripts
- Track and interact with the clipboard
- Directly interact with Safari and Chrome
- Manipulate windows
- Manipulate files
- Take and manipulate screen shots

Keyboard Maestro uses a graphical programming style that adds actions to a macro as modular blocks. Each of those blocks can be configured to control how each block works. It also supports using variables and logic constructs to make your scripts more functional.

## Throw Away Emails
Every so often I have to test something that needs a unique email, but it doesn't have to be an email at a real domain. I don't know about you, but I can't create unique things on the fly. Not with just my head and my fingers.  So this is where I've applied some Keyboard Maestro Magic. 

The macro I below is  just a couple steps. 

1. Build an email address using some fixed strings (my initials and the domain) with a time-stamp of when the email was generated. The time-stamp was a suggestion of a co-worker as a way to help identify the data by when you were working.
2. It copies that email address to the clipboard, so if I need it again soon, it will be in my clipboard manager.
3. It types the email address into the active field.

![Keyboard Maestro script for throw away email](/images/throw_away_email_keyboard_maestro.png)

I use a typing trigger to launch the macro. Currently, I use *;;temp*. And trust me, there is a bunch of muscle memory tied to that trigger.

## Wrapping Up
This is just a first example of how I use Keyboard Maestro to help me to my testing. Sure, it's not hard to come up with an email. But this way, I can create an email I can identify as mine, self identifies with when I was doing the work, and gets saved to my clipboard manager (just in case I need to enter it again). 

I'll be sharing some more examples of how I use Keyboard Maestro to make me more efficient with a variety of tasks. Keep an eye open for more.


[^1]: [Keyboard Maestro 7.1: Work Faster with Macros for Mac OS X](https://www.keyboardmaestro.com/main/)