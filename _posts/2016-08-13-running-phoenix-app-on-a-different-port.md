---
layout: post
title: Starting your Phoenix App on a Different Port in Development
categories: posts
date: 2016-08-13 11:10:00
summary: How to change the port of your Phoenix App in development in case you want to have multiple Phoenix Apps running locally
---

I've recently run into the issue where I want to run two different Phoenix apps
at the same time on my local machine. I wanted to run one app on `port 4000` and
another app on `port 4001`.

I thought that I was going to be able to do something like:

{% highlight bash %}
$ mix phoenix.server -port 4001

[info] Running MyApp.Endpoint with Cowboy
using http on port 4000
{% endhighlight %}

That didn't work, so I [dove into the docs](https://hexdocs.pm/phoenix/Mix.Tasks.Phoenix.Server.html)
to see what I could do with `mix phoenix.server`. Unfortunately, there are no command-line
arguments for passing in the port.

Doing more Internet research, I saw a suggestion by Chris McCord about how to override
the default port of 4000 on a Phoenix App [here](https://github.com/phoenixframework/phoenix/issues/962).

There are two solutions to override the default port of 4000. The solution you pick
depends on what you need.

**Solution 1: Replace the hardcoded port in config/dev.exs with a new port number**

Looking at a sample app, port is [hardcoded as 4000](https://github.com/phoenix-examples/hello_phoenix/blob/master/config/dev.exs#L10).
You can change this to `4001` and running `mix phoenix.server` will start your app on `port 4001`
everytime.


{% highlight bash %}
$ mix phoenix.server

[info] Running MyApp.Endpoint with Cowboy
using http on port 4001
{% endhighlight %}

This solution works if you know you want to run a specific app on a specific port
every single time and keeps your starting command neat.

**Solution 2: Dynamically override the port in config/dev.exs with an environment variable**

To take this one step further, you can use an environment variable to set the
port. To do this, you can replace [4000 here](https://github.com/phoenix-examples/hello_phoenix/blob/master/config/dev.exs#L10) like
this:

{% highlight elixir %}
http: [port: System.get_env("PORT") || 4000]
{% endhighlight %}

And then, when running your Phoenix server, you can do the following:

{% highlight bash %}
$ PORT=4001 mix phoenix.server
[info] Running MyApp.Endpoint with Cowboy
using http on port 4001
{% endhighlight %}

Phoenix will now use the environment variable port number you pass in to run your
app and will default to 4000 if you do not pass an environment variable in. This makes
solution 2 more flexible than solution 1.
