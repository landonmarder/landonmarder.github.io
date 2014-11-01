---
layout: post
title: The Well Grounded Rubyist
date: 2013-11-16 10:00:00
categories: posts
summary: The biggest piece of advice before starting Launch Academy (besides mentally/physically preparing myself for 12+ hour days of learning to code for 10 weeks) is to get as comfortable as I can get with Ruby.
---

The biggest piece of advice before starting Launch Academy (besides mentally/physically preparing myself for 12+ hour days of learning to code for 10 weeks) is to get as comfortable as I can get with Ruby. I had previously worked though the [Codecademy Ruby Track](http://www.codecademy.com/tracks/ruby) and a couple of other online tutorials, which served as a great foundation for reading [The Well-Grounded Rubyist](http://www.amazon.com/The-Well-Grounded-Rubyist-David-Black/dp/1933988657/ref=sr_1_1?ie=UTF8&qid=1382987281&sr=8-1&keywords=the+well+grounded+rubyist). I really liked reading [The Well-Grounded Rubyist](http://www.amazon.com/The-Well-Grounded-Rubyist-David-Black/dp/1933988657/ref=sr_1_1?ie=UTF8&qid=1382987281&sr=8-1&keywords=the+well+grounded+rubyist) as the last part of my prework because it gave awesome, detailed explanations of the Ruby language and then supplemented its explanations with nicely labelled code examples. Moreover, it emphasized becoming literate with Ruby.

**Big Picture Takeaways:**

- Black kept on harking back on the philosophy that everything in Ruby is an object. This is especially helpful when iterating through an array or a hash because each part of an array or hash is an object. For example, I found myself using “obj” in place of “n” or “i” inside a code block to make sure that I was centering myself around the “everything is an object” mantra:

{% highlight ruby %}
arr = [1, 2, 3, 4, 5]
results = arr.map { |obj| puts obj * 100 }
{% endhighlight %}

This helps me realize that in the code block, each object in the array is being looked at one at a time and multiplied by 100.

- It is really interesting how Black emphasizes _becoming literate in Ruby_. This is similar to TFA emphasizing boosting student reading fluency as a way to close the achievement gap because students that are struggling academically often have low fluency rates. The thinking is that students with low fluency rates spend a lot of their brain power pronouncing words (either aloud or in their heads) instead of spending that brain power on higher level reading comprehension. Once you have fluency mastered, your brain power is freed up for higher level comprehension. I feel like I am currently in that position with Ruby because I am using a large percentage of my brain power on the nuts and bolts of the code instead of being able to use that brain power on higher level code comprehension. The way to solve this problem is to keep on practicing, studying, and writing code.

**My priority for these next two weeks before I start Launch Academy is to strengthen my Ruby fluency.**
