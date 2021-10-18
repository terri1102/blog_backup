---
layout: default
title: "[NLP] Unlikelihood Loss 리뷰"
date: 2021-08-11
parent: Paper Reviews
grand_parent: Reviews
nav_order: 11
nav_exclude: true
search_exclude: true
comments: true
---





# 논문 키워드

`unlikelihood`, `Maximum Likelihood Estimation(최대우도법) `, `decoding methods`, `greedy search`, `beam search`, `top k sampling`, `nucleus sampling`

# 논문 주제 





# 데이터셋

* Wikitext-103 dataset (Merity et al., 2016), a large-scale collection of
  Wikipedia articles containing over 100 million words and 260 thousand unique tokens.
  
  



# 내용 요약

---

## Abstract





## 1. Introduction





## 2. Related Work



## 3. Neural Text Generation



### Language Modeling

$$
L_{MLE}(p_\theta, D) = - \sum^{\abs{D}}_{i=1}\sum^{\abs{x^{(i)}}}_{t=1} log \; p_{\theta}(x_t^{(i)}|x^{(i)}_{<t})
$$

### Sequence Completion

### Deterministic Decoding

자주 쓰이는 deterministic decoding 방법은 greedy search와 beam search이다. (그리디는 빔서치의 특수한 경우라고 할 수 있다.)

### Stochastic Decoding





## 4. Neural Text Degeneration

degeneration: 약화, 퇴화.



### 5. The Unlikelihood Training objective

5.1 Unlikelihood training



5.2 Sequence-level unlikelihood training



prefix가 의미하는 게 뭐지?? 그냥 문장의 일부 같은데?



### 6. Experiments

**Model Architecture**

**Dataset**

**Training**

**Completions**



### 6.1 Evaluation Metrics

**Repetition**

**Token Distribution**

**Language Modeling Qualing**



### 6.2 Results

**Baseline**



**Token-Level Objective**



**Sequence-Level Objective**



**GPT-2 Fine-Tuning**



**Stochastic Decoding**



**Human Evaluation**



### 7. Conclusion





## 느낀점



# References

Decoding methods https://towardsdatascience.com/decoding-strategies-that-you-need-to-know-for-response-generation-ba95ee0faadc



MLE https://angeloyeo.github.io/2020/07/17/MLE.html

최대우도법: 모수적인 데이터 밀도 추정 방법으로서 파라미터 $\theta = (\theta_1, ..., \theta_m)$ 으로 구성된 어떤 확률밀도함수 P(x|theta)에서 관측된 표본 데이터 집합을 x=(x1,x2,...,xn)이라고 할 때, 이 표본들에서 파라미터 theta = (theta1, ..., theta_m)를 추정하는 방법이다. 


$$
Likelihood \; Function: P(x|\theta) = \prod_{k=1}^{n} P(x_k| \theta)
$$
capital pi : 각 값들을 곱함(product)
$$
Log-likelihood \; Function : L(\theta|x)=log P (x|\theta) = \sum^n_{i=1}log P (x_i |\theta)
$$
log함수는 단조증가 함수이기에  likelihood 함수의 최댓값이나 log likelihood 함수의 최댓값을 갖게 해주는 정의역의 함수 입력값은 동일하다. 계산 편의를 위해 log-likelihood의 최댓값을 찾는다.

미분계수를 이용하여 계산: 찾고자 하는 파라미터 theta에 대해 다음과 같이 편미분하고 그 값이 0이 되도록 하는 thetha를 찾는 과정을 통해 likelihood함수를 최대화 시켜줄 수 있는 theta를 찾을 수 있다. 
$$
\frac{\part}{\part\theta}L(\theta|x) = \frac{\part}{\part\theta}log P (x|\theta) = \sum^n_{i=1}\frac{\part}{\part\theta} log P(x_i|\theta) = 0
$$
