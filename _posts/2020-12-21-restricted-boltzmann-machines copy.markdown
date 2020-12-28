---
layout: post
title:  "A Simple Introduction to Restricted Boltzmann Machines"
date:   2020-12-21 09:12:17 +0330
categories: interesting
---

Generation of new data has many uses in machine learning. One is augmentation and impution of data for more precision. There are many other aesthetic and pragmatic uses as well. Take "Face App" for example. It took the world by storm last year, and what it simply did was using GANs to change your face.

GANs are HOT HOT HOT topics in the world of Deep Learning these days. If Deep learning is a high school in Beverly Hills, GANs are the rich Persian girl who owns a BMW and parties til dawn. 

But the wallflower of this high school are the Restricted Boltzmann Machines. RBMs are old technology, but they were one of the earliest Deep Probabilistic Generative models that were proposed in 1986. However, it was extremely early for a generative model that's entirely reliant on the volume of data we give it. Simply because there wasn't enough data. So for the next 30 years, people just used simple generative algorithms such as Markov chains, which don't need a lot of data.

RBMs are usually trained using the *contrastive divergence* learning procedure. This method has a lot of hyperparams such as learning rate, momentum, weight-cost, sparsity target, initial values of the wieghts, and the number of hidden units --- and size of each mini-batch.

Oy vey! We can see why nobody touched them for a long time. They're basically deep models that rely on *probability* instead of *threshold*. They're like a deep network of logistic regressors. 

### Definition

Consider a training set of binary vectors which we will assume are binary images for the purposes of explanation. The training set can be modeled using a two-layer network which we call *Restricted Boltzmann Machine* in which stochastic, binary pixels are connected to stochastic, binary feature detectors using symmetrically weighted connections. These pixels correspond to *visible* units of the RBM because their states are observed; the feature detectors correspond to *hidden* units. A joiunt configuration, (v, h) of the visible and hidden units have the *energy*:

![Energy](/assets/latex/energy_rbm.png)


Where vi, hj are the binary states of visible unit i and hidden unit j. aibj are their biases and wij is their weight. Te network assigns a *probablity* to every possible pair of visible and a hidden vector via this function:

![Energy](/assets/latex/rbm_prob.png)

The probability that the network assigns to a training image can be raised by adjusting the weights and biases to lower the energy of that image and to raise the energy of other images. Then the *partial derivative* --- or slight rate of change, of the **log of the probability function** is used as loss function to get the *gradient descent*  --- or *descent to minimum error* --- basically, this done stochastically --- what we call **stochastic gradient descent**. We thus get the *best weights*.

This is basically *backpropogation* that we use in modern deep learning. This process is called *Contrastive Divergence* --- It's called that because it's the difference between two [*Kullback-Liebler*](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence) divergences. 

Just like a Feed-Forward Network, RBMs use the Logistic Function to classify the visible units. By classifying the visible units, and using the aforementioned energy function, we update the hidden units. These hidden units give us the newly-generated values.

So what do we learn from this? That RBMs are just a Feed-Forward Neural Network which uses probability isntead of thresholds. By composing them graphically, we get a *Deep Belief Network*. 

Deep Belief Networks are *graphical*. They are similar to Graph-based GANs. In fact, it's safe to say that a Graph-based GAN is a Deep Belief Network. Deep Belief Networks are used in Image Recognition and Motion Capture.

**I am writing a book on Machine Learning. Download the latest draft from my [Discord server](https://discord.gg/5uXBJvMM96)**