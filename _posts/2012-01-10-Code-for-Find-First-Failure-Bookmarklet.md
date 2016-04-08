---
layout: post
title: Code for Find First Failure Bookmarklet
date: 2012-01-10
category: articles
tags:
- fitnesse
- bookmarklet
- javascript
---
It sounds like my attempt at a bookmarklet is limited to newer browser versions. I'm a bit of a Javascript hack, so I'm not sure what is the exact source of failure on older browsers.

In case anyone wants to improve on my bookmarklet, here is the original code:

``` javascript
var divs = document.querySelectorAll(".hidden");
var firstWithFailure = 0;
for(i = 0; i < divs.length; i++){
    if (divs[i].getElementsByClassName('fail').length > 0 ||
        divs[i].getElementsByClassName('error').length > 0){
        toggleCollapsable(divs[i].id);
    }
}
var failuresAndErrors = document.querySelectorAll(".fail, .error");
console.log(failuresAndErrors);
for (j = 0; j < failuresAndErrors.length; j++){
    if (firstWithFailure == 0){
            firstWithFailure = j;
     }
}
failuresAndErrors[firstWithFailure].scrollIntoView();
```

I used Bookmarkleter to create the bookmark using this snippet of code: http://chris.zarate.org/bookmarkleter.

