---
layout: post
title: Writing faster tests with "build stubbed"
categories: posts
date: 2015-06-06 11:00:00
summary: Writing tests are good. Writing slow tests are frustrating for continuous deployment. Using FactoryGirl's build_stubbed is fast!
---

Writing tests are good. Writing slow tests are frustrating for continuous
deployment. Your test suite runs before you can deploy and this means that
deployments can take additional 5-10 minutes because of slow tests. One
of the things that I am most proud of in the past month is dramatically speeding
up our test suite. We went from a full test suite take approximately 8-9 minutes
to our test suite taking about 5 minutes! Thus, the feedback loop of producing
code, checking in code, and deploying code to staging is a lot faster.

The biggest improvement in our test suite came after I read [this
article](https://robots.thoughtbot.com/use-factory-girls-build-stubbed-for-a-fas
ter-test) about [factory_girl](https://github.com/thoughtbot/factory_girl).
The major argument in this article is that if you are building test objects that
do not need to hit the database, you do not need to create the object. Instead,
you can build_stubbed the object, which instantiates and assigns attributes to
an object to make it look like its been persisted. But, since it does not
actually get persisted into the test database, it dramatically speeds up your
tests.

**Example time!**

Using FactoryGirl.create()

{% highlight ruby %}
FactoryGirl.create(:object)
{% endhighlight %}

This takes 0.46449 seconds.

Using FactoryGirl.build_stubbed()

{% highlight ruby %}
FactoryGirl.build_stubbed(:object)
{% endhighlight %}

This takes 0.05353 seconds.

There are times when you will want to create objects in testing, but default to
using build_stubbed, it will greatly reduce your test suite!
