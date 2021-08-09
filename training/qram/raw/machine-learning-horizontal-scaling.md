---
title: Horizontal Scaling of Machine Learning Algorithms
description: 
published: true
date: 2021-07-30T00:07:55.433Z
tags: machine-learning, horizontal-scalability
editor: markdown
dateCreated: 2021-07-30T00:07:55.433Z
---

> TO BE EXTENDED
{.is-warning}

[Horizontal scaling](/training/qram/horizontal-scaling) or parallelization of different machine learning algorithms can drastically improve model efficiency. 

## Computational tasks that are easy to scale:
**[Cross-Validation using K-Folds](https://en.wikipedia.org/wiki/Cross-validation_(statistics)#/media/File:KfoldCV.gif)**

* K-Fold can be scaled across k nodes with each node training a model

**[Hyperparameter tuning with grid-search](https://en.wikipedia.org/wiki/Hyperparameter_optimization)**

* Each combination of hyperparameters can be trained in parallel

**Making predictions**
* After the model is trained and saved, each observation can be run through the model in parallel

**[Bootstrap Aggregation](https://en.wikipedia.org/wiki/Bootstrap_aggregating)**

**Training N trees with [random forest](https://en.wikipedia.org/wiki/Random_forest)**

* A random forest is a collection of decision trees, our weak learners, whose predictions are aggregated in the form of an ensemble. These N weak learners can be trained across N processors.

**Determining splitting criteria for tree-based algorithms**


## Computational tasks that are difficult/impossible to scale:
**[Gradient descent](https://en.wikipedia.org/wiki/Gradient_descent)**
* Gradient descent is an iterative approach to find the local minimum of a differential function, typical a [loss/cost function](https://en.wikipedia.org/wiki/Loss_function) hence minimizing error. IT is difficult to scale horizontally because at each iteration, it needs to consider the all the data. 
* Methods of speeding up GD generally involve taking a [subset of observations for each iteration](https://en.wikipedia.org/wiki/Stochastic_gradient_descent). 
* [Downpour SGD: Research](https://static.googleusercontent.com/media/research.google.com/en//archive/large_deep_networks_nips2012.pdf)

**Training batches in Neural Networks**
* Batch size represents the number of observations will be propagated through the network. This occurs iteratively.
* [Sandblaster L-BFGS: Research](https://static.googleusercontent.com/media/research.google.com/en//archive/large_deep_networks_nips2012.pdf)

**[Boosting](https://en.wikipedia.org/wiki/Boosting_(machine_learning))**

