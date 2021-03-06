---
layout: post
title: Book Notes for Ambitious Ember Applications
categories: posts
date: 2014-05-17 11:00:00
summary: A challenge to reading a technical book is to figure out where to best put your notes. In the past, I have used physical notebooks or Evernote. I figured I would try putting my notes on here as an experiment to see if it improves my comprehension and learning.
---

A challenge to reading a technical book is to figure out where to best put your notes. In the past, I have used physical notebooks or Evernote. I figured I would try putting my notes on here as an experiment to see if it improves my comprehension and learning.

[Ambitious Ember Applications](https://leanpub.com/emberjs_applications) By [Ruslan Yakhyaev](https://twitter.com/ryakh).

Chapter 1
---
- Download the [Ember Starter Kit](http://www.emberjs.com) for quick start

Chapter 2: First Template, First Routes
---
- Template without an ID is an application template, will be rendered on each page by default
- Ember will always redner other templates into {{outlet}} expression
- Whenever you request a route by changing the URL, Ember will render the template that belongs to the route requested
- **Debugging trick**, set LOG_TRANSITIONS to true to log a message everytime the URL changes
- link-to block expression accepts different arguments, Handlebars will take the link-to block and parse it into regular <a></a> tag
- Ember gives you .active for free, currently active link gets the .active class
- **Convention over configuration**, Ember will match the URL to the template's name and render the needed content


Chapter 3: Controllers
---
- Controller in Ember is responsible for decorating data before presenting it to the user
 (middleman sitting between your data source and your user, translating language of one into another)
 - Properties written inside a controller will be available inside our templates
 - Ember uses metamorph script tags to wrap properties we set in our controller so it can easily identify them and update if needed (so cannot use straight-up properties within a HTML tag because it will be HTML within HTML -- need to use bound attributes)
 - To explicitly tell our controller that a property is indeed a property and can be called inside our template we need to add .property() method at the end of the function
{% highlight javascript %}
App.IndexController = Ember.Controller.extend({
  currentTime: function() {
    return(new Date);
  }.property()
});
{% endhighlight %}
- There are two controller types: ArrayController and ObjectController
- ObjectController: used to represent a single model
- ArrayController: used to represent an array of models (consist of many ObjectController's for each model in array)

Chapter 4: Routing, View Tree and Naming Conventions
---
- We have an application. An application has a router which receives requests made by user (represented by URLs and typed into the browser address bar) and then the router sends users requests to the controller. The controller decorates our data and sends teh data to the template. The template displays data to the user.
- The route gives the data to the controller. Route objects are responsible for loading data from defined data storage and providing data to the controller.
- Ember expects model property of a route object to return either an object or an array
- Iterating through objects passed in through the ArrayController:
{% highlight javascript %}
{{#each}}
  <li>{{attributeOne}} - {{attributeTwo}}</li>
{{/each}}
{% endhighlight %}
- Ember objects (templates, controllers, router, routes) can be linked to one another by setting the correct names

Chapter 5: First Tests
---
- The runner.js file intercepts the index.html?test request and sets everything up for testing our application and then it loads test.js which contains the actual tests
- 'Wait until our application is idle' === all promises and other async behaviors resolves
- Ember Starter Kit uses QUnit
- Run test by adding ?test to URL

Chapter 6: Working with Real Data
---
- Ember gives an ability to define our application store (store will contain all of the data for records loaded from the external source)
- Inside the store we need to provide an adapter which will tell Ember where from the data should be loaded from
- Define the store using the Fixture adapter (fixtures represent data hardcoded into separate files -- similar to JSON response)
- Fixtures should each have a unique ID, does not have to be in a sequence of any kind
- Once we tell Ember where to fetch data from, it will do so and store the data in the store. Later we can fetch the data from the store inside our routes
- Need to define the properties and behavior of the data before we can present it to the user (we do this by creating a model object)
- Need to define the model property inside the Route
{% highlight javascript %}
App.IndexRoute = Ember.Route.extend({
  model: function() {
    return this.store.findAll('model');
  }
});
{% endhighlight %}
- Dynamic segments is a portion of a path that starts with a colon(:) and is followed by an identifier
- Resource is a thing (a model) and a route is something that you do with the model
{% highlight javascript %}
this.resource('model', { path: '/:model_id' })
{% endhighlight %}
- Ember's routes make use of promises (objects that can potentially be resolved into any value, can be fulfilled or rejected)

Chapter 7: Relating Models
===
- Ember Data includes several built-in relationshpi types to help you define how your models relate to each other
- Think of Fixtures as the hardcoded data and Models as how the data is structured (schema)
- Async true -- tells Ember to load data asynchronously from teh server
- Ember offers three types of relations out of the box: one-to-one, one-to-many, many-to-many
- Need to have both fixtures and models referenced in script tags (watch out for spelling mistakes)

Chapter 8: Managing the Authentication
===
- The server will tell you what kind of info it needs to authenticate and requests you to provide that informatoin. You send info back to the server and the server will provide you with authentication token which you can later use to tell the server you've been authenticated
- We need a place to store the authentication token and in certain parts of our application check to see if a user has the unique authentication string
- Keep a value stored in localStorage to determine whether a user is logged in or not
- **Uncaught Error: Nothing handled the action 'action'** - means we need to implement an action inside the proper controller
- To access a property from other controllers available in the current controller, we need to provide a 'need' property inside the controller
{% highlight javascript %}
App.SecondController = Ember.Controller.extend({
  needs: ['firstController']
});
{% endhighlight %}
- When you are telling a controller it needs another controller, you need to provide the other controller's name downcased with the 'Controller' part omitted
- Can put actions in either controller, but think of which template it will be used most in and put the action in there
- Ember allows us to create properties that are bound to other values inside our application. Once the bound value changes, Ember will automatically update all properties that depend on it
- Ember can't track external objects and other JS variables set outside of Ember, so need to store new Ember variables for external objects like localStorage
- Can change scope of a variable globally in Ember
{% highlight javascript %}
App.set('globalVariable', 'newValue');
{% endhighlight %}
- Can have functions changed 'live' with the new global Ember variable being changed by passing the global variable into the parameter of property at the end of the function
{% highlight javascript %}
App.ApplicationController = Ember.extend({
  importantFunction: function(){
    return globalVariable;
  }.property('App.globalVariable')
})
{% endhighlight %}

Chapter 9: Asking Our First Question
---
- Don't rely on Ember to manage user permissions, (Ember is client side so user will be able to download all the code), be sure to check for permission
- In a form, tell Ember to create a property name question on the controller and keep it in sync with the value entered into the text area
{% highlight html %}
{{textarea id="question" value=question}}
{% endhighlight %}
- signedInUser method returns a promise, not an actual User record, so need to wait for the promise to be resolved
{% highlight javascript %}
this.get('controllers.application.signedInUser').then(function(user){
  // Do stuff with the user record
})
{% endhighlight %}
- Need to clean the form after you save the record (set form fields to '')
- To get provide the controller with data:
{% highlight javascript %}
App.AskQuestionController = Ember.ArrayController.extend({
  model: function() {
  return this.store.findAll('question');
  // return this.store.findAll('question', { order: 'date' });
  }
})
{% endhighlight %}
- Can sort your data on the client side or the server side
- In our controllers, we create the functions that we can call using handlebars. Example, a controller has a latestQuestions function in it, can iterate through the latestQuestions using handlebars
- To ensure that the context refers to all objects (including ones that are being created on the fly)
{% highlight javascript %}
}.property(@each)
{% endhighlight %}
- Redirect route
{% highlight javascript %}
controller.transitionToRoute('question', question);
{% endhighlight %}

Chapter 10: Answering Questions
---
- Flow of adding a record: create a new record locally, store the record in the store (forcing the synchronization of the store from the server), clean the record form
- Mixin is an Ember Class which allows us to add it's properties into other classes
{% highlight javascript %}
App.DescribeMixin = Ember.mixin.create({
  needs: ['application'],

  functionName: function(object){
    // write function
  }
});

App.NameController = Ember.ArrayController.extend({
  App.DescribeMixin, {
   // rest of Controller
  }
});
{% endhighlight %}
- With handlebars, can do each object, and have an else case when there is no object
 (think of questions with and without answers below it)

Chapter 11: Cleaning Templates Up
---
- To define a component in Ember, we need two things: define the template of the component, define the code for the component (Component Object)
- Component templates should include a hyphen in its name
{% highlight html %}
<script type="text/x-handlebars" id="components/question-preview">
<!-- write component in here -->
</script>
{% endhighlight %}
- Components are unaware of it's surroundings and if you want it to have any of the properties available in your template to be available in your component you need to explicitly pass them in (and update component template)
- To display a component, need to call its id inside of handlebars. Also need to pass in the data you want
{% highlight html %}
// how you call the template
{{template-name templateObject=this tagName='li'}}
{% endhighlight %}
- In the example above, you can use templateObject inside your template to call that data
- You can create components for each of your models where you can give specific methods that the models can call (for component templates)
- To render a partial, you only need a template. Partial templates need to start with an underscore
- Partial will just insert one template into another. Inside partials have full access to everything the parent partial has. But, are pretty dumb, they only mimic the functionality of the controller template they are inserted into
- Ember.Components is basically a subclass of Ember.View -- unlike view though, components are completely isolated
- Unlike components, actions triggered in the view are handled by the controller of the template the view was inserted into
- Stop @ 11.3
