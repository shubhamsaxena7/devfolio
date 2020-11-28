---
title: Software Entropy 
date: 2018-08-22 00:00:00
type: post
---

I find it weird that these two terms, software and entropy can coexist in the same sentence, but as it turns out, it’s really a thing.
Let’s get going with a little bit of physics ( nothing to be afraid of 😜 )

**Second Law of Thermodynamics**

*the total entropy of an isolated system can never decrease over time. Or in other words, the universe is always moving from order to disorder*

We can apply this to other aspects of life as well. If I stop actively maintaining my garden it will turn into a chaotic jungle of twisted branches and weeds. If I own a car, it will gradually wear down until it breaks down completely.
We can do work to maintain and even temporarily increase the order of things, but inevitably it all becomes less orderly.

Software is no different. It needs maintenance and work very frequently during its lifecycle to avoid breakdowns. Software entropy can be a good proxy for measuring the data points you need to fully understand and maintain your code base.

**How to define software entropy?**

Entropy is essentially a measure of randomness, and randomness in code leads to unmaintainable software. If you find your code is getting hard to maintain and work over, it’s probably good to look over some of the signals of high entropy.

**How to measure software entropy?**

Unmaintainable software has various properties. Some of these are subjective, but nonetheless provide a good indicator of high entropy.
* software age
* poor unit test coverage
* obsolete documentation
* coupled code; easily measured by number of functions/files which needs changing to implement a feature
* high cyclomatic complexity
* broad interfaces; essentially has a lot of unexpected behaviour
* As is evident, higher software entropy is rarely a good idea. What do we do?

**Keeping it in control!**

***refactor, refactor, refactor!***

Refactoring is akin to taking care of your code much like clearing out weeds from your garden. Do it whenever you have the chance.
