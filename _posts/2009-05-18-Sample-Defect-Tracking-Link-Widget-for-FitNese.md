---
layout: post
title: Sample Defect Tracking Link Widget for FitNese
date: 2009-05-18
category: articles
tags:
---
WikiWidgets in FitNesse are a great way to extend the functionality of your FitNesse pages. Essentially they let you rewrite the content of you page in creative ways. My first forays into the creation of my own wiki widgets was to add some specially styled text to make things stand out on the page. This has been replaced by the new !style\_class widget added by Uncle Bob a few releases back.

That doesn't mean there isn't a need for a widget now and then. I wanted to come up with a way to connect my wiki pages to our JIRA defect tracking and workflow system at work. I wanted it to be easy for me and my team to be able to do this with the smallest amount of wiki text. So I created a Widget for it !JIRA{TEAM-11111} which gets transformed into a proper link to the JIRA system.

I'm putting this out here for anyone who wants to reuse it. To make it work in your environment, you will need update the URL to match your environment. It can be further modified to work with any defect tracking system that supports URL as a way to access the system. And if you aren't using JIRA, and many of you probably are not, you can just rename the Widget as well.

``` java
package common.wikitext.widgets; 
import java.util.regex.Matcher; 
import java.util.regex.Pattern; 
import fitnesse.wikitext.WikiWidget; 
import fitnesse.wikitext.widgets.ParentWidget; 

public class JiraWikiWidget extends WikiWidget { 

    public static final String REGEXP = "!JIRA(\\{\\w+.*?\\})?"; 
    public static final Pattern pattern = Pattern.compile(REGEXP); 
    private String originalText = ""; 
    private String token = null; 

    public RpJiraWikiWidget(ParentWidget parent, String text) { 
        super(parent); 
        originalText = text; 
        Matcher match = pattern.matcher(text); 
        if (match.find()) { 
            token = match.group(1); 
        } 
    } 

    private String formatTestIdentifier(String text) { 
        String tempText = removeBraces(text); 
        return "<a class=\"dts\" " + "href=\"http://jiraserver/jira/browse/" + tempText + "\">" + tempText + "</a>"; 
    } 

    private String removeBraces(String format) { 
        return format.replaceAll("\\{", "").replaceAll("\\}", ""); 
    } 

    public String render() throws Exception { 
        return (token != null) ? formatTestIdentifier(token) : originalText; 
    } 
}
```
  
It's not the trickiest code in the world. But it's yours to take and to use. 

So far this is a small contribution to the community. At some point in the not too distant future I'll share my vim syntax coloring for FitNesse wiki pages. I still am looking to make a couple of improvements before that goes out.

Comments and questions are welcome.

