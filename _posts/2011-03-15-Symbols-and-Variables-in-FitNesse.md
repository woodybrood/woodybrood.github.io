---
layout: post
title: Symbols and Variables in FitNesse
date: 2011-03-15
category: articles
tags:
---
Symbols and variables serve two different purposes in FitNesse.

Variables (which are a little bit more like constants or macros) render when the HTML in a page is generated. So this happens 100% in FitNesse before the test is even run. Variables can be defined at one level and then inherit down through a structure and can be pulled from the environment fitnesse is launched in as well.

Variables have the advantage that when they render, you can't tell that a variable was even there. This is often used to define commonly used values inside a page to reduce risk of mistyping.

Symbols on the other hand only contain values during the execution of test. In that way they are more like a traditional variable. It also means that they have a limited scope. Currently that scope is the current test page. Symbols are cleared out completely when a test is done. The main use for symbols is to move values from one table to another. Thus getting the value for something in one fixture that you need to use in another.

Symbols have the advantage of being very visible in the page even when the test isn't executed. This has it's merits as well, as sometimes $twelveMinutesFromNow is more meaningful to read than the actual time rendered by a !today widget stored in a variable. Also, as I mentioned before, symbols can be set and used while the test is running to store information not known when the test started, this variables cannot do.

Which to use depends on what you are doing in your tests. &nbsp;Both have their value and their advantages.

