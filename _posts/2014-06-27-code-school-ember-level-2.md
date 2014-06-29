---
layout: post
title: Video Notes for Code School Ember - Level 2
categories:
- posts
tags:
- ember
- book notes
---

To see Level 1 of Code School's Ember notes, check [here](http://landonmarder.com/posts/2014/05/23/code-school-ember-level-1/).

**Need to go back in and show handlebars and pound signs on markdown**

Handlebar Helpers
---
- Router is in charge of mapping a path to an Ember Route
- Bad practice to hardcode your links; use your routes to dynamically retrieve your URLs
- Use Handlebars link-to helper, use the route name to look up the path
{% highlight html %}
{{#link-to 'index' class='custom-class'}}Click here for home!{{/link-to}}
{% endhighlight %}
- Ember automatically gives you an active class (if link-to helper is the same as the link you are visiting)
- How to change the HTML element -- link-to helper has a tagName element (default is a)
{% highlight javascript %}
{{#link-to 'index' tagName='li'}}Click here for home!{{/link-to}}
{% endhighlight %}
- To change the route path
{% highlight javascript %}
this.route('credits', { path: '/thanks' });
{% endhighlight %}

Controller Basics
---
- Map paths to route names; link to other pages using route names, not paths
- How do we print out dynamic data in our template?
- Ember Controller -- where templates look to find the value of a property,
decorate your application's data for the template, contains information that is not saved to a server
- Controller provides the properties for the templates. The handlebar looks for the controller of that
template and then gets the property inside of the template from there
{% highlight javascript %}
// For the blank template rendered by the blank route
App.BlankController = Ember.Controller.extend({
  propertyName: value
});
{% endhighlight %}
- Every route has a default controller, Ember automatically creates a controller behind the scenes
- Only need to write out the controller if you need to define properties inside the controller
- Binding with Metamorph -- Ember uses to keep tabs on properties within your template so it
can potentially update it later (called binding!)
- Properties that are attributes need to be binded differently with the bind attribute helper
{% highlight html %}
<img {{bind-attr src='logo'}} alt='Logo' />
{% endhighlight %}
- To call a JavaScript function within the controller, you set the property equal to a function.
Need to tell Ember that this is a property and to call it
{% highlight javascript %}
// For the blank template rendered by the blank route
App.BlankController = Ember.Controller.extend({
  time: function() {
    return (new Date()).toDateString()
  }.property()
});
{% endhighlight %}
