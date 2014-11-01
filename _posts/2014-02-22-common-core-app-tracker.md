---
layout: post
title: Common Core Mastery Tracker PhoneGap App
categories: posts
date: 2014-02-22 11:00:00
summary: I made my first iPhone App this past week. It is an app for 6th grade students to log in and track their mastery on 6th grade math standards. Because it pulls the standards from the Common Core API, any 6th grade math classroom that is aligned with the Common Core can use it.
---

I made my first iPhone App this past week. It is an app for 6th grade students to log in and track their mastery on 6th grade math standards. Because it pulls the standards from the Common Core API, any 6th grade math classroom that is aligned with the Common Core can use it. The high level overview of the app is that a student logs in and sees a list of all 6th grade math standards. The student can use the search box to find the standard he/she is using or can scroll down to find it. Once the student finds the standard he/she is learning, they can swipe the standard to enter in his/her mastery. Once you enter your mastery, you are taken to your completed standards page where you can see your results for past standards (color coded based on score range) and your overall mastery.

Here is a YouTube demo of the app (couldn't pull the trigger on an Apple developer account just yet to put it live):

<iframe frameborder="0" height="400" src="//www.youtube.com/embed/YZ_lPPVq5Jc" width="420"></iframe>

**Pros:**

- Got to play around and become more familiar with making an app that was not Rails. This gives me a better understanding of HTTP requests, strengthens my JavaScript and jQuery chops, and allows me to increase what I know how to build with a mobile app.
- Additionally, used Apigee for the backend. Apigee is a NoSQL database so I got to play around with that too and pick up a tool I didn't previously have.
- Feels good to create an app from scratch that I would have loved to use as a teacher. Students were more invested in learning if they had a way to track their learning outcomes, but having students do this with pencil and paper was inefficient.

**Cons:**

- Scope of the app is limited. Right now you only have 6th grade math standards to track.
- Standards pulled from the Common Core API are very wordy, making the app look very wordy. I debated options to make it less wordy and cleaner looking, such as rewriting the standards or cutting the standards off after about 80 characters (approximately 2 lines on an iPhone) but realized both had their drawbacks. Cutting the standards off after two lines sometimes made it hard to differentiate standards and rewriting the standards would have made the standards list static and less flexible to adjust in the future.
