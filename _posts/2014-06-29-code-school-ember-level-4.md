---
layout: post
title: Video Notes for Code School Ember - Level 4
categories:
- posts
tags:
- ember
- book notes
---

To see Level 1 of Code School's Ember notes, check [here](http://landonmarder.com/posts/2014/05/23/code-school-ember-level-1.html).
To see Level 2, check [here](http://landonmarder.com/posts/2014/06/27/code-school-ember-level-2.html).
To see Level 3, check [here](http://landonmarder.com/posts/2014/06/28/code-school-ember-level-3.html).

Models and Ember Data
---
- Previously we loaded in data from a JavaScript array
- We can fetch data from a server
- Ember Model -- a class that defines the properties and behavior of the data that
you present to the user
- Every Route can have a model
- Route tells the controller what model to use, that gets rendered to the template
- Can think of a model as a class (in programming terms)
- Models are typically nouns
- Our Ember Model needs to know what attributes it contains
{% highlight javascript %}
App.Item = DS.Model.extend({
  title: DS.attr('string'),
  price: DS.attr('number'),
  isReal: DS.attr('boolean')
})
{% endhighlight %}
- Good practice to define property types, but not required
- Ember Data makes it easy to use Models
- Ember Data has different adapters
{% highlight javascript %}
// Default, communicate with an HTTP server using JSON
App.ApplicationAdapter = DS.RESTAdapter.extend();

// Load records from memory (hardcoded data)
App.ApplicationAdapter = DS.FixtureAdapter.extend();

// Fixture array
App.Item.FIXTURES
{% endhighlight %}
- All models in Ember need to have a unique ID
- Ember Data has a store -- central repository for records in your application, available
in routes and controllers (cache storage of all your records)
- Controller is where you are getting your data from
- Need to update routes to get fixture data out of the store (where all the data is held)
{% highlight javascript %}
App.ItemsRoute = Ember.Route.extend({
    model: function() {
      return this.store.findAll('item');
    }
})
{% endhighlight %}
- Fixtures need to be defined after the model you're providing the fixture data for
- To fetch particular item from the store (this is the default in Ember, so don't need to explicitly specify)
{% highlight javascript %}
App.ItemRoute = Ember.Route.extend({
    model: function(params) {
      return this.store.find('item', params.item_id);
    }
})
{% endhighlight %}

Related Models
---
- We've created Ember models to use Ember data, revised route to make sure it fetched/pulled
the appropriate data from the Ember store
- How do we express relationships between models? Has-Many Belongs To Relationship
{% highlight javascript %}
App.Item = Ember.Model.extend({
  title: DS.attr('string')
  secondaryItems: DS.hasMany('secondaryItem', { async: true })
});

App.SecondaryItem = Ember.Model.extend({
  body: DS.attr('string'),
  item: DS.belongsTo('item', { async: true} )
});
{% endhighlight %}
- Async true allows for lazy loading (will seconday records at same time)
- Show in fixture the has-many belongs to relationship
{% highlight javascript %}
App.Item.FIXTURES = [
  {
    id: 101,
    secondaryItem: [1, 2, 3]
    title: 'First post'
  }
]

App.SecondaryItem.FIXTURES = [
  {
    id: 1
    body: 'This is the body'
    item: 101
  }
]
{% endhighlight %}
- Note that the Item Fixture has an array of Secondary Items because has many relationship
- Fixtures need to map both sides of the relationship, using a backend will do it for us
- To make RESTFUL server calls
{% highlight javascript %}
App.ApplicationAdapter = DS.RESTAdapter.extend();
{% endhighlight %}
- Attempts to do a server request to get the JSON data
- Can create a server that has a database and have JSON be like your fixtures
- Keeps everything on the client side, access storage for records
