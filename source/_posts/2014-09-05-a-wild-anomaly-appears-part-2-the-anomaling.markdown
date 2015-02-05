---
layout: post
title: "A Wild Anomaly Appears! Part 2: The Anomaling"
date: 2014-09-05 16:50:47 -0400
comments: true
categories: ml	
---

After all the rave reviews of my last post I knew you were just on the edge of your seat waiting to hear more about my little unsupervised text anomaly detector. 

<a href="http://i.imgur.com/94aVQ7k.png"><img class="center" src="http://i.imgur.com/94aVQ7k.png" /></a>
<a href="http://i.imgur.com/9JSJPYd.png"><img class="center" src="http://i.imgur.com/9JSJPYd.png" /></a>
<a href="http://i.imgur.com/MShsVz7.png"><img class="center" src="http://i.imgur.com/MShsVz7.png" /></a>

So, we've got some [working ML](http://jasonpunyon.com/blog/2014/09/02/a-wild-anomaly-appears/)! Our job's done right?

<a href="http://i.imgur.com/aSHZLkH.png"><img class="center" src="http://i.imgur.com/aSHZLkH.png" /></a>

'Round these parts, it ain't shipped 'til it's fast *and* it's got it's own chat bot. We spend all day in chat and there's a cast of characters we've come to know, and love, and hate.

##Pinbot

<a href="http://imgur.com/qaqcvek.png"><img class="center" src="http://imgur.com/qaqcvek.png" /></a>

Pinbot helps us not take each other's database migrations. You can say "taking ###" and Pinbot removes the last taken migration from this room's pins and replaces it with this one. So we always know what migration we're up to, even if someone hasn't pushed it yet. Also pictured: Me calling myself a dumbass in the git logs. Which gets broadcast by our TeamCity bot. Someone starred it.

##Hair on Fire

<a href="http://i.imgur.com/yt2rLV4.png"><img class="center" src="http://i.imgur.com/yt2rLV4.png" /></a>

Hair on fire bot helps Careers keep making money. Hair on fire pops up every now and again to tell us a seriously critical error that affects the bottom line has happened on the site. If someone is buying something or on their way to buying something and gets an exception Hair On Fire dumps the details directly to chat so someone can get to work immediately.

##LogoBot

<a href="http://i.imgur.com/CGCeY9l.png"><img class="center" src="http://i.imgur.com/CGCeY9l.png" /></a>

Here's another little Careers helper. We have a policy that any advertising images that go up on Stack Overflow *must* be reviewed by a real live person. When display advertising is purchased from our ad sales/ad operations teams, they do it. When we introduced [Company Page Ads](http://careers.stackoverflow.com/products/company-pages) we automated the process of getting images on Stack Overflow and no one had to talk to a sales person. This begat LogoBot who reminds us to do our job and make sure no one's putting up animated gifs or other such tawdriness.

##Malfunctioning Eddie

<a href="http://i.imgur.com/KvswKSK.png"><img class="center" src="http://i.imgur.com/KvswKSK.png" /></a>

Malfunctioning Eddie's...got problems. 

##Anomaly Bot

Which brings me to the Anomaly bot. We need to make sure that all these anomalous jobs I'm detecting get in front of the sales support team. They are the human layer of detectors I alluded to in my last post who used to have to check over every single job posted to the board. 

<a href="http://i.imgur.com/ZJDszbV.png"><img class="center" src="http://i.imgur.com/ZJDszbV.png" /></a>

There it is. Anomaly bot. Where does it lead us puny humans?

<a href="http://i.imgur.com/zBsNv8n.png"><img class="center" src="http://i.imgur.com/zBsNv8n.png" /></a>

Welcome to the super secret admin area of Careers. At the top of the page we have a list of the jobs that were posted today. There are 3 columns, the anomaly score (which is based solely on title), the job title, and a few buttons to mark the job anomalous or not. The second section of the page is for all jobs currently on the board.

<a href="http://i.imgur.com/6WOgbJV.png"><img class="center" src="http://i.imgur.com/6WOgbJV.png" /></a>

I'm hoping the heatmap pops out at you. It runs from Red (pinned to the all-time most anomalous job) to Green (pinned to the all-time most middle-of-the-road job ever). The jobs posted today are light orange at worst, so that's pretty good! On the "all jobs" list there's a bunch of red that we need to review.

Just to give a reference, here was the first version sans heatmap.

<a href="http://i.imgur.com/qawGqwB.png"><img class="center" src="http://i.imgur.com/qawGqwB.png" /></a>

So much more information in that tiny extra bit of color. If you want to make simple heatmaps it's really easy to throw together some javascript that uses [the power of HSL](http://stackoverflow.com/questions/340209/generate-colors-between-red-and-green-for-a-power-meter/340214).

##What's Next?

We're gonna let this marinate for a while to actually test my hypothesis that we only have to look at the top 10% of jobs by anomaly score. The sales support team's gonna clear the backlog in the "all jobs" section of the report, then use the tool for a little while and then we'll have the data we need to actually set the threshold. Once we do that the Anomaly bot can get a little smarter. Right now Anomaly bot just shows every three hours with that same dumb message. Maybe it'll only show up when there's jobs above our human-trained threshold (modulo a safety factor). Maybe we'll change it to pop up as soon as an anomalous job gets posted on the board.

##Here, have some code

If you want to use the very code we're using right now to score the job titles it's up on [Nuget](http://www.nuget.org/packages/AnomalousText/), and the source is on [Github](https://github.com/JasonPunyon/AnomalousText)

<h2></h2>

<a href="http://careers.stackoverflow.com/jobs/62161"><img class="left" src="http://i.imgur.com/iqLGcMN.png" /></a>

Got experience solving problems like this one? Wanna work at a place like [Stack Exchange](http://stackexchange.com/work-here)? Head on over to our completely middle-of-the-road [job listing](http://careers.stackoverflow.com/jobs/62161) and get in touch with us.


