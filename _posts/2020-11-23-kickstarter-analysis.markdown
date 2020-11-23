---
layout: post
title:  "Kickstarters: An Analysis"
date:   2020-11-23 15:22:17 +0330
categories: analysis
---
Dataset can be found [here](https://www.kaggle.com/kemical/kickstarter-projects).

# One: Analysis

First, let's see just how many backers projects rising from each country gather.

![Backers vs. Country](/assets/img/backers-country.png)

Surprisingly, projects from the US don't have most of the backers. Rather, the title goes for Hong Kong.

Whilst we're on backers, let's dissect the histogram of backers for successful projects with a "logical" goal.

![Backers Histogram](/assets/img/backers-goal-successful.png)

As you can clearly see, the histogram is skewed to the left. Meaning projects with smaller amounts of backers are more prevalent.

Now let's pit backers against categories.

![Backers vs. Categories](/assets/img/backers-vs-category.png)

Let's jump from backers to something else. The following chart shows the days between goal date and start date of canceled projects.

![Canceled Days](/assets/img/cancelled-delta-vs-category.png)

I just have one stacked chart for this post, and that's country-category-pledged-state.

![Country Category Pledged State](/assets/img/country-category-pledged-state.png)

I have another histogram for you, the logical goals.

![Logical Goals Histogram](/assets/img/logical-goals-hist.png)

Now let's do pledged vs. state of the project.

![Pledged vs. State](/assets/img/pledged-vs-state.png)

Another histogram, this time, pledges. Another skewed histogram.

![Pledged Histogram](/assets/img/pledges-goal-hist.png)

Finally, I have three scatterplots, two being regressions.

First, the one that isn't a regression. It basically shows the real pledge value, as in, adjusted by inflation vs. the pledged value.

![Real Pledged vs. Pledged](/assets/img/real-pledge-vs-usd-pledge.png)

Now, I trained a Stochastic Gradient Descent Regressor on these data:

* Category - Backers -> Pledged
* Category - Goals -> Pledged

Here's the first regression plot:

![Pledges vs. Backers Regression](/assets/img/pledged-vs-backers-reg.png)

A regression with normal distribution. Fine. But the next one is just unmaintainable.

![Pledge vs. Backers Regression](/assets/img/goal-pledge-reg.png)


Well I hope this article was fun and informative.

**About the Author**: Chubak Bidpaa is a data programmer, specializing in the wide area of web spiders, machine learning, and deep learning. You can contact him on Discord with Chubak#7400 and email chubakbidpaa@gmail.com.

