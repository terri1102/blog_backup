---
layout: default
title: "클래스가 2개일 때 softmax와 sigmoid"
date: 2021-04-17
parent: Deep Learning
comments: true
last_modified_date: true
nav_order: 4
---



d

딥러닝에서 클래스 분류 문제를 풀 때 이진분류인 경우에는 softmax를 쓰나 sigmoid를 쓰나 똑같다. (똑같진 않은데 비슷하다...)



Sigmoid의 경우 output을 1개로만 두고 로짓 예측을 하는데, sigmoid 식이 
$$
\frac{1}{1+e^{-x}}
$$
이기 때문에 x=0일 때 y = 1/2인 시그모이드 함수가 그려진다. 이때 x가 음수이면 y는 0.5 아래이고, x가 양수이면 y는 0.5 위이다. 따라서 logit 예측을 한 경우 음수이면 클래스 0 양수이면 클래스 1로 예측하면 되고, 마지막에 sigmoid를 통과한 경우에는 0.5 초과이면 클래스 1로 예측하면 되고 0.5 미만이면 클래스 0으로 예측하면 된다. 



클래스 1일 확률 : 
$$
P(y_1) = \frac{1}{1+e^{-x}}
$$


클래스 0일 확률 :
$$
P(y_0) = 1- P(y_1) = 1 - \frac{1}{1+e^{-x}}= \frac{1+e^{-x}-1}{1+e^{-x}}= \frac{e^{-x}}{1+e^{-x}}
$$



---

소프트맥스

P(y1) 

