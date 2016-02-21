---
layout: post
title: Are you certain? A Bayesian method for simple A/B testing
---

Scientists, unlike Siths rarely deal with absolutes. Even if you have the most water-tight results there is always the possibility that any difference or lack thereof is due to random chance, and any conclusions you draw from them are false - this is why we have statistical tests, to formalise our confidence in a conclusion.

If your experiment is nice and simple, say to determine if two groups are different from one another then a t-test would be a good option. This sets up a null hypothesis that the difference is zero and then if the p-value is small enough you can reject the null hypothesis and proudly stick an asterix over your boxplot and say your results are 'significant', meaning the two groups actually are different. A p-value is the probability of finding the observed or more extreme results given the null hypothesis is actually correct. When you think about it, it's a pretty uninformative thing to report. In addition, something that has troubled lots of people about null hypothesis significance testing (NHST), and something statisticians love telling everyone is that if you get a large p-value, you can't accepted your null hypothesis - you've just failed to reject it. Also, a small p-value gives no information on effect size. With large enough sample sizes you can get very small p-values and yet difference between the two groups be minuscule, and a t-test still doesn't give you a measure of how believable your conclusion is.

There is a statistical test that avoids all these problems and provides lots of useful information and I only found out about it the other week, it's called BEST (Bayesian Estimation Supercedes T-test). I won't go into details in this post as I'll probably get something wrong and the [original paper](http://www.indiana.edu/~kruschke/BEST/) is very readable, though I'll go through a demonstration of a simple experiment, comparing a t-test against the BEST model.

### A simple experiment

I have the hypothesis that drug A makes cancer cells shrivel up so I treated lots of cells with drug A and lots of cells with a control, and then measured their area from hundreds of microscope images.


**A t-test**

We can get a quick idea of what the data looks like with a histogram of cell area.

![cell area histogram](/images/2016-02-21-are-you-certain/Difference.png)

The standard response now would be to carry out a t-test and make a boxplot, so here's that...

![boxplot](/images/2016-02-21-are-you-certain/boxplot.png)

With a p value of \\( 2.66 \times 10^{-86} \\), this means that there's likely a difference between the two groups, but that's all the information the t-test actually provides.

**BEST**

If we carry out a BEST model on this data we don't just get single values we get probability distributions for the mean (\\( \mu \\)) and standard deviation (\\( \sigma \\)), these distributions represent the uncertainty behind our beliefs, with the width of the distribution proportional to our uncertainty.

If we were asked for the difference between the two groups, the typical response would be the difference between the two means or medians. However, since we're working with probability distributions in order to make our uncertainty clear, it's a bit pointless to sweep all that information regarding uncertainty under the rug. So for this we can calculate the difference between the two \\( \mu \\) distributions to produce another distribution in order to show the difference between the two.

![best](/images/2016-02-21-are-you-certain/bayes_cells.png)

### Is this the end of the t-test?

If the output of the BEST model is so much more informative than the t-test, why would anyone ever use the t-test again? Well the BEST model has some pretty obvious drawbacks. I can't image showing three sets of probability distributions to my supervisor for a simple 'is A bigger than B?' experiment is ever going to go down well. Secondly, a t-test can be carried out on pretty much any statistical software since the 80's and runs in the blink of an eye - whereas unless you're an R wielding statistician or want to code it yourself, the BEST model isn't all that easy to carry out, and can take a while to run for some larger problems.

So for me, it's pretty interesting and maybe I'll use it in some of my work in the future. I hope to see it implemented in some biology papers, though I'm probably going to keep my head down and stick with the more mundane methods.

### code

The code used to run the examples is available [here](https://gist.github.com/Swarchal/6d3658d9907c31f6d315).


-- Scott
