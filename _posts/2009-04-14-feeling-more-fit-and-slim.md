---
layout: post
title: "Feeling more Slim than Fit"
date: 2009-04-14
---

It's been forever again since I last posted. My last post was on FitNesse and
my thoughts on the licensing scheme it uses. Well, Uncle Bob was already
thinking about this and is hard at work making it possible to release under a
more permissive license.

Part of how he is doing that is a move to a new table runner (interpreter)
called Slim. I have to say, that for me and my work, Slim is the new Fit. My
first forays into writing fixtures for FitNesse were less than perfect. Due to
my weak Java skills and the pressures to get a solution in place, I came up
with a solution that relied on inheritance and code generation. Neither of
which are that bad a thing, but combined with a number of other factors, have
proven to be less than my ideal.

Slim has already started to change that. Slim is much easier to write fixtures
for. Slim eliminates inheritance on a very tricky parse tree concept and
replaces it with lists of lists of strings. This has it's limitations (its a
little stricter about data and it involves building the lists to return to the
FitNesse server and the Slim table), but those limitations are worth it. One
could argue that they aren't actually limitations, but good practices or more
consistent approaches.

I was able to create a list fixture for listing sql select statements in about
125 lines of code and replace hundreds of code-generated classes in my old
system, because it was so much easier to build a list of lists to return than
to bend RowFixture to my will. I was able to replace hundreds of code
generated RowEntry fixtures with a single class that is used in scenario
tables and scripts. Eventually, I will be able to convert the 200+ tests I
have to use the new table styles and shrink to a handful of actual classes
that I, or someone on my team, had to write.

As for the classes that we will need beyond the one driver and the one list
fixture, they will only be necessary for special cases where we need to
condense table-like data into XML and where it would be a lot less elegant to
build it using the scenario tables.

It's late and I'm getting tired, but I will be posting more about FitNesse. I
doubt I can out-do the wonderful tutorial available here:
http://schuchert.wikispaces.com/FitNesse.Tutorials. Brett is doing some great
stuff there. I will try to contribute what I can to the community. So it might
be some thoughts and pointers. Eventually it will be the vim syntax coloring
file that I've been working on.

Before I go, I want to re-iterate my gratitude to Uncle Bob and all of the
other great people who are contributing to the current Renaissance in the
FitNesse community. There is great stuff afoot here and I'm glad to be in a
situation where I can experience it and contribute in my own little ways.

