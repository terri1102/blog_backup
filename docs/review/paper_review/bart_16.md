---
layout: default
title: "[NLP] BART"
date: 2021-08-11
parent: Paper Reviews
grand_parent: Reviews
nav_order: 13
comments: true
use_math: true
---





#  🗝️ 논문 키워드





# 📑 논문 주제





# 📚 데이터셋

* 

# 내용 요약

## Abstract





## Noise 방법

### 1. Token Masking

BERT처럼 임의의 토큰을 [MASK]로 교체

[MASK] 토큰이 무엇인지 예측



### 2. Token Deletion

임의의 토큰을 삭제

삭제한 토큰의 위치 예측



### 3. Text infilling

푸아송 분포(lambda=3)에서 span length를 뽑아 하나의 [MASK] 토큰으로 대체



### 4. Sentence Permutation

문장의 순서를 랜덤으로 섞음



### 5. Document Rotation

하나의 token을 uniform하게 뽑은 후 그 토큰을 시작점으로 회점함

모델이 문서의 시작점을 찾도록 학습



 <span style="background:#e8f7ff">**Text infilling**</span> 이 가장 효과적이었음



# Takeaway







# Reference

[[Paper Review] Transformer to T5 (XLNet, RoBERTa, MASS, BART, MT-DNN,T5)](https://www.youtube.com/watch?v=v7diENO2mEA&list=LL&index=2&t=5s)
