---
title: "Machine Learning Introduction"
date: 2020-07-02T11:33:15-04:00
categories:
    - machine learning
tags:
    - overview
    - supervised learning
    - unsupervised learning
draft: false
---

## What is Machine Learning?

There exist two definitions of Machine Learning, the older one describes ML as

> The field of study that gives computers the ability to learn without being explicitly programmed.
>
> Arthur Samuel

The more modern definition of ML is 

> A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E.
>
>Tom Mitchell

An example for the latter definition that is used in the Coursera [Machine Learning](https://www.coursera.org/learn/machine-learning) course is playing checkers:

- "E" is the experience of playing many games of checkers
- "T" is the task of playing checkers
- "P" is the probability that the program will win the next game


## Types of Learning Algorithms

Any machine learning problem can be assigned to one of two broad classifications:

- [Supervised learning](https://en.wikipedia.org/wiki/Supervised_learning)
- [Unsupervised learning](https://en.wikipedia.org/wiki/Unsupervised_learning)

But there are also [more specific classifications](https://en.wikipedia.org/wiki/Machine_learning):

- Semi-supervised learning
- Reinforcement learning
- Feature learning
- Sparse dictionary learning
- Anomaly detection
- Robot learning
- Association rile learning

### Supervised Learning

In supervised learning, we are given a data set and already know what our output should look like. We are using so called labeled training data consisting of a set of training examples. The supervised learning algorithm analyses the training data and produces a function that can be applied for mapping new examples. This algorithm has to generalize from the training data to unseen situations in a "reasonable" way.

Supervised learning algorithms are categorized into **regression** and **classification** problems. In regression problems, we are trying to predict results within a continuous output. An example is to predict the age of a person on the basis of a given picture. A second example is predicting house prices.

In classification problems, we are trying to predict results in a discrete output. An example is to predict whether a tumor is malignant or benign if a patient has a tumor. Another example is if a given email is spam or non-spam.

> **Features**: An individual and measurable property or characteristic of a phenomenon being observed. The features are the input for the learning algorithm.
>
> Example: In the case of the house price prediction example the number of bedrooms, size, age of the building, and so on, are features.

### Unsupervised Learning

Unsupervised learning looks for undetected patterns in a data set with no pre-existing labels and with a minimum of human supervision. The structure can be derived where the effect of the variables is not necessarily known. We can derive the structure by clustering the data based on relationships among the variables in the data. With unsupervised learning there is no feedback based on the prediction results

> **Labels**: The label is the result or output after the learning algorithm was fed with the features.

An example for **clustering** is to take a collection of 1,000,000 different genes, and find a way to automatically group these genes into groups that are somehow similar or related by different variables, such as lifespan, location, and so on. Another example is [Google News](https://news.google.com). Google News looks thousands of stories and cluster them together, so that all stories are about the same topic.

The _Cocktail Party Algorithm_ is an example for **non-clustering** and allows to find structure in a chaotic environment, like identifying individual voices and music from a mesh of sounds at a [party](https://en.wikipedia.org/wiki/Cocktail_party_effec).



## Models

Performing machine learing involves creating a model. This model is trained on training data and then the produced function can be used to process new data to make predictions. There are various type of models: 

- Artificial neural networks
- Decision trees
- Support vector machines
- Regression analysis
- Bayesian networks
- Genetic algorithms

## Links and Resources

- [Wikipedia: Machine Learning](https://en.wikipedia.org/wiki/Machine_learning)