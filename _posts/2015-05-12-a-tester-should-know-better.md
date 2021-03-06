---
layout: post
title: A Tester Should Know Better
category: articles
tags:
- cancer
- ponderings
---

So, I have cancer. It's a rare blood cancer called Multiple Myeloma. I was
diagnosed back in December and quickly started induction chemo. Before I get
too far into this, things are going well. The treatments have beaten down the
cancer and I'm feeling OK.

Throughout the treatment process I've had lots of _tests_ done: bone surveys,
MRIs, PET scans, bone marrow biopsies, blood work, and more. MRIs, X-Rays and
PET scans are all very interpretive in nature. There are numbers and
measurements (such as the size of a tumor), but they are visual in nature and
not as easy to get a handle on and may even require special software to view.

So along the way, well I made a couple of mistakes that I shouldn't have as a
tester.

## Mistake Number One: I Didn't Confirm What Was Important to the Stakeholders

Blood work is all about the numbers. I can't speak to the process for any
other cancer, but treating and managing Multiple Myeloma (MM) is a lot about
numbers. Lots of numbers. And going into things, I had no real idea what
numbers mattered. Well, all the numbers matter, but only certain numbers
matter when it comes to the cancer itself. Others are secondary, but related.
And then more just are there because they help identify other problems that
might exist in the system (also known as my body).

Now being a tester and a former QA/metrics guy, I started to explore the
numbers to figure out what they mean. And when I started, I had no idea what
the meaningful numbers were. On the typical week, I had enough blood drawn to
generate a page full of results. And some weeks, there was a bunch more. Early
on I was tempted to, but decided not to chart how many vials of blood they
took (the high in one sitting was 12).

Unfortunately, I didn't press the doctors (SMEs) from the start on what the
numbers all meant. I got clues early on about some of them, but I didn't seek
clarification. Some of them I knew from a layperson's perspective (white blood
cell count, red cell count, platelets were all names I recognized). After a
while I picked up some more, such as they were interested in my Absolute
Neutrophils (which I learned are essential to fighting off bacterial and
fungal infections). It turned out that this was a really important number to
know about, as I was Neutropenic in late December when I suffered from Tumor
Lysis and needed to be extra careful.

Over time I learned more. From MM podcasts and MM forums came the all
important "M-Spike". I realized, I didn't know where or what mine was. When I
was in the office, it was mentioned as important for getting Myeloma under
control (it should be zero), but we didn't confirm where to find it in my
labs. Eventually some internet research paid off and I knew more about what
the M-Spike was - an unusual concentration of monoclonal proteins based off of
a specific source - then I was able to examine my various test results and
find the value. The process of identifying the M-Spike involves interpretation
by a pathologist, so it isn't automatically collected and inserted into a
numerical field. Instead it is part of a longer text field that explains the
data. This also meant that MyChart by Epic Systems can't treat it like numeric
data and generate graphs of it changing (more on that later).

_On a completely unrelated note, you can be come a Non-Secretor. This means
that you have Myeloma and the cancer is growing inside you, but the version
you have has decided to stop creating the monoclonal proteins that make it
noticeable. Then blood work isn't as useful and you need more PET scans._

There are other numbers that I found out were directly related to Myeloma and
others were indirectly significant. Directly related is a thing called Free
Light Chains and the Free Light Chain Ratio. These are building blocks of
antibodies that get out of whack due to the bad Plasma cells created by
Myeloma. There is a range that these numbers should be in and there is a ratio
between the two that is normal. You pretty much want your M-Spike gone AND
your kappa and lambda light chains in balance. _If I was going to play with
analogies, M-Spike and Light Chain Ratio are a combination of direct bug
reports and related issues in your system, you want them to be zero or at
least under control._

Those indirectly related numbers. Well those are your normal blood cells,
neutrophils, uric acid, LDH, albumin, calcium and other blood chemicals and
cells. The normal blood cells get suppressed by the overgrowth of Myeloma
cells. This leads to anemia and immune system weakness. Other blood chemicals
can get out of balance due to cells being killed by the treatment. The doctors
monitor all of these numbers so they can respond with treatments to correct
when things get out of alignment. _My weak analogy here is that the indirect
numbers are a bit like the performance metrics (memory, cpu, disk access) for
the system including your software._

Now I'm not the tester in this situation, but I'm a key stakeholder along with
my treatment team. How often is it that a key stakeholder doesn't really know
what is going to deliver value? Well, more often than what you would guess.
Thinking like a tester though, what I should have done is taken more time to
ask what the numbers meant and what we wanted to see in the long run.

## Mistake Number Two: Taking a Single Sample as an Absolute Truth

As I learned more about the numbers, I started asking about more of the tests.
Eventually, I learned more about the results of my Bone Marrow Biopsy. The
initial one found a lot of Myeloma. Additionally, they found that Myeloma
cells were free floating in the blood, which isn't normal. Apparently they
were pushed out into the blood from the very full marrow.

At the conclusion of induction chemo (treatment to knock down the Myeloma and
prepare me for a stem cell transplant), we did another biopsy. The second
biopsy was done in the same hip as the first time. A few days later, When I
got the call from my nurse practitioner and she said that they found no sign
of Myeloma in my bone marrow. Well, I got excited, and I told my family about
it, and everyone was very happy for me.

Well, as a tester, I should have remembered that this was one data point.
Before you freak out, it's not bad news. Just not as good as what I initially
latched on to. A couple weeks later I had to go in for more tests and that is
when I realized that my M-Spike was not zero, but 0.35. I started at 4.06, so
that is still a huge drop. In Myeloma terminology it's called a Very Good
Partial Response and its the best that some patients get.

Later, a second PET scan showed no active clusters of Myeloma in my bones.
This being another very positive result. Just like the clean bone marrow
biopsy, and a low M-Spike, this points towards a system that has been brought
under control. Taken by itself, it would still be insufficient, but in
combination with the blood work, skeletal survey and biopsy, it does indicate
a strong success. So I've still got some of those bad cells out there, just a
lot less an not clustered in big groups. But that's OK.

But lesson learned. As a tester I should know better than to take one good
result as gospel. Think about it. A single sample in one location can miss an
important find somewhere else. This is why we don't rely on a single test or a
single approach in software testing. We know that there could be bugs hiding
next to the spot that just worked for us. And we also always know that even
with our best efforts, we are not going to find every bug in the software. We
need enough testing and enough data to establish the risk in releasing.

## Not Mistakes, But Applications of the Tester Mindset When You Have a
Medical Condition

So a few thoughts I've had about things that a tester mindset can do for you
when you have a medical condition.

### Don't Be Afraid to Question Things

Only one person has a single-minded focus on your case, and that is you. The
doctors and nurses are treating lots of people. They are professionals and
should be doing all of the right things. Sometimes though, things happen. If
someone is going to give you a treatment you don't think sounds right, ask
about it. Be assertive. It never happened to me that I was going to get the
wrong drug, but people on the forums have.

Remember, as a tester, it is your job to notify people of inconsistencies and
risks. The stakes just aren't usually so high or so personal.

### Communicate

Just as a tester needs to communicate with their team, a patient needs to
communicate with their doctor. The blood work, scans and X-Rays can only tell
them so much. You as the patient need to communicate how you feel and how the
treatments are affecting you. Quality of life is important and sometimes
changes are needed.

### Leverage the Tools Available To You

I have never been given a testing tool that did what I wanted out of the box.
And the MyChart system from Epic, while pretty decent, is no exception. I've
created my own spreadsheets to track the numbers, including the all important
M-Spike. I've hooked up this crazy combination of a symptoms recorder in
[Workflow](https://workflow.is/) triggered by [Launch Center
Pro](http://contrast.co/launch-center-pro/) and that uses Launch Center Pro to
send data. To [Evernote](http://evernote.com) and to a Google spreadsheet via
[IFTTT](http://ifttt.com). So I've built my own dashboard and metrics tools to
track things the way they make sense to me.

Point there, is taking care of testing or tracking your health, leverage the
tools you like and are comfortable with. You don't have to take stock options
and limit yourself there.

## In Conclusion

Learning you have cancer and quickly starting cancer treatment is not the same
thing as testing insurance software or the next big app. In those initial
days, I was focused on the prognosis, which thankfully was a lot better than
the median survival rates that are published (based on older data). And then
absorbing the fact that I was starting chemo in a week, and then learning to
adjust to the ups and downs and of chemotherapy. Unless you are in the medical
field, or have had to help a friend or a loved one though this, it's probably
all new to you. It certainly was new to me. And it's not like we didn't ask a
lot of questions. We asked a ton, but how we were going to measure and track
the treatment wasn't one of them.

So it took a little bit, but now I have a better understanding of what is
going on. I understand more about the myriad of numbers that come from the
blood draws. As this is a new domain to me, I still have tons to learn. I've
got a good team working on things, and I expect to have plenty of time to
learn more.

And some remaining thoughts. First off, I don't intend to make this into a
cancer journey blog. I deeply respect the people who have public blogs for
that purpose, but that is not for me and this is not the place. We might see
some more thoughts about the intersection of testing and my experiences, but I
doubt there will be many. Second, I am doing well. It hasn't been easy, but it
could be a lot worse. Finally, I am immensely grateful for the support my
family, friends, coworkers, neighbors, and employers have given me and
continue to give me. I'm a lucky man.

