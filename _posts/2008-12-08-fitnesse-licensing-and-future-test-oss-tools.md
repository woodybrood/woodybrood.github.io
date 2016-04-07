---
layout: post
title: "FitNesse Licensing and Future Test OSS Tools"
date: 2008-12-08
---

FitNesse and dbFit are both released under the GPL version 2. I have to say I
find this choice of license frustrating. Don't get me wrong, I love the fact
that this has been put out there to grow in the community. I've had it with
the commercial tools.

The reason I am frustrated is the implications of the GPL on doing more with
your tests. I've worked at or spoken with a number of companies that are
looking at the possibility of delivering tests with their software so that the
customer can run those tests on their installation. This is fine with FitNesse
and other frameworks provided that the application is a web app or something
that is loosely coupled and none of your fixtures reveal anything about
proprietary nature of your product.

My concern is that with FitNesse being released under the GPL, which I am not
an expert on, the fixture code often connects directly with the code under
test. This is fine if you don't sell or distribute the tests along with the
product to the customer. The GPL indicates that all uses within a company can
stay within that company. However once you share it with one customer, you're
on the hook to give all GPL affected code to the world.

Now I know that we're far beyond changing this, but it just seems to me that
the testing community needs to think about this issue in the future. It seems
to me that there is a real future in providing tests and testing tools to
customers along with delivered software. It really goes a long ways to help
the customer understand the product they are receiving. I'm not sure what
license open source testing projects should use in the future, but I think it
needs to be one that considers both the needs of a software vendor and the
needs of the software community.

I am embracing FitNesse now and will continue to do so. It will mean that my
company has to keep our tests to ourselves. These tests will still do a lot of
good. But unfortunately this good will stop just before delivery.

Update 12/8:  
Uncle Bob just posted on the FitNesse Yahoo group that his efforts with SLIM
are intended to eliminate the GPL requirement from FitNesse by removing FIT.
Additionally Gojiko, the author of DBFit is trying to do the same thing, as is
John Roth of PyFit. This is good for everyone who wants to work with their
customers and to deliver value to those customers in the form of tests and
test tools for the systems.

