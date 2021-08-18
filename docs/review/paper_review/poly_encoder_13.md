---
layout: default
title: "[NLP] Poly-encoders: architectures and pre-training
strategies for fast and accurate multi-sentence scoring 리뷰"
date: 2021-08-11
parent: Paper Reviews
grand_parent: Reviews
nav_order: 13
comments: true
use_math: true
---





# :key:논문 키워드

`encoder`, `poly-encoder`, `bi-encoder`, `cross-encoder` 



# :bookmark_tabs: 논문 주제

Facebook AI Research팀에서 Bi-encoder보다 성능을 개선하고, Cross-encoder보다 예측 시간을 개선한 Poly-encoder 제안



# 📚 데이터셋

* Reddit

* Wiki + Toronto books corpus

  

# 내용 요약

## Abstract

Pairwise comparison이나 주어진 인풋을 관련 라벨과 매칭하는 테스크에 트랜스포머 기반 아키텍처를 사용한 2가지 방법이 주로 사용되었다. 그 중 <span style="background:#e8f7ff">**Cross-encoder**</span>는 시퀀스 쌍별로 self attention을 수행하는 방법으로 성능은 좋지만 Inference가 느리다는 단점이 있었고, <span style="background:#fff28c">**Bi-encoder**</span>는 시퀀스를 각각 인코딩하여 Inference는 빠르지만, Cross-encoder보다 성능이 떨어졌었다. 본 논문에서 제안하는 <span style="background:#ffe4de">**Poly-encoder**</span>는 토큰 레벨이 아닌 전역적으로 self attention 피처를 배우기 때문에 cross-encoder보다 빠르고 Bi-encoder보다 정확하다. 그 외 발견으로는 사전학습(pre-training) 단계에서도 다운스트림 테스트와 비슷한 데이터셋으로 pre-training할 때 성능이 좋다는 점을 소개한다.



* **Alias 정리:** 논문에서 Input context와 Candidate label을 가리키는 단어들이 다양하게 사용되기에 정리해두겠다.
  
  * Input context (인풋) : Input, Context
  
  * Candidate label (라벨 후보) : candidate, label, response
  
    

## 1. Introduction

**Multi-sentence scoring** : Input context가 주어졌을 때 응답 후보들의 점수를 매기는 것이로 Retrieval and dialogue task에 많이 사용된다. 이 Multi-sentence scoring 분야에서 최근 SOTA 모델들은 사전학습된 BERT모델들을 사용한 Bi-encoder나 Cross-encoder가 많이 이용된다. 



<span style="background:#e8f7ff">**Cross-encoder**</span>는 full (cross) self-attention을 주어진 인풋과 라벨 후보별 로 진행하기 때문에 성능이 좋지만 느리다.
<span style="background:#fff28c">**Bi-encoder**</span>는 인풋과 라벨 후보를 따로 self-attention을 진행한 후 이를 합쳐서 final representation을 만든다. 인코딩된 응답 후보들을 캐싱해서 이를 인풋에 조합할 때 재사용가능하기에 빠르다.

본 논문에서 소개하는 <span style="background:#ffe4de">**Poly-encoder**</span>는 self-attention을 수행하기 위해 전역적인 특성을 표현하는 추가로 학습한 attention 매커니즘을 사용하여 Bi-encoder보다 좋은 성능을 보여주었다.

위의 모델들을 비교하기 위해서 Dialogue and information retrieval(IR)에서 사용되는 4가지의 데이터셋을 통해 모델을 검증하는데 Reddit과 Wikipedia/Toronto Books로 각각 사전학습시켜서 다운 스트림과 비슷한 데이터로 사전학습했을 때의 결과를 비교한다. (wiki 데이터로 학습한 BERT와 다르게 대화형 데이터인 레딧 데이터 사용해 대화형 시퀀스 스코링다운스트림 테스크에 적용함) 우리의 모델 아키텍처와 새로운 사전학습을 통해서 새로운 SOTA를 달성했다. 

## 2. Related Work

머신러닝에 있어서 다양한 후보들을 스코어링하는 것은 아주 클래식한 문제이다. 이를 이산적인 multi-class 분류라는 특수한 경우로 볼 수도 있지만 이를 일반화하면 응답 후보들은 클래스가 아닌 구조체이다. 이 연구에서 우리는 인풋과 후보 라벨(candidate label)을 텍스트 시퀀스로 고려할 것이다.

<span style="background:#fff28c">**Bi-encoder**</span> 
Input과 candidate를 각각 common feature space에 매핑한 후에 이 둘의 유사성을 내적이나 코사인 유사도를 통해서 구한다. 즉 임베딩 벡터가 Input과 candidate에서 각각 나온다. 논문에서 Next utterance prediction task를  (NSP랑 같은 건가????) 위한 Bi-encoder 접근으로 메모리 네트워크, 트랜스포머 메모리 네트워크, LSTM, CNN 등을 고려했었다.(근데 안 쓴 거지? BERT기반 인코더 쓴 거지??)  Bi-encoder의 장점은 크고 고정된 사이즈의 candidate set을 캐싱한 후에 사용하여, input과 독립적이기에 evaluation 과정에서 연산 효율적이다.

<span style="background:#e8f7ff">**Cross-encoder**</span>
Input과 candidate를 concat한 후에 self-attention을 적용한다. 따라서 두 문장의 임베딩 간의 관계(유사도)를 구하지 않고 두 문장을 합친 것이 활성화 함수로 들어가서 하나의 임베딩을 얻게 된다. 이 경우 candidate들의 모든 단어와 input의 모든 단어 간의 상호작용을 알 수 있어서 Input sequence와 candidate sequence들의 관계를 잘 나타낼 수 있다. 

## 3. Tasks

**ConvAI2 테스크 설명**

![convai2](https://paperswithcode.com/media/datasets/ConvAI2-0000003063-95759eda.jpg)20개의 선택지 중에 가장 적합한 응답 후보를 선택해야함. 페르소나
\<convai2> 데이터셋 예시 보여주기



StarSpace 설명 필요함

**Dataset 설명**

![poly2](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/polyencoder2.jpg?raw=true)

## 4. Methods



### 4.1 Transformers and Pre-traininig Strategies

* **Transformers**

비교를 위해서 2가지 데이터셋을 이용해 트랜스포머를 훈련함 
1)BERT-base와 비슷하게 위키피디아와 토론토 북스 코퍼스에서 추출한 input, label을 스페셜 토큰[S]로 감쌈
2 Reddit의 경우 input은 context이고 label은 next utterance

위키피디아, 토론토 : input: 문장 하나. label: 다음 문장
각 인풋 토큰은 3가지 임베딩의 합! (token embedding, position embedding, segment embedding) -> bert와 동일한듯

* **Input Representation**

pre-training input은 input과 label의 concat한 것. 모두 스페셜 토큰 [S]로 둘러싸여 있음(Lample&Conneau,2019) 참조.

* **Pre-training Procedure**
* **Fine-tuning**

### 4.2 Bi-encoder

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/bi-encoder.jpg?raw=true" alt="bi-encoder" style="zoom:100%;" class="center"  />



이제 위의 4.1에서 사전학습한 인코더를 Fine-tuning 과정에 대한 설명이다. 
Bi-encoder 설명
$$
y_{ct xt} = red(T_1(ctxt)) \quad y_{cand}=red(T_2(cand))
$$

$$
y_{ctxt}
$$

\(y_{ctxt}\)

는 Input을 입력받아서 벡터로 인코딩된 후(T1(ctxt))에 red(·) 함수를 거쳐서 하나의 벡터로 축소된 **임베딩 벡터**이다. (그림에서 ctxt Emb)

* T(x) = h1,...,h_n : 트랜스포머의 output (out1 == h1 == [S]의 임베딩값)

* red(·) : 벡터들의 시퀀스를 하나의 벡터로 축소시키는(reduce) 함수

  red로 사용할 함수로 

  1. 트랜스포머의 첫 번째 output을 취하는 함수

  2) 모든 output의 평균값을 취하는 함수
  3) 첫 m개의 output의 평균값을 취하는 함수 (m<N)

  를 고려했었는데, 1번 함수 사용했다. 다른 함수를 사용했을 때의 결과는 Appendix에서 볼 수 있다.

* Context encoder : Bert와 동일하게 3가지의 값을 더해서 임베딩을 구함. 이때 input과 candidate는 각각 인코딩되기 때문에 segment tokens은 모두 0이다. 사전학습에서 비슷한 환경을 모방하기 위해 input과 candidate는 모두 스페셜 토큰 [S]로 둘러 싸인다. 

Bi-encoder는 input과 candidate를 각각 인코딩하기 때문에 candidate의 representation은 캐싱한 후에 inference할 때 가져와서 사용할 수 있다. 



모델 2개를 만들어서 실험 후에 더 나은 모델 사용할 것. 
T1, T2 모
두 pre-trained된 트랜스포머. 둘 다 같은 가중치로 시작하지만 파인튜닝 과정동안 달라질 듯. T(x) = h1, h2...,hn은 트랜스포머의 output이고 red()는 벡터들의 시퀀스를 벡터 하나로 줄이는 함수
input과 label이 각각 인코딩되기 때문에 segment token은 둘 다 0임
pre-training 과정을 모방하기 위해 input과 label은 각각 스페셜 토큰 [s]로 둘러 싸임

* **Scoring**

cand_i의 점수는 
$$
s(ctxt,cand_i)=y_{ctxt y}· y_{cand_i}
$$
 input의 임베딩과 candidate의 임베딩을 내적한 값이다. 모델은 cross-entropy loss를 최소화하는 방식으로 훈련되었다. 



* **Inference speed**

  

### 4.3 Cross-encoder

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/cross-encoder.jpg?raw=true" alt="crossencoder" style="zoom:100%;" class="center" />
$$
y_{ctxt,cand} = h_1 = first(T(ctxt,cand))
$$


* Scoring

$$
s(ctxt, cand_i) = y_{ctxt,cand_i}W
$$



### 4.4 Poly-encoder

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/poly-encoder.jpg?raw=true" alt="polyencoder" style="zoom:100%;" class="center" />

## 5. Experiments



### 5.1 Bi-encoders and Cross-encoders

![polyencoder3](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/polyencoder3.jpg?raw=true)

![polyencoder4]

![polyencoder5]

### 5.2 Poly-encoders

![polyencoder6]

### 5.3 Domain-specific Pre-training



### 5.4 Inference Speed



## 6. Conclusion



## A Training Time



## B Reduction layer



## C Alternative Choices for Context Vectors





# :meat_on_bone: ​Takeaway



# Reference

**Cross-encoder**

Thomas Wolf, Victor Sanh, Julien Chaumond, and Clement Delangue. Transfertransfo: A
transfer learning approach for neural network based conversational agents. arXiv preprint
arXiv:1901.08149, 2019.

**Bi encoder**

Pierre-Emmanuel Mazar´e, Samuel Humeau, Martin Raison, and Antoine Bordes. Training millions
of personalized dialogue agents. In EMNLP, 2018.

SBERT Bi-Encoder 

https://towardsdatascience.com/advance-nlp-model-via-transferring-knowledge-from-cross-encoders-to-bi-encoders-3e0fc564f554

Augmented SBERT

https://karter.io/sentence-bert-augmented-sbert
