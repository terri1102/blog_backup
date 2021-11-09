---
layout: default
title: "BLEU"
date: 2021-11-19
parent: Deep Learning
comments: true
last_modified_date: true
nav_order: 6
---



## BLEU score

BLEU(Bilingual Evaluate Understudy)

기계 번역과 인간이 한 번역 결과를 비교하여 품질 평가를 하는 통계적인 지표



### 특징

* n-gram precision
* 너무 짧은 기계 번역에는 패널티를 줌



## 공식


$$
BLUE = BP * exp(\sum^N_{n=1}W_n log P_n)
$$

$$
log  \; \:  BLUE= min(1-\frac{r}{c},0) + \sum^N_{n=1}W_n \log{P_n}
$$






### Pn : candidate과 reference를 비교


$$
P_n = \frac{\sum_{n-gram \in C}Count_{clip}(n-gram)}{\sum_{n-gram \in C} Count(n-gram)}
$$


Candidate의 n-gram의 Count_clip 개수를 candidate의 n-gram 개수로 나눈 것을 의미한다. 



### 패널티 : brevity-penalty


$$
BP = \begin{cases} 1 \quad if \; \; c \gt r \\exp(1-\frac{r}{c}) \quad if \; \; c \le r \end{cases}
$$


예를 들어 candidate가 "It is a guide to action which ensures that the military always `obeys` the command of the party."이고

Reference가 "It is a guide to action that ensures that the military `will` forever `heed` party commands."라고 하면 

Unigram-precision은 17/18이다.
$$
\frac{reference에 \; 들어있는\; candidate\; unigram의\; 개수}{candidate에 \; 있는 \; unigram의 \;개수}
$$


