---
layout: post
title: "Normalisation VS Standardisation"
date: 2020-05-01
---

Why do we need to scale data?

To transform data to comparable scales (instead of having a variable ranging from 0 to 1 versus one ranging from 0 to 1000).

Distance algorithms (KNN, K-Means, SVM) are most affected by the range of features, because they depend on distances between data points.

Gradient Descent based algorithms (linear regression, logistic regression, neural networks) that use gradient descent as an optimisation technique require data to be scaled such that the steps are updated at the same rate for all features.

---

SO. what's the difference between normalisation and standardisation?

__Normalisation__

Typically means rescaling the values into a range of 0 to 1.

Normalisation (MinMaxScaler) preserves the shape of the distribution. It does NOT reduce the importance of outliers. Relative space between each feature's values are preserved.

__Standardisation__

Means rescaling the values to have a mean of 0 and a standard deviation of 1.

Distorts relative distances between feature values.

Standardisation helps neural networks because the mean is centred around 0 and you ensure there are both positive and negative values, making learning easier.

---

This article is excellent because it actually has plots to illustrate the effects of the scaling methods:

https://towardsdatascience.com/scale-standardize-or-normalize-with-scikit-learn-6ccc7d176a02

---

Other interesting links:

- https://www.statisticshowto.com/normalized/
- https://www.analyticsvidhya.com/blog/2020/04/feature-scaling-machine-learning-normalization-standardization/
- https://towardsdatascience.com/why-data-should-be-normalized-before-training-a-neural-network-c626b7f66c7d
