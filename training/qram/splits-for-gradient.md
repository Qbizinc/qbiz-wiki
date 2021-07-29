---
title: Optimal Splits for Gradient Boosted Trees
description: 
published: true
date: 2021-07-29T23:53:13.810Z
tags: machine-learning, decision-trees, gradient-boosting
editor: markdown
dateCreated: 2021-07-29T23:49:54.002Z
---

> TO BE COMPLETED
{.is-warning}


This piece of content aims to detail the algorithms popular gradient boosting frameworks find the most optimal splits of data when forming decision trees. This is in contrast to splitting criteria, which tends to take the shape of either [gini impurity](https://en.wikipedia.org/wiki/Decision_tree_learning#Gini_impurity) or [information gain/entropy](https://en.wikipedia.org/wiki/Decision_tree_learning#Information_gain). 

Gradient Boosting Decision Tree
* Considers every possible split

XGBoost 
* Pre-sorted Algorithm
-- iteration over sorted enumerated splits for all feature values
* Histogram-based Algorithm
-- segments feature values into bins 
LightGBM
* Gradient Based One Side Sampling (GOSS)
-- downsamples observations with small gradients

## Comparison (TBD)

| All potential Splits | Pre-sorted Algorithm | Histogram-based Algorithm | GOSS |
| --- | --- | --- | --- |
| TBD | TBD | TBD | TBD |