---
layout: post
title: How to best use IEx.pry for tesing in Phoenix
categories: posts
date: 2016-01-08 11:00:00
summary: A quick trick to get more out of pry while testing in a Phoenix App.
---

In Rails, one thing that I love doing is to throw a `pry` in a test to make sure
that my test environment is set up the correct way and to help debug while
writing the code to get my tests to pass. The great thing about Phoenix is that
you can similarly set up `pry` to do the same thing.

First, for the file that you want to throw your `IEx.pry` into, add `require IEx` under your `defmodule` line.

{% highlight elixir %}
defmodule Fishbowl.User do
  require IEx
  ...
end
{% endhighlight %}

Then, where you want to enter `pry` enter `IEx.pry`

{% highlight elixir %}
def changeset(model, params \\ :empty) do
  IEx.pry
  model
  ...
{% endhighlight %}

Run your test suite by running `$ iex -S mix test --trace` in your project directory.

`iex -S` will open up Elixir's interactive shell so that your tests will stop
when it hits an `IEx.pry`.


`--trace` will prevent your Elixir interactive shell from timing out after 60 seconds.
