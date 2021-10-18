---
layout: default
title: "[NLP] BERT"
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

Abstract

Pre-trained된 모델 위에 

output layer 추가->Task aware input transformation

다양한 태스크에서 엄청난 개선을 보여줌

\1. Introduction

LM을 pre-train하는 것은 여러 태스크에 있어 효과적.

pre-train에는 두 가지 전략 있음: feature-based 와 fine-tuning. 하지만 이 두 전략은 unidirectional한 LM이기 때문에 pre-train 아키텍처에 제한이 있음. 한방향이기 때문에 이전에 나타난 토큰들과만 self-attention 수행

우리가 제안하는 BERT는 양방향에서 보며 MLM objective으로 pre-train함.

이에 더해서 next sentence prediction 태스크도 pre-train 과정에서 수행함



## 4.4 SWAG

주어진 문장 다음에 이어지는 문장을 4개의 문장 중 선택하는 태스크




