---
layout: post
title: My Shallow Dive into iOS automation
date: 2012-08-28
category: articles
tags:
- ios
- automation
---
# iOS Test Automation Comparission

For the past couple of weeks I've been diving into iOS testing. While I would not describe my investigations as exhaustive (one might even call it a shallow dive), I have learned a few things that I think are worth sharing about a handful of the options out there.

Before I get started with that, I should share a little bit about how I started this process. I like to think of my self as being capable of most anything on a computer. Ok, maybe I should say that I'm willing to try most things. Anyways, I decided that it was important for me to understand the inner workings of things. While I probably will never get a job writing iOS apps, knowing how to connect controls up and make them accessible to automation would be very useful. So I downloaded Xcode and worked my way through the first couple lectures of the Stanford course on iOS development. After doing the first couple of homework assignments, I had what I wanted: an app to test.

Now, it is a very simple app, but one that I could modify at will to make sure it works with automation. I am sure that a more complex app would introduce things that might make one tool or another a better choice.

## The Playing Field

There are a few flavors of test automation tools available for testing iOS apps. So far these fit into the following categories:

- UIAutomation via Apple Instruments
- Automation By Wire
- Jailbreak Dependent

## UIAutomation via Apple Instruments

Instruments is a tool that comes with Xcode and is used in the process of testing and profiling iOS and OS X applications. UIAutomation runs via Instruments and can execute tests both on the device and in the iOS Simulator.

## Automation By Wire

While not an official term, the name seems to fit. Automation By Wire identifies a category of test tool that injects/adds some code to the application and then allows software running on a computer to connect to a server in the app and then run tests against said app.

Worth noting about the Automation By Wire approach:

- Relies on linking a library or building code into the application
- This code can be injected using one build target and the production software can be built with a different target
- The change in binaries between test automation and the app store could be a risk, though it is probably really small

## Jailbreak Dependent

One option for being able to interact with and control the app is to Jailbreak the iOS device. Jailbreaking allows the Test Tool to install an application onto the device that allows for interaction via violating the normal security model of iOS. This software is then controlled by remote calls from a computer where the test is run.

I believe that jailbreaking is safe for this purpose. I haven't done it myself, but i know folks who have. The big concern i would have is if Apple released a version of iOS and the Jailbreak community is unable to hack it for an extended period of time, we would be unable to run our tests. It is unlikely that this would continue for a long time, as the community is pretty fast at getting new versions jailbroken. Never the less, it is worth noting the risk. Your comfort with that risk will guide your decisions.

## Commercial Tools

Of course, commercial tools exist for mobile app testing. Most of these tools are relatively new to the market in comparison to their desktop counterparts. My research hasn't identified a clear leader in this category. Though all of the major players appear to have something.

It seems that most appear to function using the Automation By Wire approach or by Jailbreaking. They inject/add code to the app that allows them to observe the app and make it testable. Other tools claim to be able to record the execution of tests on the device via the USB port and run those tests through the same mechanism (supposedly without jailbreaking).

# Challenges and Caveats to iOS Testing

The following challenges have been identified:

- **Design for testablilty is as important in iOS applications as it is in any software product.** An iOS App can be made in such a way that it is very difficult, or almost imposssible for tools to interact with the app consistently.
- All of these tools when pitted against a basic or standard iOS app can be used without significant extension. Equally so, all of these tools will require additional effort when the application under test is designed in such a way that it makes testing difficult or uses custom controls.
- All of the tools for testing on iOS and Android are much less mature than those for desktop systems. 

# Tool Notes

## Instruments (_Apple Official_)

Instruments is a collection of tools that allow developers (and testers) to monitor and test their iOS application. Specifically to test an application, the user would choose the Automator tool and would then write automated tests in Javascript. These tests can be run on the device or in the simulator. Additionally, you can analyze memory, cpu and other factors while running your application.

Instruments has no specific parallel on Android or in other platforms. It would be a unique running and authoring experience.

**Testability Note:** While investigating Instruments, I discovered that UILabel controls are not a good choice for updating text that you might want to check in a test. There is a known issue with them, as sited in this O'Reilley post: [How to use UIAutomation to create iPhone UI tests - O'Reilly Answers](http://answers.oreilly.com/topic/1646-how-to-use-uiautomation-to-create-iphone-ui-tests/ "How to use UIAutomation to create iPhone UI tests - O'Reilly Answers"). I modified my app based on this to use UITextField controls that had been disabled. I also added Accessibility Labels to my buttons to make them easier to work with (something that all tools benefit from).

### Additional Notes

- Uses Javascript as the programming language
- Supports suspending and resuming application via home button
- Supports running memory and cpu utilization logging in sync with test execution
- Supports capture/playback
- While it looks like it is getting easier to do, there are challenges running it in CI as Apple does not seem to provide ways that are consistent with other development environments
- Is extendable within it’s own tooling using libraries like tune_up.js which provide assertions and a test declaration format. Tuneup_js also appears to have added a command line runner through a ruby script.
- Tool does not indicate when javascript is invalid (may require other tools to check for obvious syntax issues). In fact, the tool is user hostile when there are javascript errors.
- Logs are both verbose and lacking at the same time

# [Frank-Cucumber](http://www.testingwithfrank.com "Frank-Cucumber") (_Open Source_)

Frank-Cucumber is a ruby toolset for automating iOS app tests. It uses a BDD (Behavior Driven Development) style of test authoring. This style is focused on developing the tests in the language of the business and then allowing the developers and testers to separately write the wiring software that connects the words to the invocation and checking of the system. Cucumber is a style that is growing in popularity and there are several books now available on the topic. Personally, I have known about Cucumber for some time now, but have not had a reason to try it. The experience is positive, but I should save that for another post.

``` cucumber
    Feature: Simple Multiplication
    
    Scenario: Multiplying Two Numbers
    Given I launch the app
    Then I should see a Result of 0
    
    When I enter 75
    And I enter 3
    And I tap *
    Then I should see a Result of 225
    
    Scenario: Multiplying Three Numbers
    When I enter 3
    And I enter 4
    And I enter 5
    And I tap *
    Then I should see a Result of 20
    And I tap *
    Then I should see a Result of 60
    
    Scenario: Multiplying Three Numbers in a different order
    When I enter 3
    And I enter 4
    And I tap *
    Then I should see a Result of 12
    And I enter 5
    And I tap *
    Then I should see a Result of 60
```

The Given-When-Then style of tests used in Cucumber follow the concept that steps using “given”, “when” and “and” are all steps that drive the application to the point you want to check. The “then” steps are used to check the state of the software. Step descriptions are matched against the sentences and phrases after the _given, when, and, then_ keywords. Developers and/or Test Engineers could write step descriptions. Frank examples of the step descriptions are as follows:

``` ruby
    Then /^I should see a Result of (\d+)$/ do |result|
        list_of_text_contents = frankly_map( "view:'UITextField' marked:'Result'", "text" ) 
        list_of_text_contents.should have(1).item # make sure we only matched one view with our selector 
        list_of_text_contents.first.should == result
    end
    
    When /^I enter (\d+)$/ do |value|
      value.each_char do |number|
        touch "view:'UIButton' marked:'#{number}button"
      end
      touch "view:'UIButton' marked:'Enter'"
    end
    
    When /^I tap \*$/ do
      touch "view:'UIButton' marked:'*button'"
    end
```

Frank-Cucumber is an Automation by Wire style tool. It provides functionality for preparing your app for testing and to build out a structure for the tests to run within it. You then write your tests and create your step descriptions. When you run your tests, the cucumber tool runs your test files and uses Frank to drive the application. Results are displayed at the command line. The results include pass and fail results.

### Additional Notes

- Frank is the fastest of the tools I’ve tried (and it is really fast)
- It does not support the full range of gestures
- It is GPL3 which is a less favorable license for some use

Frank is actively being developed. It has an active community mailing list on Google Groups.

# [Calabash](http://calaba.sh "Calabash") (_Open Source_ - _Commercial Hosted_)

Calabash is a similar concept to Frank. It also uses Cucumber and ruby to do it's thing. It also provides tools to bootstrap your app. In fact, I didn't rewrite my tests to work in calabash, merely revised the step descriptions to match calabash's internal APIs. So, let’s look at the step descriptions:

<script src="https://gist.github.com/3342093.js?file=calabash_style_calc_steps.rb"></script>

The differences between Calabash and Frank are as follows:

- Calabash has a nice syntax for using steps inside steps, which makes them more readable.
- Calabash supports a broader range of gestures.
- Calabash has a more permissive license.

Additionally Calabash-ios has a cousin called calabash-android. This can be used to make similar tests on android devices. There are commonality in the provided step descriptions and the syntax which would reduce some overhead. In theory, you could share the tests, but in practice there might be challenges to do that.

Finally, Calabash offers an optional hosted solution. This solution allows tests to be authored locally and then uploaded to servers on the hosting company and run on devices there. The hosted solution is not free, though it is reasonablly priced to try out. I haven't tried the hosted offering.

Calabash is actively being developed. It has active user mailing list on Google Groups. The developer appears to be quite responsive there.

# Silk Mobile (_Commercial_)

SilkMobile is a fairly standard commercial tool offering. It supports capture/playback style of test authoring and seems to hope that that is what users focus on. It supports Android and iOS devices, as well as Blackberry and Windows Mobile. In doing this, it does seem to allow a common user experience in test authoring/execution.

Silk Mobile also supports exporting their test steps into other programming languages for use in unit testing. These exported tests are coupled with the SilkMobile app through the Elements that are captured as a part of the test authoring process. The unit tests can then access image capture and OCR functionality provided by the SilkMobile.

## Concerns

SilkMobile appears to be a rebranded version of Experitest. The UI is the same, other than the branding and the videos demonstrating the product are virtually identical.

## Additional Notes

- Works best on jailbroken devices, but supports the Automation By Wire style
- Runs only on Windows
- Supports Android and iOS (as well as other devices)

# Other Paths, Not Taken

There is a multitude of tools out there that I have not looked at yet. Among them might be a real standout, but I have not had an opportunity to investigate them yet. Oth options that I could have looked at include:

- Soasta: Commercial, hosted solution
- HP Unified Software Testing: Comercial
- Bwoken: Open source tool that uses CoffeScript
- Telerik Test Studio: Commercial
- TestComplete: Commercial

# In Conclusion

I don't have a specific recommendation. I like Frank for its speed. I like Calabash for the broader functionality and the macro functionality. I sort of like instruments for the ability to run tests and diagnostics together.

I don't recommend SilkMobile. I admit that I have alias against commercial tools, but based on my experience, most of what I want out of any co numerical tool is the engine. Thous I would end up using the junit runner, and I would be spending a lot of money on what is essentially a library.

Whatever the case, our mileage may vary and you may want to investigate all of these for yourself. I encourage you to. Check out the following links for more information:

- [Dominik Dary on Calabash-iOS](http://dary.de/2012/04/calabash-ios/)
- [Dominiks's list of open source mobile test tools](http://dary.de/2012/03/open-source-mobile-test-automation-frameworks/)
- [Getting started with Frank](https://vimeo.com/44224224)
