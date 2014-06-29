---
layout: post
title: Video Notes for Code School Ember - Level 5
categories:
- posts
tags:
- ember
- book notes
---

To see Level 1 of Code School's Ember notes, check [here](http://landonmarder.com/posts/2014/05/23/code-school-ember-level-1.html).
To see Level 2, check [here](http://landonmarder.com/posts/2014/06/27/code-school-ember-level-2.html).
To see Level 3, check [here](http://landonmarder.com/posts/2014/06/28/code-school-ember-level-3.html).
To see Level 4, check [here](http://landonmarder.com/posts/2014/06/29/code-school-ember-level-4.html).

Array Controllers
---
- Two different ways to sort our model
- Leave it up to the server to sort the data. Goes to the store, tells the adapter and call
out to our server that we want to order by title in our JSON array
{% highlight javascript %}
App.ItemsRoute = Ember.Route.extend({
    model: function() {
      return this.store.find('item', { order: 'title'});
    }
});
{% endhighlight %}
- Can have the client (browser) do the sorting. We will have to sort in the controller.
Controller is where we can decorate the application data for the template
- Ember.ObjectController helps you decorate a single object
- Ember.ArrayController helps you decorate an array of objects. If our model is an array,
we should be using an ArrayController
- Array Controller has helper methods like sortProperties
{% highlight javascript %}
App.ItemsController = Ember.ArrayController.extend({
    sortProperties: ['title']
});
{% endhighlight %}
- If our model is an array, change the controller to an ArrayController
- The model will first look for a property called 'length' in the ArrayController.
If it's not there, it will delegate to the model and call 'length' on the model (model.length)
- property('length') keeps a watch on the 'length' property if it changes no matter where you are
{% highlight javascript %}
App.IndexController = Ember.ArrayController.extend({
  itemsCount: function() {
    return this.get('length');
  }.property('length')
});
{% endhighlight %}
- Other way to write the above (and easier way)
{% highlight javascript %}
App.IndexController = Ember.ArrayController.extend({
  itemsCount: Ember.computed.alias('length')
});
{% endhighlight %}
- Can also use computed alias's in object controllers for built in methods
{% highlight javascript %}
App.IndexController = Ember.ObjectController.extend({
  titleName: Ember.computed.alias('title')
});
{% endhighlight %}
- Think of the Route as what data you send to the page (you call a model property)
- Think of the Controller as what properties you can call on the data in your template
- Think of the Model as how you set up your data objects

Computed Properties
---
- Controller decorates the model, put all things you want to call/decorate the data from
the template into the controller
- Can call the filterBy method to filter from an Array Controller. If it returns true,
it'll keep that property in the array that isTrue is true. Second argument is default
true so you can ignore it. You can use other Plain JavaScript methods in addition to
Ember methods
- property('@each.isTrue') -- will update template if anything changes across the template
{% highlight javascript %}
App.IndexController = Ember.ArrayController.extend({
  filterTruth: function() {
    return this.filterBy('isTrue', true).slice(0,3);
  }.property('@each.isTrue')
});
{% endhighlight %}
- In your controllers, any property that is a function needs a .property() at the end
- Example of filtering for all products less than 500, updating whenever prices change
{% highlight javascript %}
App.ProductsIndexController = Ember.ArrayController.extend({
  deals: function() {
    return this.filter(function(product) {
    	return product.get('price') < 500;
    }
  }.property('@each.price')
});
{% endhighlight %}
- Example of showing the template of the filter
{% highlight html %}
{{#each deals}}
  {{title}} - {{price}}
{{/each}}
{% endhighlight %}


Nested Routes with Controllers
---
- In the Router, nouns are resources, adjectives are route
- Route is where we specify our model
- Need a top level route name on the route. Top level parent route tells which model
you are using
- For a nested route call modelFor to return the model from the given route then
filterBy to filter the model down to only get the data that you want
{% highlight javascript %}
App.ItemsFilterRoute = Ember.Route.extend({
  model: function() {
    return this.modelFor('items').filterBy('isTrue');
  }
})
{% endhighlight %}
- Can link to nested route with 'items.filter' in a link-to template
- Template name will be item/filter
- ItemsIndexController defaults to all items, so you can just do {{#each}} in the template
