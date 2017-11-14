---
title: Is Amazon really as trustworthy as your bank?
subtitle: Never trust bold statements on distorted charts
date: 2017-11-14
photo-author-profile: thisisramiro
photo-author: Ramiro Mendes
header-url: https://images.unsplash.com/photo-1505166065723-bae088a12fc4?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1900&fit=crop&s=a8ab9df85c66a2a482f54d994fa73c7b
category: data
---

The Verge published an [interesting survey](https://www.theverge.com/2017/10/27/16550640/verge-tech-survey-amazon-facebook-google-twitter-popularity) a few weeks ago about the popularity of the big tech companies and the level of trust towards them. The topic is really exciting, especially when one of the top charts states no less than _’People trust Amazon almost as much as their bank’_.

![](https://cdn.vox-cdn.com/thumbor/5AwKVJmieBV5qP38fMSQ3_CwNAo=/1200x0/filters:no_upscale()/cdn.vox-cdn.com/uploads/chorus_asset/file/9547237/Amazon_01_01.jpg)

I know from personal experience this is the type of question that keeps banking executives awake during the night. Unfortunatelly the conclusion here is just as useless as it is bold, and suffers heavily from a handful of biases.

## Issue 1: Distorted visualisation

First, this visualisation shows nothing of the message in the title. The only information this chart can clearly convey is that banks had the highest rate of people who _did not contribute_ an answer (the rightmost yellow bars). All the other bars are disaligned and distorted by these meaningless answers and there are no labels to the rescue either.

I tried to decode the percentages by taking the pixel width of the bars (for the sake of simplicity I counted _’Greatly’/‘Somewhat distrust’_ categories together). It turns out that actually Microsoft had the highest rate of _’Greatly trust’_ answers in the survey, and Google seem to be ahead of Amazon in this aspect, too.

![](/img/posts/Screenshot 2017-11-14 10.32.52.png)

If we adjust the percentages for the people who did not have opinion, the actual ratios of (usable) opinions would look something like this:

![](/img/posts/Dashboard 1.png)

Amazon, Microsoft and Google has almost the same rate of trust (53%, 52%, 52%, respectively) amongst people who can decide whether they trust them or not, and they are visibly behind banks (58%) in this respect. Mind you, this adjusment has a tricky bias too, as it might selectively exclude some (but not all) answers of certain individuals, and would also decrease sample size (which in turn increases uncertainty).

## Issue 2: Too much noise, too weak signal

Uncertainty is a nasty beast, that eats survey results for breakfast. No matter how we try to fix the visualisaton, these percentages really mean nothing if we take into account the uncertainty of the survey (emphasis is mine):

> This survey, conducted from September 28th to October 10th, included 1,520 people nationally representative of the US, based on 2016 US Census estimates. __The margin of error is ±3 percent__, with a confidence level of 95 percent.

So the differences between the original percentages for Microsoft, Google and Amazon all fall in the margin of error, and things get even blurrier with my rudimentary adjustments, as I could easily miscalculated by as much as half or even one percentage point.

This all means one thing: based on this survey we actually _don’t know_ which of Amazon, Google, Microsoft or banks in general has the highest level of trust. The numbers for these companies are _virtually indistinguishable_ from each other.

## Issue 3: Confirmation bias

But for some reason the creator of the chart still wanted to single out Amazon as a contender for banks. Not Microsoft and not Google, even though they are likely in the same _’league of trust’_. Why?

We will never know for sure, but maybe because in the breakout report concerning [Amazon results](https://www.theverge.com/2017/10/27/16552614/amazon-popularity-user-survey-prime-echo-trust) also seem to deliver some positive messages on the company, and this chart had to fit in, even by misrepresenting the facts (not necessarily on purpose, this is just how our mind works).

## Do not trust surveys

The conclusion here is not that trust towards Amazon is not comparable to banks and not the opposite either. I don’t know anything about that. 

The takeaway is that we should not trust surveys packing blockbuster conclusions into distorted charts, based on questioning a handful of people on some mostly unconscious feelings.
