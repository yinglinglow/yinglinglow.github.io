---
layout: post
title: "Clustering"
date: 2018-07-19
---

Sooooo clustering algorithms!

Silly old brain was blanking out and could only think of KMeans and DBSCAN, so did a little google.

I think sklearn is almost like a 'cheatsheet' (sort of) lol: most things you need, you got it there!

check this out: http://scikit-learn.org/stable/modules/clustering.html#birch

---

For some next level stuff - if you have a gazillion (ok maybe like 80) features, you can use auto-encoders to reduce your features to fewer dimensions (say, 10?) and then use traditional clustering algorithms.

Auto-Encoders are like PCA on steroids - PCA is restricted to linear maps, while you can use non-linear functions for AEs.

For more deets, check this out: https://stats.stackexchange.com/questions/120080/whatre-the-differences-between-pca-and-autoencoder