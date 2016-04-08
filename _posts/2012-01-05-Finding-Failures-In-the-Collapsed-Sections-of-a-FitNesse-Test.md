---
layout: post
title: Finding Failures In the Collapsed Sections of a FitNesse Test
date: 2012-01-05
category: articles
tags:
- fitnesse
- bookmarklet
- javascript
---
I've found that where I work, we use a lot of collapsible sections in our FitNesse tests. &nbsp;These collapsible sections are nice to hide less relevant steps, but they are a pain when they contain failures. &nbsp;There is nothing about a collapsed section that indicates there is a failure in it.

To address this, I have created a bookmarklet that can find all collapsed sections that contain failures. &nbsp;It then expands them and scrolls to the first failure. &nbsp;This makes things a little easier. &nbsp;As a bonus, it expands the scenarios that contain failures so you can quickly asses the step in the scenario that went awry.

If you are interested, you should be able to drag this bookmarklet from this link: <a href="javascript:(function(){var%20divs=document.querySelectorAll(%22.hidden%22);var%20firstWithFailure=0;for(i=0;i%20%3C%20divs.length;i++){if(divs[i].getElementsByClassName('fail').length%20%3E%200%20||divs[i].getElementsByClassName('error').length%20%3E%200){toggleCollapsable(divs[i].id);}}var%20failuresAndErrors=document.querySelectorAll(%22.fail,%20.error%22);console.log(failuresAndErrors);for(j=0;j%20%3C%20failuresAndErrors.length;j++){if(firstWithFailure==0){firstWithFailure=j;}}failuresAndErrors[firstWithFailure].scrollIntoView();})();">Find First Failure</a> to your bookmarks bar. Then next time you have a FitNesse test with a failure in it, click on the link.

Comments and suggestions welcome, as always.

&nbsp;

**Update** : &nbsp;I modified the javascript to handle exceptions an failures. &nbsp;It also should scroll to the first exception or failure in a test page (won't work the same when viewing a suite run, yet).

&nbsp;

**Update 2:** &nbsp;Ralf Kern has provided a version that works with older versions of IE and FireFox. &nbsp;Use this link if the above one doesn't work for you: <a href="javascript:(function(){var%20hiddenSection=document.querySelectorAll('.hidden');for(i=0;i%20%3C%20hiddenSection.length;i++){var%20section=hiddenSection[i];var%20allElementsOfSection=section.getElementsByTagName(%22*%22);for(j=0;j%20%3C%20allElementsOfSection.length;j++){var%20classname=allElementsOfSection[j].className;if(classname==%22fail%22%20||%20classname==%22error%22){toggleCollapsable(section.id);break;}}}var%20failuresAndErrors=document.querySelectorAll(%22.fail,%20.error%22);if(failuresAndErrors.length%20%3E=1)failuresAndErrors[1].scrollIntoView();})();">Find First Failure</a>.

