---
layout: default
title: "[NLP] Retrieve and Refine 리뷰"
date: 2021-08-08
parent: Paper Reviews
grand_parent: Reviews
nav_order: 20
comments: true
---





# 논문 키워드

`Retrieve&Refine`, `Retriever model`, `Generation model`, `Memory network`, `utterance`,`perplexity`



# 논문 주제 

Retriever model과 Generation model의 단점을 보안한 Retrieve and Refine 모델을 제안한다.



<br>

# 데이터셋

* [ConvAI2 dataset](http://convai.io/) : 대화자들이 서로 자신을 소개하는 chit-chat 데이터셋 

  

<br>

# 논문에 사용된 모델

| Models  | Description |
| ------- | ----------- |
| RetNRef |             |
| RetNRef+ | |
|RetNRef++| |



<br>

# 내용 요약

---

## Abstract

Sequence generation model의 문제는 짧고 generic한 문장을 생성하는 것이고, retrieval model의 문제는 흥미로운 대답을 생성하지만 대답이 한정적이고 문맥에 안 맞을 때도 있다는 것이다. 이 두 모델을 합친 Retrieve & Refine은 두 모델의 한계를 극복한다. Retrieve & Refine 모델은 retrieve한 문장을 additional context로 사용해서 generate하는 모델이다.



## 1. Introduction

Seq2Seq 같은 Sequence generation model은 짧고 generic한 문장을 생성하고(다양한 문장에 대한 답이 될 수 있는 "I don't know"같은 문장), Retrieve model은 이런 문제는 없지만 답이 retrieval set 안에 있을 때만 문맥에 맞는 문장을 생성하는 한계가 있다.



본 논문에서 위의 두 모델의 단점을 개선하고 장점만을 합한 Retrieve and Refine 모델을 제안한다. 현재 많은 NLP task에서 predict한 후 refine하는 방법들이 많이 사용되고 있다. Retrieve and Refine 모델은 ConvAI2 데이터셋을 통해 다양하게 실험되었다.



## 2. Retrieve and Refine



### 모델 구조

Retrieve and Refine 모델은 retrieval model의 output을  standard generative model의 input에 concat해서 input으로 넣어 generative model로 훈련하는 구조이다.



[retrieved output + input] -> generative model



* Generator 모델로는 standard Seq2Seq 모델(a 2-layer LSTM with attention)을 사용

* Retriever로는 Key-Value Memory Network 사용.
  * Key-Value Memory network: Dialogue history를 참고해 input과 candidate retrieval의 임베딩을 코사인 유사도로 비교해 매치를 찾는 모델. 


* Memory network로 찾은 top scoring utterance는 Seq2Seq의 input으로 들어가서 refine됨. (아마 기존의 input과 seperator token으로 연결된 후 들어간다는 것 같음)



### 모델 훈련 **과정** 

**RetNRef**

1. 모든 dialogue turn에 대해서 retrieval result을 계산한다. 이때 top ranking result를 사용하는 대신 임베딩 스페이스 안에서 label과 유사도에 따라서 top 100 prediction을 rerank한다. 이는 refinement가 original retrieval로부터 너무 멀어지는 것을 방지하는 효과가 있다.
2. 이 chosen utterance를 Seq2Seq의 모델의 기존 input에 붙여서 Seq2Seq 모델을 훈련한다.

<br>



### 모델 variation 

**Use Retriever More : RetNRef+**

<br>

* vanilla model의 문제: generator가 retrieval utterance를 많이 참조 안 함
* 해결: Seq2Seq 모델의 input은 retrival utterance가 붙은 dialogue history이기 때문에, 이 history를 잘라내면 retrival에 더 많은 attention이 감
* ConvAI2 데이터셋에서 dialogue의 첫 profile sentences를 잘라냄

<br>

**Fix Retrieval Copy Errors : RetNRef++** 

<br>

Generate된 문장의 많은 부분이 Retrieved 문장과 중복되고 일부 단어만 다른 경우 실수를 많이 하기에, 60%이상이 중복되면 retrieval 그대로 복사하는 모델을 만들었다.





## 3. Experiments

본 논문에서는 PersonaChat 데이터셋을 변형한 ConvAI2 데이터셋을 이용하였다. ConvAI2는 랜덤하게 짝지어진 크라우드 워커들이 랜덤하게 주어진 페르소나를 유지하면서 대화를 나누는 데이터셋이다.  이중 훈련 데이터셋은 160,000 utterances, 11,000 dialogues로 이루어져 있고, validation set과 test set은 각각 겹치지 않는 페르소나를 사용하는 2000 dialogues로 이루어져 있다.



### 3.1 Automatic Evaluation and Analysis

**Perplexity(PPL)** : PPL은 언어모델을 평가하는 내부평가지표로 단어의 수로 정규화된 테스트 데이터에 대한 확률의 역수이다. 예를 들어 PPL이 10이면 문장의 앞 부분(혹은 단어)를 통해서 다음 단어를 예측할 때 평균 10개의 후보 단어 중 선택할 수 있다는 의미이다. 즉, PPL이 낮을수록 더 적은 수의 후보 단어로 예리하게 예측한다고 할 수 있다.

대화는 다양한 답(valid answer)이 있을 수 있기 때문에 자동화된 평가가 어렵다. 최근 많은 논문들은 Perplexity를 이용한 평가를 하고 있다. 하지만 Retrieve and refine 모델은 Perplexity도 적용이 어렵다. 왜냐하면 retrieval이 가져온 문장이 답과는 다르지만 말이 되는 문장일 때 model은 이를 refine하려고 해서 perplexity가 안 좋아지기 때문이다.



따라서 본 논문은 다양한 retrieval method를 고려하며 평가하고자 한다. 논문에서 사용한 retrieval method들은
1. Memory Network approach(최고 성능 retriever model)
2. 트레이닝셋에서 랜덤한 utterance를 반환하는 retriever
3. 테스트셋에서 주어진 레이블(the true label gien in the test set)
4.  memory network의 임베딩 스페이스로 평가했을 때, 트레이닝셋 utterances의 true label과 가장 가까운 nearest neighbor 

이렇게 4가지로 나눠 볼 수 있다. 3번과 4번의 경우 배포환경에서 테스트는 불가하지만 sanity check를 위해서 넣었다.



<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/rnr1.jpg?raw=true" alt="rnr1" style="zoom:67%;" />

위의 <Table 1>을 보면 RetNRef model은  label 이웃이나 레이블과 함께있으면 perplexity를 높일 수 있다. Retrieval을 이용하고 안 하고에 있어서 perplextity는 별 차이없다.
하지만 perplexity가 안 좋다고 generate된 문장이 안 좋은 문장인 것은 아니다.



**Word Statistics **: 생성해 낸 단어의 개수와 문자(character)의 개수, 흔하지 않은 단어 사용 빈도

![rnr2]()



![rnr3]()

### 3.2 Evaluation by Human Judgment Scores



# Glossary









# References

memory network 논문 읽고 싶다. (Miller et al., 2016)