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





# 논문 키워드

`encoder`, `poly-encoder`, `bi-encoder`, `cross-encoder` 



# 논문 주제

Facebook AI Research팀에서 Bi-encoder보다 성능을 개선하고, Cross-encoder보다 예측 시간을 개선한 Poly-encoder 제안



# 데이터셋

* Reddit

* Wiki + Toronto books corpus

  

# 내용 요약

## Abstract

pairwise comparison이나 주어진 인풋을 관련라벨과 매칭하는 테스크에 주로 사용되는 방법-> 왜?
트랜스포머 기반 아키텍처를 사용한 2가지 방법: Cross-encoder, Bi-encoder 
Cross-encoder는 단어 쌍별로 self attention 수행-> 성능 좋지만 느림
Bi-encoder : 단어 쌍의 단어를 각각 인코딩. 빠름

Poly-encoder : 토큰 레벨이 아닌 전역적으로 self attention 피처를 배움
poly-encoder는 cross-encoder보다 빠르고 Bi-encoder보다 정확함
-그 외 발견: 다운스트림 테스트와 비슷한 데이터셋으로 pre-training할 때 성능이 좋음



* <span style="background:#fff28c">Alias 정리</span> : 논문에서 Input context와 Candidate label을 가리키는 단어들이 다양하게 사용되기에 정리해두겠다.
  * Input context : Input, Context
  * Candidate label : candidate, label, response

## 1. Introduction

multi-sentence scoring 테스크 : input context가 주어졌을 때 응답 후보를 점수 매기는 것. retrieval and dialogue task에 많이 사용된다.

최근 SOTA는 BERT모델을 이용한 pre-training인데, 보통 이 위에 Bi-encoder나 Cross-encoder를 올려서 사용한다. 크로스 인코더는 full (cross) self-attention을 주어진 인풋과 라벨 후보별 로 진행한다. 정확하지만 느림.
바이 인코더는 인풋과 라벨 후보를 따로 self-attention을 진행한 후 이를 합쳐서 final representation을 만듦. 인코딩

된 응답 후보들을 캐싱해서 이를 인풋에 조합할 때 재사용가능하기에 빠르다.



우리는 Poly-encoder를 소개한다.   self-attention을 수행하기 위해 전역적인 특성을 표현하는 추가로 배운 attention 매커니즘
BERT와 다르게 다운 스트림과 비슷한 데이터로 pre-train함

dialogue and information retrieval(IR) 

## 2. Related Work

제너럴한 테스크에서 응답 후보들들은 이산적인 클래스가 아닌 구조체이다. 이 연구에서 우리는 인풋과 후보 라벨(candidate label)을 텍스트 시퀀스로 고려할 것.

\<Bi-encoder>
input과 candidate를 각각 common feature space에 매핑한 후에 이 둘의 유사성을 내적이나 코사인 유사도를 통해서 구한다. 즉 임베딩 벡터가 각각 나옴. 우리가 고려한 Bi-encoder 접근으로 메모리 네트워크, 트랜스포머 메모리 네트워크, LSTM, CNN 등이 있음. Bi-encoder의 장점은 크고 고정된 사이즈의 응답 후보들을 캐싱한 후에 사용하여, 인풋과 독립적이기에 evaluation이 효율적임

\<Cross-encoder>
input과 candidate를 concat한 후에 self-attention을 적용한다. 인풋과 후보들의 관계를 잘 나타낼 수 있다. 후보들의 모든 단어와 인풋의 모든 단어 간의 상호작용을 알 수 있어서

## 3. Tasks

ConvAI2 테스크 설명
20개의 선택지 중에 가장 적합한 응답 후보를 선택해야함. 페르소나
\<convai2> 데이터셋 예시 보여주기



StarSpace 설명 필요함



## 4. Methods



### 4.1 Transformers and Pre-traininig Strategies



### 4.2 Bi-encoder



### 4.3 Cross-encoder



### 4.4 Poly-encoder



## 5. Experiments



### 5.1 Bi-encoders and Cross-encoders

![polyencoder3]

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



