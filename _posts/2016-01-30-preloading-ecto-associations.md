---
layout: post
title: Preloading Ecto Associations
categories: posts
date: 2016-01-30 11:00:00
summary: Associations are not preloaded by default in Ecto. Instead, you need to explicitly declare when you want your model to preload its associations.
---

Associations are not preloaded by default in Ecto. Instead, you need to
explicitly declare when you want your model to preload its associations.

For example, lets say that we have a calendar app where each appointment has a
location and each location has a city. You will probably want to find the
location and the city of your appointment. This is not 100% straightforward with
Ecto because the location and city do not get preloaded. You will need to
explicitly preload each model.

**To preload the immediate association:**

{% highlight elixir %}
preloaded_appointment = appointment |> Repo.preload(:location)
{% endhighlight %}

This will allow you to do something like `preloaded_appointment.location.name`
because now, the location association is preloaded with the appointment.
However, if you want to access the city that is associated with the location,
you will not be able to yet because the city is not preloaded.

**To preload both associations:**

{% highlight elixir %}
preloaded_appointment = appointment |> Repo.preload(location: :city)
{% endhighlight %}

Now the city is also preloaded into the appointment. You will be able to access
both the location and city from the appointment!
