---
layout: post
title: Video Notes for Code School Ember - Level 3
categories:
- posts
tags:
- ember
- book notes
---

To see Level 1 of Code School's Ember notes, check [here](http://landonmarder.com/posts/2014/05/23/code-school-ember-level-1.html).
To see Level 2, check [here](http://landonmarder.com/posts/2014/06/27/code-school-ember-level-2.html).

Resource Routes
---
- The name of the controller matches the name of the route and template names
- Templates can use properties from their controllers
- Regular routes -- adjectives, verbs, adverbs
- Resource routes are typically nouns (singular or plural)
- Resource routes behave like a normal routes (templates, paths, links), but with additional functionality
{% highlight javascript %}
App.Router.map(function(){
  this.resource('noun');
});
{% endhighlight %}
- Route fetches data and passes it to the Ember Controller, decides on what model to
use for the current route, decides which template to render to the screen
- One Ember Router translates a path into a route. Ember Route provides data for the controller (can be many)
- Routes sits between the Router and the Controller
{% highlight javascript %}
App.ItemsRoute = Ember.Route.extend({
  model: function() {
    return App.MYITEMS;
  }
});
{% endhighlight %}
- Note how routes are Ember.Route, controllers are Ember.Controller
- The model property should return an object or an array (can be pulled from an API)
- Route is responsible for fetching the model. Ember will set this model on the Controller
and that's what'll be used in your Handlebars Template
- To loop in handlebars
{% highlight html %}
{{#each}}
  {{firstAttribute}}
{{/each}}
{% endhighlight %}
- Do not need to specify the model before firstAttribute because template knows which Controller and Route we are coming from

Dynamic Routes
---
- Routes map a model to a controller and a template
- Unique URL to go to a specific product page
- Always start with the Router when thinking of Ember Apps
{% highlight javascript %}
App.Router.map(function(){
  this.resource('items');
  this.resource('item', { path: '/items/:id'} );
});
{% endhighlight %}
- Application will read the :id out of the URL path
- Ember adds helpers to JavaScript base objects including findBy
- Data from the URL is in our singular Item Route
{% highlight javascript %}
App.ItemRoute = Ember.Route.extend({
    model: function(params) {
      return App.MYITEMS.findBy('id', params.id);
    }
})
{% endhighlight %}
- Can create a new template using the route path's name
- Templates first look in the controller for properties, if not found in the controller,
will look in the model for the properties (model is the Route)
- In the items template, you can iterate through the items, using this will pass in the item we want
{% highlight html %}
{{#each}}
  {{#link-to 'item' this}}
    {{itemProperty}}
  {{/link-to}}
{{/each}}
{% endhighlight %}

Nested Routes
---
- Want to put the singular template inside the plural template (think of item and items respectively)
- If your views are nested, your Routes should be nested
{% highlight javascript %}
App.Router.map(function(){
  this.resource('items', function(){
    this.resource('item', { path: '/:id'});
  };
});
{% endhighlight %}
- We don't need /items because its nested
- Calls the item route
- We need to add a new outlet to show where to product template will be rendered inside the products template
- Ember Inspector for Chrome is good for debugging nested routes (routes)
- ItemIndexRoute gets rendered when you go to the items route (so need to create
  a template for items/index for when no item is selected)

**Important vocab**
---
**- Router -- define routes our application accepts**
**- Route -- Responsible for getting data from external resources (think of as the fetcher)**
**- Controller -- Decorates the model, provides property values**
