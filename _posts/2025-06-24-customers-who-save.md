---
layout: post
title: "15 minutes could save you from having to understand conditional probability"
tags: stats
---
I recently received some junk mail from a national insurance company, which touted: "Customers who save when they switch save an average of over $800". OK, so how many didn't save money, and how much more they were charged? At least it's not as meaningless as the GEICO slogan: "15 minutes could save you 15% or more." It could save you nothing. It could cause frogs to rain from the sky. Anything could happen in 15 minutes. 

Seriously, though, here's a toy example. Let's say out that $$n$$ people received an insurance quote that saved $$x$$, and $$m$$ people received a quote that was more expensive by $$y$$. Let's understand $$x$$ to be positive and $$y$$ to be negative. Regardless of how huge $$m$$ or $$y$$ might be, the expected value over the positive values is still $$x$$.

Let's explore a scenario that's a little more generous. Assume the random variable $$X$$ has a [standard normal distribution](https://en.wikipedia.org/wiki/Normal_distribution#Standard_normal_distribution) with a mean of zero and a standard deviation of 1. What's the expected value over the positive part of the distribution only? Symbolically, that's $$E(X\|X>0)$$, but it's a little easier to just set up a new random value Y on the positive reals and figure out $$E(Y)$$. Fortunately, that already has a name: the (standard) [half-normal distribution](https://en.wikipedia.org/wiki/Half-normal_distribution). I could bore you with the derivation anyways, but let's not: the mean of that distribution is $$\sqrt{\frac{2}{\pi}}$$, about 0.8. One wonders if Geico's 15% is where 0.8 standard deviations lies on the distribution of savings.

(Hi! I'm not dead, I just got busy with the kind of work that actually pays me.) 