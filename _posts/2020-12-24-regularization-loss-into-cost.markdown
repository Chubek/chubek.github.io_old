---
layout: post
title:  "Regularization or How I Turned Loss into Cost"
date:   2020-12-24 09:12:17 +0330
categories: regression
---
We know that in order to train a regression model, with a continuous target or a set of labels (i.e. logistic regression) --- we need to solve the *Empricial risk minimization* problem. One of the ways to solve the ERM is least-squares method, explained [here](https://cross-validated.fandom.com/wiki/Least-Squares_Linear_Regression) by yours truly. I have also implemented least-squares regression in Python [here](https://github.com/Chubek/TheMLCodexCode/blob/main/chapter_01/simple_least_squares_reg.py) and it rust [here](https://github.com/Chubek/RustMLEtudes/blob/main/src/regression.rs).

This method may cause *overfitting*. Overfitting is basically when the model is *too generalized* and is too confident in itself. Overfit models have an extremely high variance. A high variance is what we should aim at, but not *too high*.

To battle this, methods such as K-Fold Cross Validation have been proposed. But perhaps the best thing we can do is to *regularize* the loss function. This paradigm is known as *Regularized Loss Minimization*. In RLM we minimize the sum of empirical risk through a regularization function which we call the *cost function*. In other words, regularization is an *stabilizer* of the loss function. 

## Definition

RLM is a learning rule that joins ERM (Empirical risk minimization) with a regularization function. Formally, a regularization function is defined as:

![Regularization Function](/assets/latex/regfunc.png)

We are looking for the minimum *argument* that is **w** here. **w** is basically the differential of the features and the target/label --- in other words, the *slope* of the best-fit line (in linear functions, differential/derivative is the slope). L here is the loss function, and R is the regularization function.

There are many regularization functions. In this post, we're going to focus on a type of regularization which we call *Tikhonov regularization* or *L2 regularization*.

## L2 or Tikhonov Regularization

Tikhonov regularization has a *penalty* which is denoted by Greek letter Lambda. This is multiplied by the *L2 norm* of the weight. L2 norm is often called *Euclidean distance*. We talked about L1 and L2 nrom extensively in [this](https://chubek.github.io/rust/2020/12/18/rust-recommender-p1-copy-2.html) post.

As such, the L2 cost function is hypothesized as:

![L2 Regularization](/assets/latex/l2reg.png)

In the following sections we're going to see how L2 regularization is applied in two places: Linear Regression, with the loss function of MSE, and Logistic Regression, with the loss function of Cross Entropy.

## Ridge Regression

Applying RLM rule with Tikhonov regularization o linear regression with squared loss (MSE) we get the *cost function* of:

![Ridge Cost](/assets/latex/ridge_cost.png)

This is called *Ridge Regression*. But just *why* does this so-called loss function stop overfitting?

Intuitively, a learning algorithm is stable if a small change of the input to the algorithm does not change the output that much. Of course, there are many ways to define what we call a *small change in the input* or *doesn't change the out put much*. If you wanna see a good mathematical proof on why a stable cost function doesn't overfit, refer to [*Understanding Machine Learning*](https://www.amazon.com/Understanding-Machine-Learning-Theory-Algorithms/dp/1107057132) pages 138-140.

## Regularized Cross Entropy

If we apply L2 regularization to Corss Entropy we get:

![Regularized Cross Entropy](/assets/latex/crossentropy_reg.png)

In APIs such as Scikit-learn, Logistic Regression comes with regularization by default. The *hyperparameter* for it is often called `C`. 

# Fidning the Optimal Penalty

You can obtain the optimal penalty by either doing a Cross-Validated Grid Search, or a Cross-Validated Random Search.

In Grid Search, we basically give the algorithm a list of hyperparameters, for the penalty, and for other hyperparams as well. The GridSearchCV algorithm pairs them up. It could get very dimensional, even if the algorithm reduces the dimensionality using Principle Component Analysis. A better, faster method is *Random Search* in which you feed it a list of hyperparameters, The RandomSearchCV then randomly pairs them up. You finally get the optimal model with the least errors. 

I recommend you implement RandomSearchCV yourselves using Python's `random.choice` function.


