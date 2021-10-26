---
layout: default
title: "[NLP] ELECTRA"
date: 2021-10-25
parent: Paper Reviews
grand_parent: Reviews
nav_order: 18
comments: true
use_math: true
---



## Abstract

BERT의 MLM은 좋은 결과를 내지만 pre-train시 많은 양의 학습이 필요함.

본 논문에서는 Replaced token detection이라는 효과적인 pre-train task를 제안

Replaced token detection : 작은 generator를 이용해 masked된 입력을 다른 token으로 샘플링한 후 discriminator를 이용해 generator를 통해 다른 token으로 대체되었는지 예측

데이터의 100%를 예측-학습 데이터 사용이 효율적



## Introduction

BERT를 Generator와 Discriminator로 나누어 pre-train한 언어 모델

downstream 태스크에서는 discriminator만 사용

BERT에 비해 빠르게 학습

Discriminator가 pre-train할 때도 mask를 안 보기 때문에 pre-train과 fine-tuning의 데이터 형태를 일치시켰음. (반면 BERT는 pre-train시에만 mask를 봄. 이를 해결하기 위해 10%는 원래 토큰 유지했었음) <-> XLNet은 MASK 토큰 안 씀

GAN과 비슷해 보이지만...GAN을 text에 적용하기 어렵기 때문에 Maximum Likelihood Estimation(crossentropyloss 줄이는 거) 방식으로 학습했다. 



