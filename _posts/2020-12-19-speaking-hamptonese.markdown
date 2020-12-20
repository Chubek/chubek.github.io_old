---
layout: post
title:  "Hamptonese: Ramblings of Outsider Madman James Hampton"
date:   2020-12-18 09:12:17 +0330
categories: interesting
---

![James Hampton](/assets/img/jhampton.png)


Ever since I started writing my own book on implementing machine learning algorithms with mathematical context, I've gained a newfound respect for people who put the effort in expressing themselves creatively at any cost.

In the modern world, anyone has a digital device in their pocket with which they are just taps away from being creative. In the past, however, you had to go lengths to be able to express yourself properly and more importantly, be seen for your work. You had to shadow a master, be part of an elite, or go to art school.

James Hampton was just a janitor at a hospital. He made a 190 piece work of art from *garbage*, yes, garbage, he found around the city. This throne is a religious work of art, with Biblical references galore. It's one of the most famous pieces of outsider art, and is taken care of and is saught after to be displayed more  than the Mona Lisa. Hampton didn't show his work to anyone, except a few people, whilst he was alive. And his work was never finished.

![James Hampton](/assets/img/throne.png)

Along with his throne, a few notebooks were found. These notebooks were in bad shape, and they were written in a strange script. People tried to decode and unencrypt it for years. They though it's just English in a different script. But it wasn't.

unbeknownst to them, years later, two computer scientists by the names of Mark Stamp and Ethan Lee wrote a [paper](http://www.cs.sjsu.edu/faculty/stamp/Hampton/papers/hamptonese.pdf) in which they reached the conclusion that this script is *equivical to speaking in tongues* and the language was called *Hamptonese*.


![Hamptonese](/assets/img/hamptonese.png)


Each page in these notebooks are denoted by *St. James* --- perhaps he saw himself as a saint, and it becomes much more clearer and easier to transcribe by others as it progresses. The authors are not clear on how they've transcribed the language, but perhaps they used an indexing method.


First, they started by taking the *Entropy* of Hamptonese. **I wrote a profile on Shannon Entropy in my book, if you want to read a draft, refer [here](https://discord.gg/5uXBJvMM96)**. Basically, entropy is the amount of the *surprise* we get after finding out the value of each random variable. In other words, entropy is the amount of information gained from a set of random variables.

The authors arrived at these values:

![Entropy of Hamptonese](/assets/img/entropy_hamp.png)

As you can seem the groups of strings containing 3 different symbols and their difference with the entropy of three symbols in the Latin alphabet. Authors suffice that this does not prove that Hamptonese is a language, but is enough to go on with further analysis.

Then the authors try and calculate the hidden state of each set of strings using *Hidden Markov Models*. HMMs are an augmentation on the Markov chains. A Markov chain is a model that tells us *something* bout the probablities of sequences of random variables, in other words, *states*, eacho f which can take on values from some set. A Markov chain makes the assumption that if we want to predict the future in the sequences, all that matters is the current state, and the states before the current state have no impact unless if they are *chained* to the current state. They are best used in random word generators.

However, sometimes the events we are interested in are *hidden*, and we don't observe them directly. For example, we see words, adn must infer the tags from the word sequence. We call the tags *hidden* because they are not *observed*. An HMM allows us to connect hidden events with the observed events. It has a transition matrix, or transmat, which represents the probablityo f moving from state i to state j. An initial probablity distribution, likelihood of observations, and observations. 

These are the results of two English states in script, vowel and constanant:

![Entropy of Hamptonese](/assets/img/hmm.png)

And these are the results of two Hamptonese states, the *hidden states*:

![Entropy of Hamptonese](/assets/img/hmm_hamptonese.png)



Results: WE CAN'T BREAK THE CODE!

What is this langauge? What does it mean? What did HE mean? Was he inspired by God? Was he mentally ill? Was he an schizo?

Maybe we'll never know!