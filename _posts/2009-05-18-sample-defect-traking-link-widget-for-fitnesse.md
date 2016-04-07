WikiWidgets in FitNesse are a great way to extend the functionality of your
FitNesse pages. Essentially they let you rewrite the content of you page in
creative ways. My first forays into the creation of my own wiki widgets was to
add some specially styled text to make things stand out on the page. This has
been replaced by the new !style_class widget added by Uncle Bob a few releases
back.

That doesn't mean there isn't a need for a widget now and then. I wanted to
come up with a way to connect my wiki pages to our JIRA defect tracking and
workflow system at work. I wanted it to be easy for me and my team to be able
to do this with the smallest amount of wiki text. So I created a Widget for it
!JIRA{TEAM-11111} which gets transformed into a proper link to the JIRA
system.

I'm putting this out here for anyone who wants to reuse it. To make it work in
your environment, you will need update the URL to match your environment. It
can be further modified to work with any defect tracking system that supports
URL as a way to access the system. And if you aren't using JIRA, and many of
you probably are not, you can just rename the Widget as well.

<pre style="border: 1px dashed rgb(153, 153, 153); padding: 5px; overflow: auto; font-family: Andale Mono,Lucida Console,Monaco,fixed,monospace; color: rgb(0, 0, 0); background-color: rgb(238, 238, 238); font-size: 12px; line-height: 14px; width: 100%;"><code>    package common.wikitext.widgets;</p><p>   import java.util.regex.Matcher;<br/>   import java.util.regex.Pattern;<br/>   import fitnesse.wikitext.WikiWidget;<br/>   import fitnesse.wikitext.widgets.ParentWidget;</p><p>   public class JiraWikiWidget extends WikiWidget {<br/>       public static final String REGEXP = "!JIRA(\\{\\w+.*?\\})?";</p><p>       public static final Pattern pattern = Pattern.compile(REGEXP);</p><p>       private String originalText = "";</p><p>       private String token = null;</p><p>       public RpJiraWikiWidget(ParentWidget parent, String text) {<br/>           super(parent);<br/>           originalText = text;<br/>           Matcher match = pattern.matcher(text);<br/>           if (match.find()) {<br/>               token = match.group(1);<br/>           }<br/>       }</p><p>       private String formatTestIdentifier(String text) {<br/>           String tempText = removeBraces(text);<br/>           return "&lt;a class=\"dts\" "<br/>                   + "href=\"http://jiraserver/jira/browse/"<br/>                   + tempText + "\"&gt;" + tempText + "&lt;/a&gt;";<br/>       }</p><p>       private String removeBraces(String format) {<br/>           return format.replaceAll("\\{", "").replaceAll("\\}", "");<br/>       }</p><p>       public String render() throws Exception {<br/>           return (token != null) ? formatTestIdentifier(token) : originalText;<br/>       }<br/>   }<br/></code></pre><br/>

It's not the trickiest code in the world. But it's yours to take and to use.

So far this is a small contribution to the community. At some point in the not
too distant future I'll share my vim syntax coloring for FitNesse wiki pages.
I still am looking to make a couple of improvements before that goes out.

Comments and questions are welcome.

