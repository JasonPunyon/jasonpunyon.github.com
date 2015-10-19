---
layout: post
title: "Trip Report: ICML 2015"
date: 2015-07-17 09:13:45 -0400
comments: true
categories: 
---

One of the awesome benefits of working at Stack Overflow is the Conferences and Education policy. You get a ballpark budget ($3500) and time (3 days) each year to update your skills and knowledge. In return for all this conferencing and educationing you write up a summary of what you learned and your experience at the conference.

This year I went to the [International Conference on Machine Learning](http://icml.cc/2015/) in Lille, France. ICML is an academic conference broken up into three parts: 1 day of tutorials, 3 days of "The Main Conference", and 2 days of Workshops.

**TL;DR**: I loved the tutorials, the keynotes, and the workshops. The paper sessions were hit-or-miss but I might've been doing it wrong (sorry, I'm new to the job). It was a great conference, I'm gonna go again next year.

##The Tutorials
The tutorials were my favorite part of the conference. I wish the whole conference were tutorials like the ones I went to on this first day. There were three two-hour-long sessions that started at the surface and tried to dive somewhat deeply into their respective topics.

###Natural Language Understanding: Foundations and State-of-the-Art
This was my favorite tutorial. It was given by Percy Liang, a computer science professor and natural language researcher at Stanford. One of the presenter's goals was to seed the minds of ML theorists and practitioners with the essential phenomena and sharp corners of natural language in hopes of motivating new ideas. The amount of information he related in such a short time was impressive and there's no way I can do it justice in a paragraph. Read through [the slides](http://icml.cc/2015/tutorials/icml2015-nlu-tutorial.pdf) to get an idea. There's video coming in September.

###Advances in Structured Prediction
The next tutorial was a two-parter on Structured Prediction from Hal Daume III and John Langford. I liked this tutorial because I knew the least about structured prediction going in and I felt like I had at least a tenuous grasp on it by the end. Structured prediction is about answering the question "What can you do if you're going to give up the independence assumption and model a joint probability distribution?" The tutorial went through what structured prediction was and existing methods, then talked about their method (Learning to Search), empirical results, performance, and advertised their fast/flexible implementation in Vowpal Wabbit. [slides](http://icml.cc/2015/tutorials/AdvancesStructuredPrediction.pdf)

###Modern Convex Optimization Methods for Large-scale Empirical Risk Minimization
The final tutorial of the day was another two-parter on convex optimization. Machine learning is frequently formulated as minimizing (optimizing) a loss function. Certain loss functions have mathematical properties that make them efficiently optimizable and we call those functions "convex". Two subsets of methods within convex optimization are: "primal" methods (gradient descent) and "dual" methods (coordinate descent). The first part of the talk covered primal methods and the second part of the talk covered the dual methods and concerns at-scale like minibatching, parallelization, and distributed implementations.

I liked this talk because it was mathy and about performance characteristics on big problems and we really dig that kind of thing at Stack Overflow, however this seemed like it was the furthest tutorial from me practically. It's nice to know that people are attacking that size of problem but our problems just aren't big enough to warrant some of these techniques. [part 1 slides](http://icml.cc/2015/tutorials/2015_ICML_ConvexOptimization_I.pdf) [part 2 slides](http://icml.cc/2015/tutorials/2015_ICML_ConvexOptimization_II.pdf)

##The Main Conference

Each day of the main conference had a keynote address and then 3-4 sessions of 3-5 short talks on submitted papers organized into different tracks. There were also poster sessions where you could meet and greet the researchers and dive into their paper with them in more depth.

###Keynotes

Susan Murphy's "Learning Treatment Policies in Mobile Health" was an interesting tale at the corner of healthcare funding, clinical trial design, machine learning, and mobile development. Smartphones might be able to provide effective, low-cost, personalized, just-in-time treatment for things we don't fund particularly well in the US like drug addiction treatment. How to even study the problem is a can-of-worms factory. Conflating factors abound where you're vying for a subject's attention via their phone in the real world instead of in an in-patient treatment setting. Trying to learn effective treatment policies through the noise is a crazy-ambitious goal. I was really impressed by their devotion to the ethics of the research. Things like refusing to have the phone light up/deliver messages when it was moving above a certain speed. No point in trying to help the person's drug addiction if messaging them causes them to crash their car.

Jon Kleinberg's "Social Phenomena in Global Networks" talked about how we're recording social activity more granularly than ever and insights that can be gleaned from looking at a person's network neighborhood in a social graph and the interactions the person has with their network over time. He discussed the different features to extract from the graph in order to solve the problem of identifying a person's romantic partner.

####Two High-Stakes Challenges in Machine Learning

Both of those were super interesting but they were eclipsed by Leon Bottou's talk on [two high-stakes challenges in machine learning](http://icml.cc/2015/invited/LeonBottouICML2015.pdf). The first challenge is that machine learning components are fundamentally different from the things we're used to in software engineering. ML components aren't rigid like sorting routines, they're squishy. The guarantees they provide are weak. If the training set data distribution is different from the data distribution in situ for any reason, all your guarantees go out the window. Your component just ambles around drunkenly in an alien world grasping for vestiges of the only reality it knows.

"AHA!" you think. "I ain't no fool. We won't fall victim to the data distribution drift problem with statically trained models because we won't use 'em! We'll learn *online*!" I, for one, greatly admire the bold leadership it takes to suggest that we actually fight fire with fire. Some would say "Maybe we've got enough learning," but you say "Maybe we haven't got enough learning enough!" I like you.

The problem with learning online is that you've created an even shakier foundation for your engineering division, because feedback loops are *everywhere*. If your organization has more than one online learning ML algorithm in a particular pipeline you've got a system where one piece can learn from another piece that is learning from the first piece. These feedback loops can manifest themselves at two online algorithms in the pipeline, imagine what happens when you're trying to learn online in many places across a large engineering organization.

The second challenge is that the experimental paradigm that has driven machine learning forward over the last few decades is reaching its limits. Machine learning has a single experimental paradigm: split up the data into a test set and training/validation sets, train on the training/validation sets, and test final performance on the test set. Machine learning is an outlier among experimental sciences in that it has a single paradigm. Other sciences have to work much harder to design their experiments and interpret their results.

One of the problems with this single paradigm and its focus on datasets is that [large datasets are biased](http://people.csail.mit.edu/torralba/publications/datasets_cvpr11.pdf). Training and testing on biased datasets leads to unrealistic expectations of performance. The image below pretty much tells the story. When classifying cars, models trained on one of the large datasets perform worse when tested on the other large datasets almost universally. If cars were represented in these datasets in an unbiased way you'd expect much smaller differences in performance.

{% img http://i.imgur.com/KgOfZGh.png %}

And while we're at it, we aren't even learning the right things if we want to acheive the heights of our increased ambitions. Our solution to classifying is statistical classification. But it's hard to find a purely statistical basis in which to ground some problems like the one brought up in this slide...

{% img http://i.imgur.com/v6FUATk.png %}

These insights were really eye-opening for me coming from a young engineering organization that's only recently become more rigorous about adding ML components to our stack.

###Papers

Paper sessions were scheduled around the keynotes. Each session had a topic, some topics spanned multiple sessions over the three days. The sessions all went similarly, a presenter would present their paper in ~15 minutes, then there was time for a few questions, then...next presenter. 

This ended up being pretty grueling by the end of the third day. There wasn't really time to just think about any of the things that were presented to you. Trying to put together the-problem-you-didn't-even-know-was-a-problem-3-minutes-ago and the research-at-the-edge-of-human-understanding that was just presented as the solution to that problem was a difficult process to repeat 3 times an hour, 6 hours a day, for 3 days. That's not to mention synthesizing the information from the day's keynote, and the poster sessions. These were long days. Jet lag wasn't helping either.

At any time there were 6 paper sessions going on, so my strategy was to just pick the topic that I was most interested in. This was a bit of a high variance strategy as the sessions had a hit-or-miss quality to them. Other people used a more informed strategy of picking specific papers/presenters they knew to be good and then chose their sessions based on that. There's no "Map of the Machine Learning Stars" to consult though...my lack of familiarity with a lot of the field precludes that kind of strategy. Maybe when I've been around a bit longer I'll have a better handle on that kind of thing.

##The Workshops

The final piece of the conference was the workshops. These were little mini-conferences organized by groups repesenting subfields of machine learning. I was happy to attend the 2-day workshop on the latest in Deep Learning. Invited speakers gave hour long talks on their research and took questions. The first day was supervised learning, the second was on unsupervised and reinforcement learning.

The highlight of the two days was Jason Weston's presentation on the [bAbI tasks](https://research.facebook.com/researchers/1543934539189348). Leon Bottou mentioned this work in his keynote as an example of a way to evaluate machine learning systems that wasn't just another instance of the normal training/validation/test set paradigm. The context for the work is in Question Answering. The computer is presented a small story and asked a simple question like this...

{% codeblock %}
Mary moved to the bathroom.
John went to the hallway.
Where is Mary?
{% endcodeblock %}

You can classify the questions asked by the kind of knowledge necessary to answer them. The example above is of task #1 "Basic Factoid QA with a Single Supporting Fact". Here's an example of task #14 "Time manipulation"...

{% codeblock %}
In the afternoon Julie went to the park.
Yesterday Julie was at school.
Julie went to the cinema this evening.
Where did Julie go after the park?
{% endcodeblock %}

There are 18 more of these tasks that all test a different aspect of the machine's "intelligence". Counting, Negation, Deduction, Positional reasoning are some of these aspects. The bAbI tasks are interesting because they attempt to probe a specific ability of the program, much like a unit test. Cataloging the abilities of an ML system in this way was something I haven't come across before. I'm really drawn to the idea.

##Wrap Up

ICML 2015 was a great conference. It reminded me of being in the physics research community and rekindled my latent love for academia. I've been away from it for a long time. ICML 2016 is in NYC next year which is a lot closer to home. I'll definitely be going.