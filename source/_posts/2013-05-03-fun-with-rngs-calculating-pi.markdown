---
layout: post
title: "Fun with RNGs: Calculating &#960;"
date: 2013-05-03 15:41
comments: true
categories: 
---

So, calculating &#960; is a fun pastime for people it seems. There are many ways to do it, but this one is mine. It's 12 lines of code, it wastes a lot of electricity and it takes forever to converge.

{% codeblock lang:csharp %}
public double EstimatePi(int numberOfTrials)
{
	var r = new Random();
	
	return 4 * Enumerable.Range(1, numberOfTrials)
					     .Select(o => {
										var x = r.NextDouble();
										var y = r.NextDouble();
										return Math.Pow(x, 2) + Math.Pow(y, 2) < 1 ? 1 : 0;
									  })
						 .Average();
}
{% endcodeblock %}

What's going on here? First we initialize our random number generator. Then for 1 to the number of trials we specify in the argument we do the following:

1. Generate two random numbers between 0 and 1. We use one for the X coordinate and one for the Y coordinate of a point.
1. We test if the point (X,Y) is inside the unit circle by using the formula for a circle (x^2 + y^2 = r^2).
1. If the point (X,Y) is inside the circle we return a 1 otherwise a zero.

Then we take the average of all those zeros and ones and multiply it by a magic number, 4. We have to multiply by four because the points we generate are all in the upper right quadrant of the xy-plane.

How bad is it? Here's some output:

		Number Of Trials       Estimate of Pi
   			10					3.6
	        100					3.24
	        1000				3.156
	        10000				3.1856
	        100000				3.14064
	        1000000				3.139544
	        10000000			3.1426372
	        100000000			3.14183268
	        1000000000			3.141593 (Took 2:23 to complete)