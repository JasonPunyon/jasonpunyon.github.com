---
layout: post
title: "Trip Report: ICML 2016"
date: 2016-07-11 14:21:00 -0400
comments: true
categories: 
published: false
---

Me and ICML started off on the wrong foot this year. The tutorials session started at 8:30AM on Father's Day. This was decidedly a cramp in the dick. Flying Saturday afternoon cut the weekend with the kids even shorter. Couldn't muster a Saturday night jaunt through NYC so I carped the per diem and fueled an evening of frustrated coding with room service 40 stories up from ricketa racketa of Times Square.

##The Tutorials

I awoke in the city that doesn't sleep and headed down for the first tutorial. Met by the level of spoiled we'd been a year ago in Lille...

<div style="display: flex">
<div style="text-align:center">
<h4>Last Year</h4>
<blockquote class="twitter-tweet" data-lang="en" data-width="100%"><p lang="en" dir="ltr">Lotta ML peeps. (this is just one auditorium running right now) <a href="https://twitter.com/icml15">@icml15</a> <a href="http://t.co/kNpEM0qeSc">pic.twitter.com/kNpEM0qeSc</a></p>&mdash; JSONP (@JasonPunyon) <a href="https://twitter.com/JasonPunyon/status/617954302250885120">July 6, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
</div>
<div style="text-align:center">
<h4>This Year</h4>
<blockquote class="twitter-tweet" data-lang="en" data-width="100%"><p lang="en" dir="ltr" width="250">Hope the <a href="https://twitter.com/icmlconf">@icmlconf</a> presenters aren&#39;t gonna rely too much on their slides. <a href="https://t.co/oekdppUrYy">pic.twitter.com/oekdppUrYy</a></p>&mdash; JSONP (@JasonPunyon) <a href="https://twitter.com/JasonPunyon/status/744509684745371648">June 19, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
</div>
</div>

...I took my seat.

###Deep Residual Networks

All that junk melted away for a couple hours while Kaiming He broke down [Deep Residual Networks](http://kaiminghe.com/icml16tutorial/icml2016_tutorial_deep_residual_networks_kaiminghe.pdf). Why go deep with Neural Networks to begin with? The goal is to build better representations of your data which hopefully yield better generalization and depth has been shown to do that. But depth isn't free, it comes at the cost of not just more but more difficult training of the network. 

At 10 layers careful initialization of weights and batch normalization become necessary. Think about multiplying a number repeatedly with itself when that number is very close to, but not exactly 1. If you have a number slightly larger than 1 and you multiply it over and over again, it's going to blow up. 

For example:  
    
1.01 × 1.01 × 1.01 × 1.01 × 1.01 = 1.01<sup>5</sup> = 1.05
    
But:  
    
1.01 × 1.01 (...998 more times) = 1.01<sup>1000</sup> = 20959

Similarly, if you have a number slightly less than 1 and you multiply it over and over again, it's going to disappear. The amount of the blowing up or disappearing has to do with how many times you multiply and the deviation from 1.

There's an analogous repeated multiplication in the layers of a linearly activated neural network. The quantity that gets multiplied is (the variance of the weight matrix for the layer × the number of nodes) for all the layers. More layers, more multiplications. If you don't choose initial weights to limit the variance just right, the multiplications just swamp the signal. Batch normalization transforms the other part of the model (the data) to make its variance more well behaved as it passes into each layer, so as not to disrupt the signal.

Before you get to 100 layers even these machinations are no longer enough. Eventually adding more layers just increases training error. Optimization difficulties begin to dominate and you need something else to fix it. Enter Residual Nets.

Residual Nets (ResNets) are a change to the architecture of the neural network, which induces a change in the representation the network learns. If you zoom in on a block of two adjacent layers of a "plain" network, they might look like this...

<img class="center" src="http://i.imgur.com/Zx4TWb5.png" />

For a moment, imagine that this block had to learn to exactly reproduce its inputs. Sounds silly right, who wants to learn the identity function? But your conclusion has to be that it's actually pretty hard for the block to do that. The weights have way too much power for the function they're trying to represent, so the nodes in the block have to fight amongst themselves to cancel out their effects just right to build up the identity function around your data.

The idea of ResNets is to change the block to look like this...

<img class="center" src="http://i.imgur.com/9rJfvQX.png" />

ResNets add an identity skip-connection (or shortcut-connection) between the input and output of the block. This builds up the output signal from two parts: the input, and whatever's left over, called the residual.  It's easy for this block to learn the identity function. The learning part of the block that models the residual just has to learn to indiscriminately output zero. That's a much easier objective to hit than learning the identity in the plain net.

Who wants to learn the identity function? The answer is *no one*. The ResNet architecture ensures you never have to. And as an added bonus, when the block isn't learning the identity and there *is* a non-zero residual, the representative power of the weights is brought fully to bear characterizing that residual, instead of wasted on rebuilding the input from scratch.

And what about those repeated multiplications we were worried about when straight stacking linear layers? The ResNet architecture *turns them all into additions!* I saw the mathematical prestige coming maybe 10 slides ahead and I was squirming with glee in my seat when it finally arrived. Transforming the multiplications into additions makes forward and backpropagation buttery smooth. Repeatedly adding numbers together instead of multiplying them means the probability of the signal exploding galactically or petering out is greatly reduced.

ResNets brought it pretty hard this year...expect more.

<img class="center" src="http://i.imgur.com/AerzarZ.png" />

###Memory Networks

When I say the words "Intelligent Conversational Agent", I'm guessing you don't think "Siri". But that's basically the best we've got right now. We string together a few black boxes with some basic logic et voilá! Siri can do anything, as long as it's setting a timer. 

<img class="center" src="http://i.imgur.com/YNxRO1v.png" />

Right now, bots can't handle open ended conversations of any length really. They can't put together the flactobytes of data in the cloud to do something simple like order some chinese food you like. They can't learn new words from you in conversation. They don't even know all the words you use now.

Jason Weston (et al) are betting that Memory Networks are the [way forward](http://www.thespermwhale.com/jaseweston/icml2016/icml2016-memnn-tutorial.pdf). Memory Networks are a class of model that augments a neural network with an explicit memory bank that can be written and read, and trained with backpropagation. Memory networks are a direct attack on the problem of context. The network encodes every sentence it's ever seen (CONTEXT ALL THE THINGS!) into a vector representation and then keeps them in a table. Every time the network is asked a question, it encodes the question, searches through all its memories (perhaps multiple times) figuring out which ones are relevant (attends to them), then weighted averages them into an output that is then decoded to answer to the question. The network "reasons with attention over memory".

You can't jump right to autonomous learning dialog (duh), so the tasks at hand are in question answering. I have but a [passing interest](https://stackoverflow.com) in such things, which is why I always go to Weston's talks. The datasets they use are almost as fun as the model. The synthetic one is the [bAbI tasks](https://research.facebook.com/research/babi/) which I learned about at ICML [last year](http://jasonpunyon.com/blog/2015/07/17/trip-report-icml-2015/#jason-weston). They're a set of unit tests for different things like positional reasoning and counting.

<img class="center" src="http://i.imgur.com/Ek8xZPZ.png" />

They also try the model against Children's Books, dialog about movies, and ubuntu chat logs. They're obviously nowhere near done, but with Facebook Messenger's bot platform [out this year](http://newsroom.fb.com/news/2016/04/messenger-platform-at-f8/), they're about to get a whole lot more real world data. It'll be exciting to see where it ends up.

And that was the end of the tutorials. There was one more session, but it *was* Father's Day, and my own Dad wasn't too far away so...

<blockquote class="twitter-tweet tw-align-center" data-conversation="none" data-lang="en"><p lang="en" dir="ltr">OK <a href="https://twitter.com/icmlconf">@icmlconf</a>, you&#39;re half forgiven for this Father&#39;s Day scheduling because I did get to see my Dad. <a href="https://t.co/qyr9JtK7Bz">pic.twitter.com/qyr9JtK7Bz</a></p>&mdash; JSONP (@JasonPunyon) <a href="https://twitter.com/JasonPunyon/status/744666495192547328">June 19, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

###Causal Inference for Policy Evaluation

Causal Inference
P-values, Confidence Intervals are table stakes for statisticians in causal inference. ML doesn't have them.
You want that stuff when you're working on thorny issues like "What happens if we raise the minimum wage?"

###A Quest For Visual Intelligence

History Lesson,
What can people write in 

https://www.youtube.com/watch?v=1SECE4ZOiX0

http://cs.stanford.edu/people/karpathy/densecap/

###Neural Nets: Back to the Future

You know those "History of Science" or "History of Math" courses that upperclasspeople get to take and there's never enough slots? This workshop was like that but for neural nets. This walk down memory lane was to look at the trajectory of research as it's played out over the last 30 years, and recall hidden gems that didn't get their dues. A buncha people from years ago, talking shop from years ago. 

We heard from Larry Jackel about Bell Labs in Holmdel, NJ. This was kind of the beginning of it all. Convolutional Neural Networks and Support Vector Machines came from the people in this group. Fast forward and now Jackel's at NVIDIA teaching cars to self drive. We got to see video of them tooling around near the grounds of the lab in their creation. It's gotta be something to begin your career microfab'ing neural-net hardware and jump to 30 years later being driven around by your neural net on the actual street.

We also heard from 