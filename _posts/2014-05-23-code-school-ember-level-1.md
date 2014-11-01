---
layout: post
title: Video Notes for Code School Ember - Level 1
categories: posts
date: 2014-05-23 11:00:00
summary: Level 1 Code School Ember Notes.
---

I find that I am a better learner when I attack learning through different mediums and through different points of view. In conjunction with reading Ambitious Ember Applications, I am also watching the [Code School](ember.codeschool.com/) videos on Ember. I will be posting my notes from these videos for you (and myself).


Setting Up Ember
---
- Ember depends on jQuery, handlebars, ember, ember-data
- We need a single object to contain all the functionality of our app (namespaced as App), think of it as the root of the app, everything grows from here
{% highlight javascript %}
var App = Ember.Application.create({ });
{% endhighlight %}
- Ember adds a body class and data-ember-extension to your code (so Ember knows what is can control)
- To wrap HTML inside Handlebars template
{% highlight javascript %}
<script type='text/x-handlebars'>
// Put HTML in here
</script>
{% endhighlight %}
- Each Handlebar template is in a unique div
- Handlebars works just like HTML, uses handlebars for templating, the part of the application that people see


Named Templates
---
- Handlebar expressions allow us to insert expressions into out HTML
- Name templates
{% highlight javascript %}
<script type='text/x-handlebars' data-template-name='templateName'>
// Put HTML in here
</script>
{% endhighlight %}
- We need a way to tell our templates where to render on the page
- Handlebar expression outlet gives our code a hint as to where templates should be inserted
- If our application template hits an outlet, it is going to look for a template named index and render that in the place of outlet


The Router
---
- How do we render other templates besides the index template and map it to a URL?
- Ember Router translates a path into a route
- All requests start at the router
{% highlight javascript %}
App.Router.map(function(){
  this.route('routeName');
});
{% endhighlight %}
- Every page on the website is defined by the router. When an outlet is found, the template is loaded into it
- All application is loaded through one file (index.html)
- All templates are sent over at initial load from the client
- At that point it loads up individual templates to load out onto the screen
- The # tells the browser that there is nothing else to load (without the hash will look for another file path)
- To load up a different path from a route, use a JS object
{% highlight javascript %}
App.Router.map(function(){
  this.route('routeName', { path: '/newPathName' });
});
{% endhighlight %}
- Base URL/index route is done automatically (default behavior is to load the default template)
