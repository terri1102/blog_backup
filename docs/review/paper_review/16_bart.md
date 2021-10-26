---
layout: default
title: "[NLP] BART"
date: 2021-08-11
parent: Paper Reviews
grand_parent: Reviews
nav_order: 16
comments: true
use_math: true
---





#  🗝️ 논문 키워드





# 📑 논문 주제





# 📚 데이터셋

* 

# 내용 요약

## Abstract

BART를 소개한다! denoising autoencoder for pretraining sequence-to-sequence models.

Pre-train 방법은 (1) noising function으로 텍스트를 오염시킴 (2) 모델을 이용해서 오리지널 텍스트를 예측함.

## 2. Model

### 2.1 Architecture

표준적인 seq2seq 트랜스포머 아키텍처를 사용하고, GPT처럼 GeLU를 사용. 정규분포(0, 0.02) 사용

Bart_base는 인코더와 디코더에 각각 6개의 레이어 사용.

BERT와 차이 : 

BART는 디코더의 레이어 마다 인코더의 마지막 hidden과 cross-attention 수행

BERT에서는 단어 예측 전에 사용한 feed-forward network를 BART에서는 삭제



### 2.2 Pre-training BART

BART는 디코더의 output과 original document의 cross entropy loss를 줄이는 방식으로 학습한다.



## Noise 방법

### 1. Token Masking

BERT처럼 임의의 토큰을 [MASK]로 교체

[MASK] 토큰이 무엇인지 예측



### 2. Token Deletion

임의의 토큰을 삭제

삭제한 토큰의 위치 예측



### 3. Text infilling

푸아송 분포(lambda=3)에서 span length를 뽑고,각 span은 하나의 [MASK] 토큰으로 대체

span이 0이면 토큰을 지우지 않고 [MASK] 토큰 넣음.

Text infilling은 모델이 몇 개의 token이 span에서 사라졌는지 예측하게 함. 



SpanBERT와의 차이: SpanBert는 clamped geometric 분포에서 샘플링을 하고 각 스팬을 대체하는 토큰 개수와 동일한 [MASK] 개수로 마스킹함. Text infilling은 여러 토큰을 하나의 [Mask]로 마스킹해서 몇 개의 토큰이 대체되었는지도 학습하게 함.



### 4. Sentence Permutation

문장의 순서를 랜덤으로 섞음



### 5. Document Rotation

하나의 token을 uniform하게 뽑은 후 그 토큰을 시작점으로 회점함

모델이 문서의 시작점을 찾도록 학습



 <span style="background:#e8f7ff">**Text infilling**</span> 이 가장 효과적이었음





## 3. Fine-tuning BART

### 3.3 Sequence generation task

생성 QA나 요약 태스크의 경우 input을 복제해서 이를 조작한 후 예측하게 되는데, denoising task와 매우 흡사하다. 인코더 input은 input sequence이며 디코더는 auto-regressive하게 output을 생성한다. 



## 4. Comparing Pre-training Objectives

BART base 모델(인코더, 디코더 각각 6레이어, 히든 사이즈 768)



### 4.1 Comparison Objective

## 5. Large-scale Pre-training Experiments

### 5.1 Experimental Setup

인코더, 디코더 각각 12 레이어이고 히든 사이즈가 1024인 Large 모델을 사용했다.

RoBERTa와 같은 조건으로 맞추기 위해 배치 사이즈는 8000, 500000 스텝 동안 훈련했다. GPT2와 같은 byte-pair 방식으로 토크나이징했다. Text infilling 방법과 sentence permutation을 사용했는데 각 document의 30%를 마스킹했고 모든 문장을 permute했다. 트레이닝 스텝의 마지막 10%에서는 dropout를 비활성화해서 데이터에 fit하게 훈련할 수 있게 했다. 160gb의 뉴스, 책, 이야기, 웹 텍스트를 이용해서 훈련했다. 

### 5.3 Generation Tasks

Bart large는 표준적인 Seq2seq 모델로서 fine-tune되었다. 이 과정에서 label smoothed cross entropy loss를 사용했고, smoothing parameter는 0.1로 주었다. 생성 과정에서 beam size는 5로 두고 beam search 과정에서 중복되는 trigram을 제거했다. validation set으로 예측시 min0len, max-len, length penalty를 설정했다.



# Takeaway

Pre-training 단계에서 어려운 태스크를 주면 모델이 더 잘 배운다.





# Reference

[[Paper Review] Transformer to T5 (XLNet, RoBERTa, MASS, BART, MT-DNN,T5)](https://www.youtube.com/watch?v=v7diENO2mEA&list=LL&index=2&t=5s)
