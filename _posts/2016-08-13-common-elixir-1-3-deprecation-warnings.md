---
layout: post
title: Common Elixir 1.3 Deprecation Warning Fix
categories: posts
date: 2016-08-13 11:00:00
summary: How to fix a common Elixir 1.3 deprecation warning
---

[Elixir 1.3](http://elixir-lang.org/blog/2016/06/21/elixir-v1-3-0-released/)
was released at the end of June. Personally and professionally, the upgrade went
smoothly without any hiccups. One thing that I did realize was that upgrading caused
deprecation warnings to pop up when running my tests or starting my server. The deprecation
warning looked a little like this:

{% highlight bash %}
warning: the variable "my_params" is unsafe as it has been set inside a case/cond/receive/if/&&/||. Please explicitly return the variable value instead. For example:

    case int do
      1 -> atom = :one
      2 -> atom = :two
    end

should be written as

    atom =
      case int do
        1 -> :one
        2 -> :two
      end
{% endhighlight %}

The awesome part about this deprecation warning is that it is really straight
forward about what I should do to fix the warnings. Moreover, reading the release
notes gives a great background about why this deprecation warning exists.

The code that I needed to refactor that caused this warning looked a little bit like this:

{% highlight elixir %}
if links = my_params["links"] do
  foreign_keys = %{
    "foreign_key_a_id" => links["foreign_key_a"],
  }

  my_params = my_params |> Map.delete("links") |> Map.merge(foreign_keys)
end
{% endhighlight %}

The code looks at the JSON from my request and sanitizes it so that it can be
nicely inserted into the database. The problem with this code (and why Elixir 1.3 has
a deprecation warning about it), is that it is implicitly changing the value of `my_params`
inside of the if block and the `my_params` variable is being accessed in an outer scope. This
is not very functional and can lead to unintended consequences.

To fix this, Elixir prefers that I need to explicitly return the variable that is changing.

The fix code is below:

{% highlight elixir %}
my_params =
  case my_params["links"] do
    nil ->
      my_params
    links ->
      foreign_keys = %{
        "foreign_key_a_id" => links["foreign_key_a"],
      }

      my_params |> Map.delete("links") |> Map.merge(foreign_keys)
  end
{% endhighlight %}

Now, I am explicitly setting the variable `my_params` that I am changing inside of
the if block. If was I was initially checking for doesn't exist (and is `nil`), it
will return the initial value. If the `my_params["links"]` does exist, it will return `links`
and I can access `my_params["links"]` by accessing `links` and performing the same
code that I had in the pre-refactored version of the code.

The change makes updating the variable safer and, in my opinion, makes the code
more readable.
