---
title: Keyboard Maestro as a Test Tool Part 3 - My Version of Bug Magnet
layout: post
category: articles
tags:
- keyboard maestro
- testing
- tools
- cybernetic testing
---
![Keyboard Maestro implementation of Bug Magnet](/images/keyboard_maestro_bug_magnet.gif)

## Bug Magnet 

Bug Magnet[^1] is one of Gojko Adzic's numerous contributions to software development and testing. It's an exploratory testing aid and extension for Chrome and Firefox. It keeps on hand a number of useful strings that you can enter into your web application as you perform exploratory testing. There is no script for what to use, just a handy collection of inputs to expose certain common errors. 

I like Bug Magnet. It's good to have little tools like this to extend our resources and maybe trim a little time off. Some of the entries are obvious, and one can argue that you could just remember them. Others are impossible to just type in by hand. I have it installed in Chrome, and I've used it often. But I also like to use Safari in my testing, and there isn't a version for that. 

I could have taken the time to find a way to port Bug Magnet to Safari. Totally was an option. Right now, I just don't have the time, nor the desire to figure that out. Instead, I went and created a collection of Macros in Keyboard Maestro.


This installment of using Keyboard Maestro as a test tool is actually only a slight deviation from the previous two: Throw Away Emails[^2] and Browser Agnostic Form Filling[^3]. Again, the macros are using a lot of pasting or typing into fields. 

## Palettes
![Roll-up Palette](/images/keyboard_maestro_rollup_palette.png)

What is different here is the addition of palettes to the workflow In Keyboard Maestro, palettes are like a pop-up menu that is summoned on screen. You can then choose the action you want to take from the palette. If multiple pallets are sharing a hot key, they you can choose which one you want to use. Keyboard Maestro could also be configured to expand all of these macros by some sort of typed keyword (which is how I trigger the throw away emails [^1]), but that's a lot to remember. Bug Magnet's great virtue is that you can pick from a list of things to decide what makes sense in a given field. Being presented with the choices is part of the value. So palettes match the original functionality well.

![Keyboard Maestro Palette for group](/images/macro_group_configure_with_palette.png)

One last thing, to trigger this, I'm using my Caps Lock key as a hyperkey, as explained by Brett Terpstra[^4]. Each of my macro groups is bound to ^⌥⇧⌘Delete, which I can hit very easily using Caps Lock + Delete. All of them get rolled up into a higher level palette that you can pick from with the mouse or keyboard, as you can see in the animated gif above.


You can download my macros as a library here: [KM Bug Magnet](/downloads/KM_Bug_Magnet.kmlibrary)

[^1]: [https://github.com/gojko/bugmagnet](https://github.com/gojko/bugmagnet)
[^2]: [Keyboard Maestro as a Test Tool Part 1 - Throw Away Emails](/articles/2016/04/28/keyboard-maestro-as-a-test-tool-part-1-throw-away-emails.html)
[^3]: [Keyboard Maestro as a Test Tool Part 2 - Browser Agnostic Form Filling](/articles/2016/05/08/keyboard-maestro-as-a-test-tool-part-2-browser-agnostic-form-filling.html)
[^4]: [Useful Caps Lock Key](http://brettterpstra.com/2012/12/08/a-useful-caps-lock-key/)
