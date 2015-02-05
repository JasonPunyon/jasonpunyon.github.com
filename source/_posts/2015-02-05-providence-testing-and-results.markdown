---
layout: post
title: "Providence: Testing and Results"
date: 2015-02-05 08:38
comments: true
categories: providence
---

*This post is part of a series on the Providence project at Stack Exchange.  The first post can be found [here](http://kevinmontrose.com/2015/01/27/providence-machine-learning-at-stack-exchange/).*

The Providence project was motivated by our desire to better understand our users at Stack Exchange. So we think we’ve figured out what kind of [developers come to our sites](http://kevinmontrose.com/2015/01/27/providence-machine-learning-at-stack-exchange/), and [what technologies they’re using](http://kevinmontrose.com/2015/01/29/providence-what-technologies-do-you-know/). Then we figured out a way to combine all our features into the [Value Function](http://kevinmontrose.com/2015/02/04/providence-matching-people-to-jobs/). How did we test these features?

After each feature worked in the clean room (it passed the muster on the training, validation, and test sets we'd labeled) we had to drop it into some real-world mud to try and move a needle. Please attempt to contain your surprise as I tell you we chose to use ads as our mud puddle, and click-through rate as our needle.

{% img center http://i.imgur.com/FkWbS9y.png %}

The particular ads we wanted to use were the Careers Job Ads that run in the sidebar of Stack Overflow. Last we left Careers Job Ads, we knew that [geography](http://jasonpunyon.com/blog/2013/01/20/geographys-the-fuck/) was by far the most important feature we could use to select jobs to show you in the ads. We’ve done some visual updates since then, but the job selection algorithm has remained unchanged.

<p style="text-align:center">
{% img http://i.imgur.com/kl5MRME.png %} {% img http://i.imgur.com/qEJeg1b.png %}
</p>

We had a number of features in the pipeline at this point that were untested in real-world conditions. Major Dev Kinds, Minor Dev Kinds, Tech Stacks and Job Tags were the individual features we had to test. We did those first and then the final test was for the Value Function that combined each of those individual features and geography. Each test had these phases:

###Labeling Party: The Jobening

We had a model for users, but we weren’t going to model jobs at the same time because then we wouldn’t know whether we were testing the user model or the job model. We also needed the cleanest labels on jobs that we could get which meant good ol’ fashioned home grown fresh squeezed human labels. Gotta give a big shout out to the developers at Stack Exchange for giving us their time to label each data set.

###Pre-analysis and Experiment Design

This is a thing we’re getting more and more into here. We’ve wasted a lot of cycles on tests and ideas that would’ve been avoidable if we’d just looked at the data ahead of time. We needed to know approximately how many impressions we were going to get in the test groups so we would know how long we’d have to wait for the A/B results to converge. If it was going to take months for the results to converge for one of these experiments, we’d be better off just doing a different experiment. We had to know when we were going to have to give up controlling for important factors like geography in order to get enough data points in a bucket.

###Turning it up

Once we knew how long we’d have to wait, we did some easing in. We’d run the test at 1% for a day to work out any bugs, monitor the early impression distributions and click-through rates to make sure they weren’t getting completely hosed and that we were also measuring correctly. Then we’d slowly ramp it up during the second day to 100% of usable impressions.

###Wait

{% img center http://i.imgur.com/L77yMSg.gif Just watching the hours tick byyyyyyyyyyyyy... %}

###Post Analysis

At this point we’d have click-through rates that had supposedly converged, but it wasn’t enough that CTR just went up. We’d also have to make sure that gains on one subgroup weren’t at an unreasonable expense to the other groups. We called these checks “Degeneracy Passes”.

By the end, we were able to get through about a test per week. By the luck o’ the data scientist we even got some decent results.

##Results

The Value function performs pretty admirably in comparison to the geography based targeting we were using before. We saw ~27% lift. 

Looking at CTR improvement by geography, we found that jobs closer to the user improved less, confirming our previous experiments that geography is pretty important. Inside 60 miles we saw ~17% lift picking jobs using Value Function. Outside 60 miles we saw even larger gains.

{% img center http://i.imgur.com/u1orDz3.png %}

Looking jobs by some popular dev kinds we saw the following gains:

{% img center http://i.imgur.com/GkLcbZI.png %}

It wasn’t all kitten whispers and tickle fights. Desktop OSX jobs lost about 6% of their clicks, but we only had 9 of those jobs in the test. Similarly, SharePoint Integration jobs lost 100% of their clicks in the test, but there were only 3 of those jobs. I guess we’ll just have to live with those terrible blemishes for now.

Looking at a jobs with a smattering of popular tags we saw these gains:

{% img center http://i.imgur.com/sE3x6Bl.png %}

Node really stands out there (I think we just got lucky), but we were pretty happy with all those gains. We didn’t find any losers among tags used on more than 50 jobs.

*Next up: The Architecture of Providence*