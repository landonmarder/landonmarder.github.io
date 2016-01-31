---
layout: post
title: Elixir's Pattern Matching and ExUnit
categories: posts
date: 2016-01-31 11:00:00
summary: Pattern matching with Elixir creates surprisingly unsurprising test results.
---

While writing tests for a Phoenix app, I found something very strange.
Look at the code below (contrived tests for this example):

{% highlight elixir %}
test "the solution to 2 + 2" do
  assert 2 + 2 == 4
end

test "the solution to 2 + 2" do
  assert 2 + 2 != 4
end
{% endhighlight %}

Running `$ mix test` in my terminal, I got 2 tests, 0 failures. Strange! Did I
write my assertions correctly? Yes I did, but I should be getting 2 tests, 1
failure. Why am I getting them both to pass?

This is Elixir's pattern matching at work! Looking at [ExUnit.Case
Module's code](https://github.com/elixir-lang/elixir/blob/v1.2.2/lib/ex_unit/lib/ex_unit/case.ex#L218), we define a `test` with a string. In the examples above, both
test cases have the same string. Therefore, we will never match the second
clause because the strings are the same. Because the first clause gets matched,
and the first test passes, both tests will pass.

When I further examine the output of my tests, I can confirm this because I see
that I get the warning `test/models/user_group_test.exs:23: warning: this clause
cannot match because a previous clause at line 19 always matches`.

To get both tests to work as expected, I will need to make my test strings
unique.

{% highlight elixir %}
test "the solution to 2 + 2" do
  assert 2 + 2 == 4
end

test "the solution to 2 + 2 is not this" do
  assert 2 + 2 != 4
end
{% endhighlight %}

2 tests, 1 failure!
