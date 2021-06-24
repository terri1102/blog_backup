---
layout: default
title: "T academy 자연어 언어모델 BERT"
date: 2021-06-24
parent: ETC
grand_parent: Reviews
nav_order: 7
comments: true
---



BERT 소개와 실습 강의입니다. [T academy 자연어 언어모델 'BERT'](https://tacademy.skplanet.com/live/player/onlineLectureDetail.action?seq=164) 를 듣고 정리한 내용입니다. 실습 부분만 보다가 강의 내용이 너무 좋아서 처음부터 듣기 시작했는데, BERT를 자세하게 이해하고 싶은 분들께 추천합니다. T academy나 유투브 채널에서 무료로 시청 가능합니다.



# [1강] 자연어 처리(NLP)



## 언어란? 

메시지의 전달 -> 메세지의 부호화(Encode) -> 경로(noise가 들어갈 수 있음) -> 메세지의 수신 -> 메시지의 해독(Decode)



자연어: 일반 사회에서 자연히 발생하여 쓰이는 언어 <-> 인공 언어(c 언어)

자연어 처리: 컴퓨터를 이용하여 인간 언어의 이해, 생성 및 분석을 하는 기술



## 기존의 자연어 처리의 접근 방법

1) Symbolic approach: 규칙/지식 기반 접근법. 패턴 정의 후 if문으로 처리

2) Statistical approach: 확률/통계 기반 접근법, TF-IDF 이용한 키워드 추출

Statistical approach를 딥러닝 모델에 사용하는 것이 요즘 트렌드



### 전처리

개행문자 제거, 특수문자 제거, 공백 제거, 중복 표현 제거, 이메일, 링크 제거, 제목 제거, 불용어 제거, 조사 제거, 띄어쓰기, 문장분리 보정, 사전 구축

### Tokenizing 

자연어를 어떤 단위로 살펴볼 것인가? 우리말은 형태소 단위, 자소 단위로도 쪼갬

종류: 어절 tokenizing, 형태소 tokenizing, n-gram tokenizing, WordPiece tokenizing

### Lexical analysis

어휘 분석, 형태소 분석, 개체명 인식, 상호 참조

### Syntactic analysis

구문 분석

### Semantic analysis

의미 분석



다양한 자연어 처리 Applications: 문서 분류, 문법, 오타 교정, 정보 추출, 음성 인식결과 보정, 음성 합성 텍스트 보정, 정보 검색, 요약문 생성, 기계 번역, 질의 응답, 기계 독해, 챗봇

형태소 분석, 개체명 분석, 구문 분석, 감성 분석, 관계 추출..



EVA 챗봇 앱에 들어가는 기술들은...챗봇 + 음성 합성 + 감성 분류 + 개체명 인식 + 추천 시스템 + 기계 독해 + 지식 그래프 + 관계 추출 + 플러그인

사용자와 상호작용하면서 지식 그래프를 만들어감(사용자가 답한 것을 db에 저장하는 듯)

Bert는 기계 독해에 강한 모델. QA 모델에 적합하다.



### 특징 추출과 분류

* **One-hot encoding**

자연어에서 특징을 추출하는 가장 단순한 표현 방법: one-hot encoding -> Sparse representation

n개의 단어는 n차원의 벡터로 표현

단어 벡터가 sparse해서 단어가 가지는 '의미'를 벡터 공간에 표현 불가능

* **Word2Vec**

자연어(단어)의 의미를 벡터 공간에 임베딩하는 간단한 신경망

한 단어의 주변 단어들을 통해 그 단어의 의미를 파악

![bertq]()



Word2vec 알고리즘은 주변부의 단어를 예측하는 방식으로 학습(skip-gram방식)

단어에 대한 dense한 vector를 얻을 수 있음



# [2강] 언어 모델(Language Model)





## Reference

BERT 톺아보기 http://docs.likejazz.com/bert/
