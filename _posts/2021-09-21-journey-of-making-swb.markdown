---
layout: post
title:  "Journey of Making Will Sh3 B33"
date:   2021-09-21 09:12:17 +0330
categories: interesting
---

Ok so, get this. I'm single! And have been all my life, except for a very brief window at 20. I always wondered "Why? Chera? Porquoi?" --- the answer is, I'm ugly, I'm not that smart, and I'm fat and I have the personality of a wet rag. I'm also bipolar and diabetic. No girl wants in this mess.

I wanted to affirm my suspicions. So what I did was I went to Kaggle, downloaded OkCupid profiles dataset, and got busy.

You can view the model deployed here ---> [Will Sh3 B33](https://willsh3b33.xyz)

My mundane journey will not shock, nor surprise you.

First of all, in the repo (linked in the website) you'll see two notebooks. My entire code for preparing these four models is in there.

The first problem I faced was NaN values. I first tried using Sklearn's `SimpleImputer` to fix this issue, but I decided to take another route. I simply changed them all to "Prefer Not to Say".

I then used 11 `LabelEncoder`s and 3 `MinMaxScaler`s to preprocess continouous, and discrete data.

After that, I installed Optuna, which is surprisingly not installd on Colab by default. I created two objective functions. One for an XGBoost model and one for an SVM model.

I was surprised that XGBoost had no contols over OVR vs. OVO, but maybe I'm misunderstanding how Gradient Boosting works? I chose softmax as loss function since I had 5 labels and ran 3000 trials. It was unnecessary since the 30th trial was the best one. I got the best trial, got the best model from it that I had saved using a trick I had learned from Stack Overflow, and saved it to Google Drive after evaluating  it.

I did the same with the SVM model. I chose OVR and tuned three parameters. The accuracy of both models was over 0.92 and the loss was sub 0.05.

Now I wanted to train a deep model. I used the same X as the shallow models, but One-Hot-Encoded the ys. Then I created a simple feed-forward model using Keras, created an early stopper, and got a great validation accuracy at the first epoch -- 93% -- the ealy stopper took effect so I  gave it the benefit of a dobut and saved the model.

Finally, I installed the transformers library, and used its BertTokenizer and BertModel to classify the 9 OkCupid essays rolled into one.

I then created a backend for it using Flask and a frontend for it using Vue. I bought a VPS for $10 with 6GBs of RAM and deployed it on it using Docker.

Everything I did, I had done before, save for frontend. I always sucked at frontend, but I decided to use Vue and Bootstrap-Vue and honestly, it was fun!

I hope you have fun using my model. Maybe one day I get to train a real model? :winky face:

Thanks, Chubak Bidpaa (Chubak#7400, [Resume](http://chubakbidpaa.com/resume/))