---
layout: post
title: Fantasy Baseball and Ruby with Kimono Labs API
categories: posts
date: 2014-03-16 11:00:00
summary: It seems silly, but one of the things I really wanted to do once I learned to program was to use it to improve my fantasy sports teams. I play fantasy baseball, football, and basketball with 12+ of my childhood friends, but I am historically in the bottom of the league.
---

It seems silly, but one of the things I really wanted to do once I learned to program was to use it to improve my fantasy sports teams. I play fantasy baseball, football, and basketball with 12+ of my childhood friends, but I am historically in the bottom of the league.

Before every draft, I spend an afternoon manually inputting projections from ESPN into an excel spreadsheet and then doing a simple excel formula to find out how valuable a player is given my league's scoring system. The process of manually inputting projections takes too long to do it more than once and I usually have time to do this 2-3 weeks before my draft. As a result of doing my projections so far out before the draft, my projections don't match what is currently happening because players get injured or lose/gain playing time during the preseason.

**Ruby Solution to this problem:**

1. Create a script to web crawl ESPN's fantasy projections to reduce the time cost of inputting projections. I used [Kimono Labs](http://www.kimonolabs.com/) to scrape the data into an API because of its easiness. In the future, I'll probably use Nokogiri to get my data from the web to reduce third party dependencies.

2. Parse the data from the API and enter it into an algorithm to fit my league's scoring settings.

3. Write this data to a new CSV each time so that every time I run the script from my terminal I get the most up to date projections.

**Next Steps for Fantasy Football**

1. I want to take a similar approach, but improve upon it. I am going to get projections from multiple sites and average them together. Also, from an OOP/data standpoint, things can be more flexible and broken down.

2. Right now baseball is just a script that doesn't have any bells or whistles and is something that I will probably be the only one to use. If your league has different settings than mine, you need to manually go into the code and tweak the algorithms, not hard to do, but cumbersome. For football I want to make this a web app that makes it easy for people to adjust settings based on their leagues and something that people will actually use.

**tl;dr** Check out [https://github.com/landonmarder/fantasy_baseball](https://github.com/landonmarder/fantasy_baseball) to clone my project and get up to date fantasy baseball projections. Hopefully I will do something cooler/better for football.
