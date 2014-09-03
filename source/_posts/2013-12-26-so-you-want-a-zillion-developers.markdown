---
layout: post
title: "So you want a zillion developers&hellip;"
date: 2013-12-26 11:29
comments: true
categories: 
---

I work at [Stack Overflow](http://stackoverflow.com) on [Careers 2.0](http://careers.stackoverflow.com). In addition to our job board we have a candidate database where you can search for developers to hire. Our candidate database has 124K+ developers in it right now.

Customers frequently gawk at this number because they've looked at other products in the dev hiring space that offer **millions** of candidates in their databases. [Sourcing.io](http://sourcing.io) claims to have "over 4 million developers" in their database. [Gild](http://www.gild.com) offers "Over 6 Million Developers". [Entelo](http://www.entelo.com/) will give you access to "18+ million candidates indexed from 20+ social sites."

###Yeah man, your numbers stink

Hey. That hurts.

Let's put those numbers in perspective. The vast majority of the developers "in" these other databases don't even know they exist. The devs never signed up to be listed or even indicated that they were looking for work. There isn't even a way to opt out. These databases are built by scraping APIs and data dumps from sites developers *actually* care about like Stack Overflow and GitHub. 

On the other hand the *only* people you'll find in the Careers 2.0 database are ones who made the *affirmative choice* to be found. They start by getting an invitation to create a profile. They build out a profile with their employment and education history, open source projects, books they read, peer reviewed answers on Stack Overflow, and so on. Then they can choose to be listed as either an active candidate (they're in the market for a job right now) or a passive candidate (they're happy where they are but are willing to hear your offer). After a candidate gets hired they can delist themselves from the database so future employers don't waste any time on them.

So the difference between us and them is that we give you a smaller number of candidates who are already interested in job offers and they give you a giant database filled with hope and built by skeez.

We have some data from Careers that tells us hope is not a recruiting strategy.

###Our Experiment

Careers 2.0 experimented with the "index a bunch of people who don't know they're being indexed" model to see if it could possibly work. We created what we called "mini-profiles" which consisted exclusively of already public information available on Stack Overflow. We would add mini-profiles to the database if the Stack Overflow user provided a location in their profile and had a minimum number of answers with a minimum score. We showed these mini-profiles along with our "real" candidates in search results. If an employer wanted to contact one of the people behind a mini-profile Careers 2.0 would send them an e-mail asking if they want to open up a conversation with the employer. If the candidate wanted to continue they could authorize us to share their contact information with the employer and they'd start working on their love connection.

###Our Results

We track response rates to employer messages to look out for bad actors and generally make sure the messaging system is healthy. A candidate can respond to a message interested/not interested or they can fail to respond at all. Response rate is defined as Messages Responded To / Messages Sent. When we compared the response rates of messages to mini-profiles to the response rates of messages to "real" profiles the results were not good for mini-profiles. **Messages to "real" profiles were 6.5x more likely to get a response than messages to mini-profiles.** That was the last and only straw for mini-profiles. We retired the experiment earlier this year.

###So what about the zillions of programmers?

All those services I named at the beginning of this post do what we did in our experiment, just a little more extensively by including devs from more places online. I have to believe that the response rates from their unqualified leads are similar to the ones we found in our experiment. I suppose technically the response rates from randodevs on GitHub or Bitbucket could be higher than that of randodevs on Stack Overflow thus invalidating our conclusion, but anecdotal evidence from our customers about those other services suggests not.

"Wait a sec there Jason," you're thinking, "if their databases are at least 6.5x larger than yours I'll still get more responses to my messages right?" Absolutely! That's called **spam**. You are totally allowed to go down the path of the spammer but let me hip you to the two problems there. The first problem with your plan is that devs hate recruiting spam more than they hate PHP, and they hate PHP alot. The word will get out that you're wasting everyone's time. [People will write about it](http://jasonpunyon.com/blog/2013/03/31/things-that-were-i-to-unfortunately-wake-up-tomorrow-as-a-recruiter-i-would-never-do/). The second problem is that spam is supposed to be cheap. This isn't cheap. In this case you'll have to spend at least 6.5x the time wading through these zillions of devs identifying the ones that meet your hiring criteria, messaging them, and waiting for responses. So not only are you wasting their time, you're wasting yours.

We aren't going to build this business off hope and spam and wasting people's time. If a smaller database is the price, so be it.