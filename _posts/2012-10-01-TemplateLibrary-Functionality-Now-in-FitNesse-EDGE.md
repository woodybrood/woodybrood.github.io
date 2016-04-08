---
layout: post
title: TemplateLibrary Functionality Now in FitNesse EDGE
date: 2012-10-01
category: articles
tags:
- fitnesse
---
I'm proud to announce the addition of enhanced template and snippet support to The FitNesse edit/new page. A default template option has existed in FitNesse for some time now, but it involves modifying a file outside of the wiki and is singular in nature. The teams I work with create a variety of tests with different needs, so one table isn't enough. With the new TemplateLibrary feature, teams can now have multiple templates defined at multiple levels in the wiki, giving them new options and flexibility.

## How It Works

TemplateLibrary is a new special page. All immediate children of the page are considered template pages. The contents of those pages can then be inserted when editing a page. Template pages can be whole pages or just a single table. It's your template, you decide. TemplateLibrary pages can be created under the FrontPage or in suites. If you have TemplateLibrary pages under FrontPage and Under a suite page at contains the test, the children of both pages will be available.

## A Closer Look At The Structure

Given these pages:

FrontPage.TemplateLibrary.BasicTestTemplate FrontPage.TemplateLibrary.SuiteTempate FrontPage.TemplateLibrary.ScenarioLibraryTemplate FrontPage.WebSuite.TemplateLibrary.WebTestTemplate FrontPage.WebSuite.TemplateLibrary.ScenarioLibraryTemplate

This gives us the following list of templates that can be added to a test added to the WebSuite or any of the suites underneath it:

FrontPage._.BasicTestTemplate FrontPage._.SuiteTempate FrontPage._.ScenarioLibraryTemplate WebSuite._.WebTestTemplate WebSuite.\_.ScenarioLibraryTemplate

You will notice a couple of things:

- The wiki word TemplateLibrary has been replaced by an underscore. This makes the option in the dropdown a little shorter and easier to read.
- The path leading up to the suite containing the template was removed from the front of the templates defined at lower levels. Again, this was done to keep things shorter and easier to read.

## Using the Templates

When in the editor, there is now a dropdown that contains the list of the available templates. The templates available are come from the TemplateLibrary pages in the direct ancestry of where the new/edited page. The user selects the template in the dropdown and then clicks on the insert button. The template is then inserted into the current cursor selection in the editor.

## Available Now in EDGE

Please give TemplateLibraries a try. We look forward to your feedback. Currently it only works in the Plain Text mode, but we will be adding support for the new WYSIWYG editor in the near future. TemplateLibrary functionality is compatible with Slim, FIT and FitLibrary.

I want to thank Josh, a co-worker of mine who did all of the implementation. He did a great job and adapted to the ideas presented by other folks on the FitNesse committer team. It's his first contribution to FitNesse and I hope to see him get more opportunities to pitch in.

