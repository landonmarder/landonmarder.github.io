---
layout: post
title: Video Notes for Code School Ember - Level 7 (not finished yet)
categories:
- posts
tags:
- ember
- book notes
---

To see Level 1 of Code School's Ember notes, check [here](http://landonmarder.com/posts/2014/05/23/code-school-ember-level-1/).
To see Level 2, check [here](http://landonmarder.com/posts/2014/06/27/code-school-ember-level-2/).
To see Level 3, check [here](http://landonmarder.com/posts/2014/06/28/code-school-ember-level-3/).
To see Level 4, check [here](http://landonmarder.com/posts/2014/06/29/code-school-ember-level-4/).
To see Level 5, check [here](http://landonmarder.com/posts/2014/06/30/code-school-ember-level-5/).
To see Level 6, check [here](http://landonmarder.com/posts/2014/06/30/code-school-ember-level-6/).

Controller Actions
---
- Render calls a new controller, view, template
- valueBinding binds the value of a text area to a text property inside the controller
- Text property gets updated and then the template gets updated
- Action helper calls function in the controller, we then need to save the item into the model
{% highlight javascript %}
App.ItemController = Ember.ObjectController.extend({
    text: '',
    actions: {
      createItem: function() {
        var item = this.store.createRecord('review',{
            text: this.get('text'),
            product: this.get('model'),
            reviewedAt: new Date()
        });
        var controller = this;
        item.save().then(function(item){
          controller.set('text', '');
          controller.get('model.items').addObject(item);
        })

      }
    }
})
{% endhighlight %}
- Steps to save an object from the controller: build a new object, save the object, clear out the variables
- Need to create the variable controller to reference the controller in the save callback
- Saving an object returns a promise
