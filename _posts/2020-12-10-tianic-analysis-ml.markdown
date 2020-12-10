---
layout: post
title:  "Titanic: Analysis and Predictions"
date:   2020-12-10 09:12:17 +0330
categories: classification
---
The story of this dataset is a weird one. It dates back to the earlier days of Kaggle, when the site was mainly designed for competitions. The goal of the competition was to predict if a passenger survives based on the features in the dataset. The main features of this dataset are:

* Survived
* Sex
* Class
* Age

Let's get the graphs out of the way first. Histograms show the distribution of a certain range of data. Here are the histograms for age:

![Age Histogram](/assets/img/age_hist.png)

Basically, there were much more younger people in on the ship.

The next histogram shows the distibution of cabin classes:

![Cabin Class Histogram](/assets/img/class-hist.png)

Ok. This next graph is going to be extremely controversial. We all knwo that pie charts are bad and imprecise, but with only two categories, I can't help it!

![Died-Survived Pie Chart](/assets/img/died-survived.png)

And finally, gender of those who died, and survived.

![Died-Survived Sex](/assets/img/died-survived-male-female.png)

Now let's focus on the machine learning part. I'm going to use two features, Sex and Class, to predict whether the passenger will survive. The accuracy scre used is F1. Both for the grid search, and the evaluation.

Here's the results:

| Model    | Preceptron                                                                                                              | SGDClassifier                                                                 | Feed-Forward Deep Network                    | SVC                                         |
|----------|-------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|----------------------------------------------|---------------------------------------------|
| F1 Score | 0.3684210526315789                                                                                                      | 0.6296296296296297                                                            | 0.7666666507720947                           | 0.7692307692307692                          |
| Params   | {'alpha': 0.0001,  'early_stopping': False,  'fit_intercept': True, 'max_iter': 100,  'penalty': 'l1', 'shuffle': True} | {'average': True,  'eta0': 0.01,  'learning_rate': 'optimal',  'loss': 'log'} | {loss: 'binary_crossentropy' optimizer: rms} | {'C': 1,  'degree': 1,  'kernel': 'linear'} |