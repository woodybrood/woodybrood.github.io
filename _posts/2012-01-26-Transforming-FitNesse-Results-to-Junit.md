---
layout: post
title: Transforming FitNesse Results to Junit
date: 2012-01-26
category: articles
tags:
- fitnesse
- java
---
I've been meaning to do this for a long time.  Back when I was discussing how I got FitNesse to work on Hudson, I pointed out this [page](http://andypalmer.com/2009/04/showing-fitnesse-test-results-in-hudson/) from Andrew Palmer.  Since then, FitNesse has changed a little and I've had to adapt my script to work with those changes.  So here is my XSL to transform FitNesse results XML into Junit results:

``` xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSLTransform"> 
  <xsl:template match="/">
   <xsl:element name="testsuite">
     <xsl:attribute name="tests">
       <xsl:value-of select="sum(suiteResults/finalCounts/\*)" />
     </xsl:attribute>
     <xsl:attribute name="failures">
       <xsl:value-of select="suiteResults/finalCounts/wrong" />
     </xsl:attribute>
     <xsl:attribute name="disabled">
       <xsl:value-of select="suiteResults/finalCounts/ignores" />
     </xsl:attribute>
     <xsl:attribute name="errors">
       <xsl:value-of select="suiteResults/finalCountsexceptions" />
     </xsl:attribute>
     <xsl:attribute name="name"> <xsl:value-of select="suiteResults/rootPath" /> </xsl:attribute>
   <xsl:for-each select="suiteResults/pageHistoryReference">
     <xsl:element name="testcase">
       <xsl:attribute name="classname">
         <xsl:value-of select="/suiteResults/rootPath" />
       </xsl:attribute>
       <xsl:attribute name="name">
         <xsl:value-of select="name" />
       </xsl:attribute>
       <xsl:attribute name="time">
         <xsl:value-of select="runTimeInMillis div 1000" />
       </xsl:attribute>
       <xsl:choose>
         <xsl:when test="counts/exceptions > 0">
           <xsl:element name="error">
             <xsl:attribute name="message">
               <xsl:value-of select="count/exceptions" />
               <xsl:text> exceptions thrown </xsl:text>
               <xsl:if test="counts/wrong> 0">
                 <xsl:text> and  </xsl:text>
                 <xsl:value-of select="counts/wrong" />
                 <xsl:text> assertions failed </xsl:text>
               </xsl:if>
             </xsl:attribute>
           </xsl:element>
         </xsl:when>
         <xsl:when test="counts/wrong > 0">
           <xsl:element name="failure">
             <xsl:attribute name="message">
               <xsl:value-of select="counts/wrong" />
               <xsl:text> assertions failed </xsl:text>
             </xsl:attribute>
           </xsl:element>
         </xsl:when>
       </xsl:choose>
     </xsl:element>
   </xsl:for-each>
   </xsl:element>
  </xsl:template>
  </xsl:stylesheet>
```
o how do I use it?

First off I have the following ANT targets to run FitNesse and then capture the output:

``` xml
<!--Run the tests first using the command line runner with text output.  This lets us see the results as tests complete.-->
 
     <java jar="javalib/fitnesse.jar" classpath="${toString:install.classpath}" fork="true" maxmemory="1024m">
           <arg value="-c" />
           <arg value="${fitnesseSuite}?suite&amp;suiteFilter=${fitnesseSuiteFilter}&amp;excludeSuiteFilter=${fitnesseExcludeSuiteFilter}&amp;format=text" />
           <arg value="-p"/>
           <arg value="${fitnesse\_port}" />
        </java>
        <!--Then run the page history responder to get the latest run of fitnesse in xml format-->  
 
        <java jar="/javalib/fitnesse.jar" classpath="${toString:install.classpath}" fork="true" maxmemory="256m" output="${fitnesse.output.file}.temp" >
           <arg value="-c" />
           <arg value="${fitnesseSuite}?pageHistory&amp;resultDate=latest&amp;format=xml" />
           <arg value="-p"/>
           <arg value="${fitnesse\_port}" />
        </java>
 
        <!--Trim off the text that FitNesse sends before and after the XML-->
 
       <exec executable="${env.RPFITDIR}/tools/XMLTrim.exe" output="${fitnesse.output.file}.xml">
          <arg value="${fitnesse.output.file}.temp"/>
       </exec>
```

The latest result will be written out to a temporary file, but won't be true XML as there are http headers and traling characters around the XML.  XMLTrim.exe is a little executable that can trim off the http headers in front of the XML results temporary file.  I can't share it right now, but will post something cross-platform in the future.

I have the following ANT target to transform the XML to the format we want.

```xml
<target name="convert-fitnesse-results-to-junit">
      <xslt style="src/xsl/fitnesse2junit.xsl" in="${fitnesse.output.file}.xml" out="TEST-${fitnesse.output.file}.xml" >
       </xslt>
</target>
```

Then I configure Hudson to use the results I generated, and we're all set.

