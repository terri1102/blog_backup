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

Generate된 문장의 많은 부분이 Retrieved 문장과 중복되고 일부 단어만 다른 경우 실수를 많이 하기에, 60%이상이 중복되면 retrieval 그대로 복사하는 모델





# 관련 논문

memory network 논문 읽고 싶다. (Miller et al., 2016)



# References

