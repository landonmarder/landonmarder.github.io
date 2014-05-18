---
layout: post
title: Book Notes for Ambitious Ember Application
categories:
- posts
tags:
- ember
- book notes
---

[Ambitious Ember Applications](https://leanpub.com/emberjs_applications)
===
By [Ruslan Yakhyaev](https://twitter.com/ryakh)

A challenge to reading a technical book is to figure out where to best put your notes. In the past, I have used physical notebooks or Evernote. I figured I would try putting my notes on here as an experiment.

Chapter 1
---
- Download the [Ember Starter Kit](http://www.emberjs.com) for quick start

Chapter 2
---
- Template without an ID is an application template, will be rendered on each page by default
- Ember will always redner other templates into {{outlet}} expression
- Whenever you request a route by changing the URL, Ember will render the template that belongs to the route requested
- **Debugging trick**, set LOG_TRANSITIONS to true to log a message everytime the URL changes
- {{link-to}} block expression accepts different arguments, Handlebars will take the link-to block and parse it into regular <a></a> tag
- Ember gives you .active for free, currently active link gets the .active class
- **Convention over configuration**, Ember will match the URL to the template's name and render the needed content


Chapter 3
---
