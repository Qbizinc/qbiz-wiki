---
title: K-Nearest Neighbors
description: 
published: true
date: 2021-07-29T19:14:13.761Z
tags: qram, machine-learning, unsupervised-learning
editor: markdown
dateCreated: 2021-07-29T19:12:58.453Z
---

# K-Nearest Neighbors Algorithm
﻿[K-Nearest Neighbors](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm) (KNN) is an unsupervised machine learning algorithm that makes predictions on a new data point based on the values of the k closest points in space. 

**Classification**: New data point is classified based on vote of k closest points.

**Regression**: Output of new data point is based on average of k closest points.

## Algorithm:
*inputs*: vector of points, new unclassified point, number of neighbors (k)

*for* each point in vector of points:
   
* *store* distance between new unclassified point and each point
   
*return*: vote or average based on k points with lowest distances

## Python Example:

```
import math

# columns = ['x1', 'x2', 'label']
vector = [[1, 2, 'Red'],
		 [-3, 8, 'Blue'],
		 [-1, 5, 'Blue'],
		 [-1, -4, 'Green'],
		 [-8, -2, 'Green'],
		 [-10, -1, 'Green'],
		 [10, 3, 'Red']]


def distance(data, point):
	# ex. euclidean
	euclidean = []
	for datum in data:
		partial_sq_dist = 0
		for i, coordinate in enumerate(datum[:len(datum)-1]):
			partial_sq_dist += (point[i] - coordinate)**2

		euclidean.append([math.sqrt(partial_sq_dist), datum[-1]])
			
	return euclidean


def vote(distances, k):
	neighbors = sorted(distances, key=lambda x: x[0])[:k]

	counter = {}
	for distance, category in neighbors:
		if category in counter:
			counter[category] += 1
		else:
			counter[category] = 1

	classes = []
	top_count = 0
	for category, count in counter.items():
		if count > top_count:
			classes = [category]
			top_count = count
		elif count == top_count:
			classes.append(category)

	return classes


def knn(data, new_point, k):
	return vote(distance(data, new_point), k)

```
