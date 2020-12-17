---
layout: post
title:  "Solving the Cold Start Problem in Recommender Systems"
date:   2020-12-15 09:12:17 +0330
categories: recommender
---

Recently, Kim Falk released a book through Manning publication which explains Recommender systems in detail. One thing he discussed which caught my eye was the *Cold Start* problem. But what is it and how do we solve it?

## How do recommender systems work?

There are four types of recommender systems:

1- Those that use *collaborative filtering*, as in, other user's data compared to yours, to recommend new items.
2- Those that use *content-based filtering*, as in, your own data and histroy, to recommend new items.
3- Those that use both, they're called *hybrid systems*.
4- Ones that use IP, or self-entered location, and use geographic and demograpgic similarties to recommend new items to the user.

They achieve results by measuring the *similarity* of the scores, ratings, visits, posts, etc with others. Cosine similarty and L2 norm are the most used similarty functions in recommender systems. Then data is put into a feature matrix, and regression is used to calculate the future score. Then this value is used to classify the data. I would personally use Gini impurity. 

Recommender systems are new. You're free to do whatever you want in it to solve problems that you face. One of these problems is the cold start problem.

![Recommender system types](/assets/img/rec.png)
 
 ## What is cold start?

 We have a lot of data on exisiting items and users, but what about new items and users? How do we recommend new items for the buyers of new items, and new movies to users who've just reigstered?

 That's just tip of the iceberg (speaking of icebergs, read my post on Titanic survivors [here](https://chubek.github.io/classification/2020/12/10/tianic-analysis-ml-copy.html)). Another issue we face are what Kim calls *grey sheeps*. These are the users with no discerning taste, or extremely niche items. You can't really match recommendations for these without going the extra mile.

Another issue are guest visitors. Our client may ask use to recommend stuff to guest visitors. Guest visitors, by default, are cold. 

So to sum it up, we have:

1- Cold visitors
2- Cold items
3- Cold users
4- Grey sheeps

Let's solve these in order.

## Solving the problem of cold visitors

Earlier we said that we can use IPs to do filtering, it's a part of what's called *demographic filtering*. With a normal user, we ask wher the user is from ,s odemographic filtering can be done more precisely. But with a cold visitor, they could be using VPNs. Not everyone has an static IP. For me, personally, an static IP cossts a lot. Then again I'm from one cesspit of a country!

We can create a cookie in users' browser to keep track of their activity, and recommend stuff based on that activiy. We can mostly do demographic and content-based filtering, but we can also use collaborative filtering. The way we do this is to assume that a visitor is just an anonymous user. To cold start the user, we can start with demogaphic filtering and slowly shift to content filtering. At the end it comes down what you, and your client want.

## Solving the problem of cold items

Youtubers will love this part.

Items need similar items. Otherwise there would be no flow in the site. When a new item arrives, there's no history of *who* purchased this product, watched it, or rated it. So what we need to do is to manually promote this product on the site's front page and on the interim, display items that are similar to it in name and properties only. This is cheating but there's no other way.

We can use string similarty algorithms, from a simple Levenstein distance, Hammind distance, to a full-fledged BERT model and shamelessly promote the product until enough users have rated it or bough it. 

This is a problem that a lot of Youtubers face. They call it *the algorithm* but in reality, it's a small part of Youtube's recommender algorithm. In the old days, Youtuve manually added videos to the front page. However, these days Youtube is entirely reliant on the interaction of a channel's subscribers' with the video to cold start a video. This is why most Youtubers are so adamant that you subscribe to them, and 'push the bell'. Pushing the bell is equal to pushing new videos to your feed regardless of its states. Youtube uses string similarties on the sidebar to recommend "similar videos". Videos that are not collaborative, not content-based. They're for the more adeventerous people who want to watch something similar, yet different. These are all assertions though. Youtbube is silent about their recommender system, unlike Amazon and Netflix. Because it can be gamed, and abused.

This is my anthithesis to the *Youtube grind*. I propose that the number of videos you post doesn't matter to your channel being recommended at all on the Trending page or the Feed at all. If we reverse-engineer Youtube's recommender system, we can game the system, a new channel could possibly reach the Trending. The reason for the 'grind' is the trial-and-error nature of battling Youtube's recommender system. You make 10 videos a week, ~4 could potentially become popular through recommendation.

## Solving the problem of cold users

This is a problem that strictly deals with content filtering, and to some extent, collaborative filtering. Content-based recommendation can be renamed to *user-based recommendation* because it only deals with a user's taste. Problem with new users are that we have no info on them so we don't know what to recommend them.

Solutions for a cold users are various. One of them is to have the user login with their Facebook or Google account, and ask for their info from Facebook's Graph API and OAuth's various scopes. This is pontially very invasive.

Another solution is to have the user complete a survey before registering. Here, we need to classify users into super labels based on their answer to these questions, and whenever the user answer these questions, he'll be classified as one of those labels, and get the label's recommendations until they gget their ow recommendations.

What Reddit does is amix of the two. It uses the user's browsing history to get a good glimpose into their psyche, then recommends the user subreddits based on it. Whatver user seelcts is used for cold starting their personalized feed.

Another solution is demographic's filtering based solely on the user's IP. A lazy way is to not just recommendations until they interact! Hey, we're brainstorming here aren't we?

## Solving the problem of grey sheep

Basically, a grey sheep's recommendation matrix is a sparse matrix. Sparse matrice are converted to normal matrices using accumulative sum. This is one possible solution.

Another solution is to change how we categorize items. For example, we should categorize by art, not by artist. For example, instead of giving the user works of Tolkein, we give him books that are similar to The HObbit. This way if the user only liks an obscure fantasy book, he'll get recommendation for similar works.

![Recommender system types](/assets/img/greysheep.png)

***


Alright. I hope this hs been a sufficient introduction to recommender systems. Keep safe!
