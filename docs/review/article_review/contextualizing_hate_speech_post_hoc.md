---
layout: default
title: "[NLP] Contextualizing Hate Speech Classifiers with Post-hoc Explanation 리뷰"
date: 2021-06-26
parent: Article Reviews
grand_parent: Reviews
nav_order: 8
comments: true
---

Contextualizing Hate Speech Classifiers with Post-hoc Explanation.

Brendan Kennedy, and Xisen Jin, and Aida Mostafazadeh DavaniMorteza Dehghani and Xiang Ren. University of Southern California

URL https://arxiv.org/abs/2005.02439

# 논문 키워드

`hate speech detection`, `SOC`, `Sampling and Occlusion`,

<br>

# 논문에 사용된 모델

* 


<br>

# 논문 주제 

group identifier의 영향을 regularize해서 hate speech detection의 false positive를 낮추는데 얼마나 영향이 있나 검증 

<br>

# 데이터셋

* Gab Hate Corpus(Kennedy et al., 2020)

* Stormfront white supremacist online forum(de Gibert et al., 2018)

Hate Speech detection에 있어서 Gab과 Reddit이 자주 나온다. 트위터에서 ban 당한 유저들이 Gab으로 간다던데...원래 [Hate speech의 big 3](https://static1.squarespace.com/static/5ee500d316a2470c370596d3/t/5ef37fd2bda38f405a76cca9/1593016277155/190529_Beyond_Big3_final.pdf)인 페이스북, 트위터, 유튜브에서는 이제 알고리즘으로 Hate Speech를 아예 못 쓰게 하거나, 빨리 삭제되기 때문에 데이터 수집이 어려워져서 그런 것 같다. 

<br>

# 내용 요약

## 1. Introduction

기존의 imbalanced한 데이터셋 때문에 hate speech classifier들은 hate speech에 자주 등장하는 group identifiers(black, gay, Muslim 등) 가 들어간 문장을 무조건 hate speech로 분류하는 경향이 있었다. 이를 극복하기 위해 특정한 context가 들어간 문장만 골라낼 수 있는 방법에 대해 알아보겠다.

논문에서 사용한 방법은 SOC를 통한 설명으로 드러난 모델의 group bias를 극복하고자 regularization based approach로 group identifier 근처의 context에 대한 모델의 sensitivity를 높였다. 

그 결과 regularization은 group identifiers에 주어지는 attention을 낮추고 hate speech의 일반적인 특징들을 더 중요하게 보게 되었다.

<br>

## 2. Related Work

* 영향을 받은 논문:

  * Warner and Hirschberg(2012): offensive word를 abusive한 context vs. non-abusive한 context

문제의식: 

1) 데이터셋의 심각한 불균형으로 인한 문제

2) 분류 모델의 bias  : social group 이름(gay, Muslim)이 나오면 context 고려없이 hate speech로 분류

->이런 bias를 없애기 위해 : less biased한 데이터셋으로 훈련하는 data augmentation사용, term swapping, debiased word embedding 사용

  

이 논문의 접근법

post-hoc explanation approaches: prediction에 대한 input의 word-level이나 phrase-level 의 importance를 알아낼 수 있다. 즉, 어떤 문장을 혐오 발언으로 분류했을 때 영향을 가장 많이 준 단어/구를 알아낼 수 있다.

  

<br>

## 3. Data

[ Gab Hate Corpus (GHC)](https://osf.io/nqt6h/)

Training data for the GHC (GHCtrain) included24,353 posts with 2,027 labeled as hate, and testdata for the GHC (GHCtest) included 1,586 postswith 372 labeled as hate. Stormfront splits resultedin 7,896 (1,059 hate) training sentences, 979 (122)validation, and 1,998 (246) test.

<br>

## 4. Analyzing Group Identifier

![hate_detection_context1]()

group identifier를 없앴을 때 모델의 성능이 얼마나 떨어지는지 관찰했다. 

### 4.1 Classification Models

* logistic regression with bag of words features
* fine-tuned BERT model

### 4.2 Model Interpretation
**Group identifiers in linear models**

25 identity words를 모아서 이 단어들에 대한 SOC score를 구한다.

![curated_group_identifiers]

**Explanation-based measures**

Sampling and Occlusion(SOC) 알고리즘


$$
\phi(p) = E_{x \delta} [S(X) - S(x/P)]
$$


### 4.3 Bias in Prediction

Hate speech model은 post-hoc(사후의) 설명에 따르면 group identifier에 너무 민감하게 반응하는 경우가 많다. 따라서 모델이 예측을 할 때 그 단어에만 집중해서 예측을 하고 주변의 context는 무시하는 경우가 많아, false positive가 높게 나온다.



여기서 중요한 점은 모델이 identifier를 무시하게 하는 것이 아니라 이를 적절한 context와 매칭시킬 수 있게 하는 것이다.

그냥 무시하게 만들면 false positive는 낮아지더라도 hate speech detection은 잘 못하게 되기 때문이다.

<br>

## 5. Contextualizing Hate Speech Models

model은 identifier terms에게 importance를 주지 않게(give no explained importance) regularized될 수 있다.

**Word Removal Baseline**: 베이스라인 방법으로 group identifier를 모두 제거하는 방법. section A.1.에 있는 단어들을 train과 test set 모두에서 제거했다.

**Explanation Regularization**: SOC 함수(SOC explanations)는 모든 구간에서 미분가능하기 떄문에(fully differentiable), 본 논문에서는 SOC 함수를 regularize하여 0에 가깝게 만들게 할 것이다. 이를 classification 목적함수 L'와 합치면 combined learning objective는
$$
L = L' + \alpha \sum_{w \in x \cap S}[\phi(w)]^2
$$
이다.

[^ ]: S: group identifiers들의 set
[^ ]:x: input word sequence
[^ ]: alpha: regularization의 강도를 나타내는 하이퍼파라미터

또한 SOC에 더해서, OC(input occlusion) explanation도 regularizing 해볼 것이다.



## 6. Regularization Experiments

### 6.1 Experiment Details

* 비교군: fine-tune한 Bert 모델 + word removal

* 논문에서 주장하는 모델: BERT + SOC

실험과정: 

e repeat the process forboth the GHC and Stormfront, evaluating test sethate speech classification in-domain and accuracyon the NYT test set. For the GHC, we used thefull list of 25 terms; for Stormfront, we used the 10terms which were also found in the top predictivefeatures in linear classifiers for the Stormfront data.Congruently, for Stormfront we filtered the NYTcorpus to only contain these 10 terms 

### 6.2 Results



## 7. Conclusion & Future Work

<br>

# References

[Understanding Abuse: A Typology of Abusive Language Detection Subtasks](https://www.aclweb.org/anthology/W17-3012.pdf)
