---
layout: default
title: "Maximum Likelihood"
date: 2021-10-12
parent: Deep Learning
comments: true
last_modified_date: true
nav_order: 5
---



## MLE에 대한 정리

Maximum Likelihood Estimation은 데이터의 평균이나 표준편차 등을 찾기 위해서 우도를 최대로 만드는 지점을 찾는 것이다. 


$$
\hat{\theta} = \underset{\theta}{arg \;max} \; f(X|\theta) 
$$
만약 관측치가 모두 독립적이고 identically distributed이라면


$$
f(X|\theta) = \prod_i f(x_i|\theta)
$$


라고 쓸 수 있다.





https://machinelearningmastery.com/what-is-maximum-likelihood-estimation-in-machine-learning/
