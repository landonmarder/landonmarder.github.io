---
layout: post
title: Introduction to the Classroom Flipper
categories:
- ruby
- rails
- launchacademy
- edtech
---

My breakable toy is officially complete!* For my breakable toy, I created a way for teachers to assign a video and an assessment for homework and then have student analytics from the assessment to better inform instructional decisions in the classroom to make class time more purposeful. [Check it out](http://classroom-flipper.herokuapp.com/) or check out the [code](https://github.com/landonmarder/classroom\\_flipper).

<iframe frameborder="0" height="400" src="//www.youtube.com/embed/HuzB7WpOtPY" width="100%"></iframe>

To reflect on the learning experience of building something from scratch, here are five things I learned:

1. **Have a good mock up/wireframe** of what the final product is going to look like. When I wrote tests, I realized that I had a 70-80% idea of what my pages were going to look like, so I needed to break from coding to design again. If design was done at the beginning, I would be able to let my design drive my coding.
2. **Outside in testing is awesome and efficient** . Although it is tempting to cowboy code and throw stuff together, having the diligence to always write tests first made refactoring my code a lot easier. For example, I first wrote a query that looped through my database multiple times. This got my tests to pass and got me the result I wanted, but it was hitting my database way too much for good performance. Because I had my tests written, it was not too overwhelming to refactor my code to turn my multiple SQL querying into one SQL query.
3. **Reference your ER diagrams** . My classroom flipper app had 8 models with each model having different relationships with each other. The times I got most stuck while working on my breakable toy was when I was trying to navigate the relationship between my models without my ER diagram. Using my ER diagrams helped ground me to what I needed to do.
4. Going off that, the I felt **most productive when I broke down my tasks into actionable steps** . Saying something like I need to create the ability for a student to take an assignment is daunting because there are many pieces to a student taking an assignment. Breaking this down into steps that I was familiar with (ex. an assignment is a submission, I need to first have the student navigate to a new submission page) allowed me to tackle more complex challenges.\n5. **Debugger and pry are your friends** . One of the things that was confusing at first was what params I was sending where and what information I had available to me. The best way to answer those questions were to go into pry or use the debugger and play around with what I had.

* Complete for now, there are tons of features that I want to add on, but I found a good stopping point for version 1.0.
