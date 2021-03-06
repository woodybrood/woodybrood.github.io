---
layout: post
title: Lessons Learned with Weld and Cucumber
category: articles
tags:
- java
- weld
- cucumber
---

I’ve recently started a conversion of Junit-style Selenium tests to using Cucumber. The goal of this is not currently BDD, but more about writing example driven tests that will focus on the significant behaviors in the system. But I’ll save my thoughts on the test authoring style for another day.Today I want to talk about my experiences converting the existing framework from being heavily singleton based to using Dependency Injection using Weld.

The [Pragmatic Programmers](http://pragprog.com) just recently released [The Cucumber for Java Book](https://pragprog.com/book/srjcuc/the-cucumber-for-java-book) which I have been following since the beta book was announced. It’s a very useful book if you are a tester/programmer in the Java world and are interested in using Cucumber. As an aside, I do in principle agree with Adamin his recent post that [you don’t have to write your tests in the same language as your developers](http://sauceio.com/index.php/2015/02/stop-being-a-language-snob-debunking-the-but-our-application-is-written-in-x-myth/), but in my case, I have a large number of Page Objects that I can reuse, and we use QueryDSL to generate database objects for interacting with the database of the system under test. So if you are like me, and you feel Java is the best choice to write your tests, I highly recommend this book.

I’ve been aware of Dependency Injection (DI) for quite some time, but have not really used any frameworks for it in any of my previous testing automation.But I was very intrigued by the chapter in the book that covered using various DI frameworks in Cucumber and that Cucumber is essentially DI aware. Having no personal opinion on on the available frameworks, I chose Weld, as that is what our developers use. Again, is is not required to use the same framework as your developers, if you are not directly using their code. In my case, I chose Weld because they do use it and are very willing to help with questions. Given the choice between learning on my own or leveraging their knowledge, I chose to leverage.

## Challenge Number One: Becoming Pojos

Weld requires that the objects you are using be Pojos, or [Plain Old Java Objects](https://en.wikipedia.org/wiki/Plain_Old_Java_Object). This means that all classes should have a constructor, with no arguments, or a constructor that takes a single argument where that argument is another injectable object.This meant architectural changes to the WebDriver wrapper, the logger, the database objects and the page objects. All of these needed to be changed to make Weld happy with them.

In principle this wasn’t too hard to do, but it took some discovery to find the various changes I needed. And even then, it required some additional trial and error (and help from my developer) to identify classes that were created using public fields rather than setters and getters. Weld is rather opinionated about class design, and that’s OK. I just had to get used to identifying where our code didn’t match what it wanted.

Typical changes I made included:

  * Making sure that instead of executing code in constructors, used @PostConstuct annotated methods. Weld injects objects after the constructor is called, so you wind up with NullPointerExceptions if you use injected fields in the constructor.
  * Converting public fields to private ones with setters and getters
  * Changing the page objects to skip validation steps that used to happen in the constructors, as the page objects are injected now. This means that the pages might not be active/visible when the constructor (and @PostConstruct methods) are called.
  * Converting some utility classes that were written using static methods to be injectable Pojos instead, as they needed other classes injected into them (and figuring a way around the circular dependency I created at one point).

These changes turned out to be reasonable, it just took a little bit of
education and effort to think through. I’m liking the result. I haven’t had
much issue with the code once I figured my way around the places where it
didn’t match what Weld expected.

## Challenge Number Two: Dealing With a Little Classpath Hell

Back in the old days of Windows development, we all had to deal with the special joy known as DLL Hell. Back then you had C and C++ based libraries that might have the same name, but have different entry points and software would crash when you called a version of a method that was different orc hanged in the version of the DLL found by the application.

Well the Java version is Classpath Hell. Where you have different versions ofJars being used by the various Jars you are dependent on. Java is better about this than the DLLs of old, but you can still get caught. I was initially usingWeld-SE 1.1.0 as the version of Weld in my pom file. But when I tried to run,I would get a MethodNotFoundException because the version of google collections I was getting didn’t have a method that an updated version ofSelenium wanted. I went through all of the dependency tree and tried doing exclusions of guava from other jars with no success. Eventually my developer(who was extremely helpful every time I needed him) dug all the way into theWeld jar and we discovered that they had actually incorporated the google collections code into their code and didn’t rename the packages. The result was that their version of the code was always being found. The fix was a newer version of Weld, but it was a bunch of headaches to get there.

## Challenge Number Three: Learning the Quirks of Cucumber Lifecycle

Cucumber is very well thought out. I really appreciate the separation of concerns between Feature doc, Step Definitions, Helper Code and the WebDriver.It makes for a great structure. It also enforces some rules and behaviors that were different than I expected. And some of these quirks had a relationship to how Cucumber worked with Dependency Injection. That isn’t bad, but it still requires learning and then adapting.

A couple of key things I learned:

  * @ApplicationScoped classes are potentially recreated on for each test. You may still need a Singleton to ensure that you get a single browserDriver created for the test run.
  * On a related note, I created SharedTestData class to carry data between step definition files. This worked well enough, until I tried to use it in a @Before and @After hooks. The SharedTestData was reset when the test ran, so values stored in the @Before didn’t make it to the @After. Created a separate SharedHooksData to carry data between hook files.

## Still Learning

There are still challenges waiting for me out there. I’ve just really started working through converting all of the page objects and creating the new tests.I’ve got a good start going and I feel like I’ve broken through the initial barriers. But I know that there will still be some behaviors I don’t expect inCucumber and with Weld. I’m better able to handle them now than when I started. It doesn’t mean there won’t be some cursing and some frustration, butI’m up for the challenge. I’ll come back and document the other challenges later.

