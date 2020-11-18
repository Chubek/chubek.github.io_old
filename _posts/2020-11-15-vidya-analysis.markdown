---
layout: post
title:  "Videogames: A Data Study"
date:   2020-11-15 00:12:17 +0330
categories: video games
---

**NOTE**: This post contains large images that are readable upon zoom. Use Ctrl+Plus to zoom. After you're done reading, press Ctrl+0.

The videogame bubble in North America has burst twice. Once in 1977, only three years after Pong started the trend (Magnavox Oddessy is not at all what I consider a gaming console) and once in 1983. This data ignores all the second generation consoles except Atari 2600, and it ignores the premiere years of this console and begins at 1980. It's worth noting that this dataset ends at 2016. It can be found [here](https://www.kaggle.com/sidtwr/videogames-sales-dataset).

# One: Data Analysis

Let's start gumshoing in this dataset by comparing count of game sales based on genre across years.

![Genre by Year](/assets/img/top_genre_year.png)

There's nothing worth noting in this graph that we didn't already know. Action games have indeed taken over the majority of videogames. Europe and North American tastes are almost indentical. However, the Japanese seem to take extra joy in the role-playing genre. Another genre which Japan takes a slight lead in is the puzzle genre. The Japanese don't enjoy shooters at all. Having seen the trends in Japanese speedrunner community, it could be factual.


The next chart we're going to analyze is all about global sales and publishers. Guess who comes on top?

![Global Sales by Publisher](/assets/img/top10-publisher-globalsales.png)

One of these 10 is not like the other, and that's THQ. A publisher that's a former shadow of itself. This just goes to show how enormous THQ was at once.

Now let's do the same, but with top 10 platforms.

![Global Sales by Top 10 Platform](/assets/img/top10_plat_gsales.png)

Earlier platforms such as NES and Genesis are not to be found. Had I taken the weighted average of global sales, they would have appeared. 
This chart just confirms many people's beliefs that PS2, X360 and PS3 turned a corner in the videogame market. More people became gamers. Gaming went from a quasi-niche to a mainstream form of entertainment in the 2000s, so it's not false to assume that the consoles which were dominant in this decade also sold the most games.

Now let's see which platforms have the lowest sales.

![Lowest Sales in Platforms](/assets/img/b10_plat_gsales.png)

This chart shows that the lowest-selling "mainstream" consoles sold loads more than the "niche" consoles. I bet some of you are killing yourself for not knowing what these "niche" console name abbrevations satand for. So let me put your mind at east:

* SCD -> Sega CD
* NG -> Neo Geo
* WS -> Wonder Swan

The rest are obvious.

Another graph I've made shows the top publisher sales according to region:

![Regions Sold](/assets/img/regions_sold.png)

This just goes to show that Japan HATES Western publishers.



Now what I wanna do is to analyze the critic scores, bottom 10 critic scores, based on publisher. Why am I not examining top critic scores? Because there's little variety in top scores, and we all know which publishers have the highest-rated games. So let's let's see which publishers have the lowest *mean* critic score?

![Bottom Mean Critic Score based on Publisher](/assets/img/meancriticscore_bot10.png)

But who is "Popcorn Arcade", which is appearantly the worst publisher ever, based on hard data?

According to Moby Games:

> Popcorn Arcade is a Wii publishing label of Data Design Interactive and parent company Green Solutions Ltd., established in August 2007. It is a re-branding of Metro3D Europe Ltd. that published games for different platforms.

Their games include:

* Urban Extreme: Street Rage	(2008)
* Hamster Heroes	(2008)
* London Taxi: Rush Hour	(2008)
* Kawasaki Jet Ski	(2008)
* Rig Racer 2	(2008)
* Kawasaki Quad Bikes	(2008)
* Myth Makers: Orbs of Doom	(2008)
* Kawasaki Snowmobiles	(2008)
* Myth Makers: Trixie in Toyland	(2008)
* Billy the Wizard: Rocket Broomstick Racing	(2007)
* Anubis II	(2007)
* Classic British Motor Racing	(2007)
* Action Girlz Racing	(2007)

People into videogames will definitely recognize Anubis II. This game was a reskin of a reskin of a reskin. It was basically shovelware based on a Zool remake that never got published.

But which genre has the highest mean critic score? 

![Mean Critic Score based on Genre](/assets/img/mean_score_genre.png)

it's a close call, but it seems like role-playing games are the highest rated. Rest of the tiers are extremely close to call.

Let's analyze age ratings, specificly ESRB ratings, through 2000s and 2010s.

![Ratings based on Year](/assets/img/rating_year.png)

The dominant rating seems to be E, however, there seems to be a drop in games rated for Everyone in the 2010-2015 epoch. Also there seems to be a lot more M rated games in 2005-2010.

Graphs from now on are going to be stacked, and extremely multivariate. So prepare yourself for some complex charts. 

First one I'm going to show you is the Top 10 Publisher-Developer-Global Sales-Genre chart. You may hate stacked charts but you can deny that they indicate more info than their baggage of reding.

![Top 10 Publisher - Genre - Global Sales - Developer](/assets/img/t10_pub_dev_gs_gen.png)

First off, this chart shows that most publishers hire developers to do sports games. It also affirms that EA Sports is the biggest sports publisher, with Visual Concepts being the smallest. Visual Concepts is now operating under Take-Two Interactive, as it is evident in the graph, and they make franchises such as WWE and NBA. The rest of the sports developers are various EA Studioes.

I'll let you analyze the rest, but note at how Rockstart North is the biggest Action game developer in terms of global sales. I'm not sure if you've heard of them but they once made a game called Body Harvest.

Now another stacked chart. Genre vs Year. Years are grouped in terms of five-year periods, with each single year being colored.

![Year Genre](/assets/img/genre_year.png)

What this graph shows is a spike on the covariance of number of games in 2010-2015 epoch. Plus, some it really shows that a lot of genres took their root in 1990-95 epoch, with them not appearing in previous periods. Again, it shows that sports and action games have always been popular.

Now another graph, genre vs platform grouped by generation:

![Genre Platform in each Generation](/assets/img/genres_plat.png)

I admit, when I made this chart, I forgot to remove the label reserved for unknown publishers which the person who scraped the data put in not to break his loops. I apologize.

In early games, early being Atari 2600 and some NES, shooter and sports games took the helm. There was no sumulation, strategy or role-playing games back then.
Most games in the 8-bit era are NES and WonderSwan. We see a surge of RPGs here. There's just too many RGPs in 16-bit era. I've also made 2000s handheld and PC games a separate category.

We've had enough bar charts. Let's take a look at a few scatterplots. These are much easier to grasp

The first one I wanna show is Global Sales vs. Critic Score:

![Global Salse Critic Score](/assets/img/gsaels_cs.png)

I'm honestly not surprised by this chart. One WOULD expect the distribution to be normal (as in, increase-increase or decrease-decrease) but this is just... cathardic. It shows that people don't even care about what criticism is pointed at a game. It's a hectic chart that shows little to no correlation.

Same with Japan Sales vs. Critic Score:

![Japan Sale vs. Critic Score](/assets/img/jpsales_cs.png)

Well, there are more outliers in this graph, but the distribution remains the same.

Now, let's predict some stuff.

# Two: Predictions

We have sufficient data to train a model that predicts numerical, or even categorical values that we want. I trained some models with careful feature selection, but only one of them seemed to be accurate enough. And that model was:

Training features:

* Year
* Genre
* Publisher
* Global Sales

Target:

* Critic Score

I used bagging, Random Forest to be exact, and I did a grid search with various hyperparameters to find the right settings for the model. Mean Absolute Error of this model was 8.5, rounded up to the nearest half (do people do that?).

Generating a chart for all the publishers at the same time would be awesome, but with current technologies, I'll have to generate predictions separately. I'll choose future of 2021-2031, and only choose publishers who develop games across all spectrums. Remember, the trend is the same across all predictions, and it looks slightly bleak. However, what's different is the outcome.

Let's look at Nintendo's:

![Nintendo Prediction](/assets/img/nintendo_cscore_pred.png)

What we see here is that Nintendo probably won't preform well in most genres. Just joking, if Nintendo is not performing well, it means there wasn't enough data for that certain genre. Same with most of these charts. I just got the predictions for all the genres to be safe. It's obvious that some publishers perform better in their own specialty.

Let's Ubi's:

![Ubisoft Prediction](/assets/img/ubisoft_pred.png)

Uh. Ubi is known for making garbage, and the fact that their shooters seem to be so high-scored makes me realize this model needs much more work (I don't deny that this model is the lowest common denominator of models).

Valve's outlook isn't changing, considering that they don't even make games in the genres where the outcome is low:

![Valve Prediction](/assets/img/valve_pred.png)

Same for Activision:

![Activision Prediction](/assets/img/acti_pred.png)


And EA will do well in sports, as it always does:

![EA Prediction](/assets/img/ea_predict.png)

One thing, the last genre is "nan" or the garbage category so don't mind it not being labeled.

If I wish to make better predictions in the near future, I'll need to do much, much more careful feature selection using Chi Squared Score for the categorical features and ANOVA for the numerical ones. I barely did some ANOVA on Global Sales and I determined that I need to include it. Otherwise I believe some of these training features are completely irrelevant.


Another thing I did was running a Bayesian GLM (Generalzied Linear Model) on the Critic Score and Global Sales metric. Bayesian GLMs are like Linear Regression, but they run on Bayesian Probablities, and not Frequentist Probabilites.

![Critic Score GLM](/assets/img/global_sales-cscore_posterior.png)

Looking at the scatterplot of Global Sales vs. Critic Score, this graph is wishful thinking.


# Three: Some FUN!

Using a Markov Chain heuristic (note: not an algorithm, like the one sklearn uses, just a small heuristic) I made a probablistic generative model that creates video game titles of various lengths. As it is not a sophisticated algorithm, I don't expect to generate meaningful names. They're more risible than convey any point.

Let's see some of these titles:

* Duty: Battle Edge (seed: Duty)
* MarioWare II NCAA (seed: Mario)
* Sonicles The Amused (seed: Sonic)
* Crash of Hono Shi (seed: Crash)
* Assassing Storm II (seed: Assassin)
* Honor: Wanted Cup (seed: Honor)
* Soccer 2016 Dream Nine (seed: Soccer)
* Captain Megamind (seed: Captain)
* Blast War: Warfight (seed: Blast)
* Baby Star Hero Grand (seed: Baby)
* Childish: Midway Dream (Childish Gambino used one of these to come up with his stage name)


Enough for now.

**About the Author**: Chubak read his first Machine Learning book in 2016, and wrote a Classifier microlibrary. He's currently a freelance data scientist. if you wish to contact him, his Discord is Chubak#7400 and his email chubakbidpaa@gmail.com