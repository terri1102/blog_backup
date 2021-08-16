---
layout: default
title: "[NLP] Memory-Based Deep Neural Attention (mDNA) for Cognitive Multi-Turn Response Retrieval in Task-Oriented Chatbots 리뷰"
date: 2021-08-15
parent: Paper Reviews
grand_parent: Reviews
nav_order: 12
use_math: true
comments: true
---





# 논문 키워드

`mDNA`, `Bi-LSTM`, `memory`, `attention`, `dialog-system`, `retrieval`

# 논문 주제 





# 데이터셋

메모리를 넣은 논문들

11

15

16

17

18





# 내용 요약

---

## Abstract

최근 많이 사용되고 있는 Transformer based 모델들은 generative chatbot에는 잘 작동하지만 Retrieval-based model에서는 답변이 비일관적이거나 모호한 error를 만들 수 있다고 한다. 그 원인은 'information loss'에 의한 것이다. 

본 논문에서는 Transformer-based retrieval chatbot을 memory-based deep neural attention(mDNA)를 이용해서 강화한다. mDNA는 Bi-LSTM, attention mechanism, memory for information retention in the encoder를 포함하는 간단한 encoder-decoder 모델이다.



## 1. Introduction

Retrieval model: Seq2seq generative chatbot이 자주 사용되는 open domain과 달리 vertical domain에서는 retrieval-based system이 더 많이 사용된다. 

DAM과 같은 최근 연구들은 multi-turn dialogue를 이용하는 것이  response selection에 있어서 더 좋은 성능을 가져온다는 것을 보여주었다. 이는 사람들의 대화 매너는 종종 이전 대화의 컨텍스트와 감정에 영향을 받는 다는 믿음에 기반한다. 

최근 SOTA multi-turn retrieval chatbot 모델들은 Transformer 모델을 기반으로 하는데 여기에는 2가지 문제가 있다.
1) logical inconsistency error : 논리적 미스매치로 인한 retrieved response의 오류
2) vague or fuzzy response candidate error: 모호한 대답

---
이를 해결하기 위해 mDNA를 제안한다. mDNA에의 주요 목적은 transforer-based retrieval 챗봇 모델을 증강하는 것. mDNA는 Bi-LSTM 인코더에 attention 매커니즘을 결합한 DNN

메모리 module이 encoder에 적용되었음. 이는 중요 발화 워드 임베딩을 포착하고 저장해서 인지능력을 제공하기 위해서임.

우리의 연구에서 DAM 모델을 트랜스포머 기반 리트리벌 모델로 사용하고 이를 mDNA와 결합하여 unimodal late fusion method와 비슷하게 구현함



장기 문맥을 형성하기 위해 (to match long-range contextual dependencies) retrieval process 동안 이전의 multi turn 대화 정보는 mDNA Bi-LSTM 인코더로 어텐션 매커니즘을 거쳐서 들어감. 

DAM과 mDNA를 거친 응답 후보들은 소프트맥스를 거쳐 투표함

## 2. Related Works

트랜스포머가 gated-recurrent nn을 대체제 취급을 받지만 novel retrieval chatbot 모델에 트랜스포머-어텐션 아키텍처를 적용한 모델들의 문제가 있다 . - 컨텍스트 미스매치와 비일관적인 논리

우리는 메모리를 gated RNN-based 모델에 적용하면 트랜스포머와 같은 성능 달성할 수 있을 거라 생각. 

메모리, gated-RNN, 트랜스포머의 어텐션 요소를 하나로 합치는 것 제안

## 3. Model Description



## 3.1 Memory Utterance Embedding

긴 multi turn 대화 중에 관련된 워드 representation이 사라지면 예측 답변의 퀄리티에 악영향을 끼침

mDNA- 메모리 유닛을 통해 발화 입력을 임시 저장 - 중요 맥락 발화를 유지하기 위해 - 

word2vec을 이용해 parsed된 인풋 발화 임베딩을 메모리에 m으로 나타냄.
응답 임베딩은 바로 BI-LSTM 인코더로 들어가고 이는 r로 나타냄.

그리고 pre-trained된 워드 임베딩 테이블을 이용해서 우리는 두 개의 인풋 시퀀스를 M과 R로 생성함 e는 R에 속하고, 워드 임베딩 차원임. 

soft attention (alpha M)을 이용해서 중요 발화 워드 벡터를 걸러냄. (eml 길이에 맞게) 

## 3.2 Bi-LSTM Encoding with Attention



## 3.3 Matching Composition and Prediction



## 4. Experiments



## 4.1 Dataset



## 4.2 Hyperparameter Settings





## 5. Results and Evaluation

$$
R_n@k = \frac{TP}{TP+FN'}
$$



## 6. Conclusions







## 느낀점



# References

