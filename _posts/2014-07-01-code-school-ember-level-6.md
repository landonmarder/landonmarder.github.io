---
layout: post
title: Video Notes for Code School Ember - Level 6
categories: posts
date: 2014-07-01 11:00:00
summary: Level 6 Code School Ember Notes.
---

To see Level 1 of Code School's Ember notes, check [here](http://landonmarder.com/posts/2014/05/23/code-school-ember-level-1/).
To see Level 2, check [here](http://landonmarder.com/posts/2014/06/27/code-school-ember-level-2/).
To see Level 3, check [here](http://landonmarder.com/posts/2014/06/28/code-school-ember-level-3/).
To see Level 4, check [here](http://landonmarder.com/posts/2014/06/29/code-school-ember-level-4/).
To see Level 5, check [here](http://landonmarder.com/posts/2014/06/30/code-school-ember-level-5/).

Components
---
- In the past, we are using the same HTML templating in multiple places
- Can split out parts of your page to reusable components
- Can wrap user interactivity
- Template name for all components must start with components/ and must have a slash (-)
in its name
- Need to send data to our component so it knows what to render -- item=this and then item.title
- Include classNames and tagNames into your component call outside the template
- Components are reusable templates that you can use throughout other templates, need
to include the data that you want in the component when you call for it
- Can change the tagName and classNames per component, so that part can be flexible

Component Objects
---
- Component item-details first looks for the component object App.ItemDetailsComponent.
It is here that we can set properties. Once we set the properties, the Component Object
then renders the Component Template
- We can create properties on the component object just like a controller. hasReviews
updates based on any changes to itemCount
{% highlight javascript %}
App.ItemDetailsComponent = Ember.Component.extend({
  itemCount: Ember.computed.alias('item.reviews.length'),
  hasReviews: function() {
    return this.get('itemCount') > 0;
  }.property('itemCount')
});
{% endhighlight %}
- We can also set attributes of a component template inside the Component Object
instead of in the template. We only want to do this if we want the same tagName and
classNames wherever your Ember Component is being used
{% highlight javascript %}
App.ItemDetailsComponent = Ember.Component.extend({
  tagName: 'li',
  classNames: ['row']
});
{% endhighlight %}

View Objects
---
- View is responsible for encapsulating HTML content, registering and responding to
user-initiated events
- Use view objects to give templates dynamic classes that change based on information
found in our model
- Ember Components extend from Ember View
- Views sit between the Controller and the handlebars template
- Ember View in a Request: Route to Controller to View to Template
- We can define a View to set the proper classes on a template's div
{% highlight javascript %}
App.ItemView = Ember.View.extend({
    classNames: ['row'],
    classNameBindings: ['isTrue']
    isTrue: Ember.computed.alias['controller.isOnSale']
})
{% endhighlight %}
- Can alter properties include tag, class, which template to render
- classNameBindings first checks on property, if its true it will add a new class is-true
to the div

Partials
---
- Use partials when you want to split template and would be overkill to make your
own component for it.
- You do not need to pass anything into a partial because it has all the data from the parent template
- All partials begin with an underscore for the data-template-name
- Partials have same access to controller and model
- Include partial with {{ partial 'name' }}

Using render
---
- Sorting typically happens in the controller
- From template we need to go to a controller to sort, then go to view and template for that template
- You can pass objects into a render, this will become the model -- go look for this controller to
decorate the render

**Main ideas**
---
- Use route for when you have an end point URL, route uses a controller > view > template
- Use render handlebars helper if you need a controller to decorate the model, render
uses a controller > view > template
- Use component if we need to create a highly reusable part that does not need a controller,
only uses the data that we pass into it, component > template
- Partial is reusable template that uses the parent controller (whatever it got included from)
