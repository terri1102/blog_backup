---
layout: post
title: "[선형대수]Eigen vector와 Eigen values를 이용한 차원축소"
date: 2021-01-14
category: [TIL]
excerpt: "고유값, 고유벡터의 개념과 PCA"
tags: [Eigen vector, Eigen values, PCA, plot]
comments: true
---





Hierarchical Clustering

:algorithm that builds heirarchical clusters

1. each components are assigned by its own cluster
2. based on the distance, close componets are bind into one cluster
3. repeat it until there is only one cluster



K-means와 Hierarchical clustering 차이

HCA은 빅 데이터를 잘 다루지 못한다. time complexity가 높아서

HCA의 clustering은 reproducible

K means은 주로 구형 클러스터와 잘 어울림



Distance metrics

linkage에 다양한 방법이 있다.(centroid)

방법에 따라 다른 cluster은 얻게된다.



https://joernhees.de/blog/2015/08/26/scipy-hierarchical-clustering-and-dendrogram-tutorial/#Perform-the-Hierarchical-Clustering