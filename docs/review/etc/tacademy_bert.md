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

![bertq](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/bertq.jpg?raw=true)

Word2vec 알고리즘은 주변부의 단어를 예측하는 방식으로 학습(skip-gram방식)

단어에 대한 dense한 vector를 얻을 수 있음



| Sparse Representation | Dense Representation |
| --------------------- | -------------------- |
| one hot encoding      |   word embedding     |
| 단어가 커질 수록 무한대 차원의 벡터 | 한정된 차원으로 표현 가능|
|의미 유추 불가능| 의미 관계 유추 가능|
|차원의 저주|비지도 학습으로 단어의 의미 학습 가능|

Representation Learning에 대한 좋은 글



단어의 의미가 벡터로 표현됨으로써 `벡터 연산`이 가능! 추론 가능



### 임베딩이 잘 되었는지 확인하는 법: 

사람이 단어의 유사도를 annotate함(정답)

두 단어 벡터의 cosine similarity를 구한 후 정답과 Spearman's rank-order correlation값을 획득

경험상 0.7이상의 값이 나오면 embedding이 잘 수행되었다고 할 수 있음



```python
#gensim을 이용한 word2vec 실습

from gensim.models.word2vec import Word2Vec
import gensim

path = 'train_corpus.txt'
sentences = gensim.models.word2vec.Text8Corpus(path)

model = Word2Vec(sentences, min_counts=5, size=100, window=5)
model.save('w2v_model')

saved_model = Word2Vec.load('w2v_model')

word_vector = saved_model['강아지']

saved_model.similarity('강아지','멍멍이')

saved_model.similar_by_word('강아지')

```

장점: 단어 간의 유사도 측정에 용이, 단어간 관계 파악 및 벡터 연산을 통한 추론 가능

단점: 단어의 subword information 무시(서울 vs 서울시 vs 고양시)

​		 out of vocabulary에서 적용 불가능



## FastText

* **Training:** 

  기존의 word2vec과 유사하나, 단어를 n-gram으로 나누어서 학습을 수행

  n-gram의 범위가 2-5일 때 단어를 다음과 같이 분리하여 학습

  "assumption" = {as,ss,su,...,ass,ssu,sum,ump,mpt,...,ption,assumption}

  as-> 2개니까 bi-gram

  이 때, n-gram으로 나눠진 단어는 사전에 들어가지 않으며 별도의 n-gram vector를 형성함(각 단어별 n-gram vector가 있음)

* **Testing**

  입력 단어가 vocabulary에 있을 경우, word2vec과 마찬가지로 해당 단어의 word vector를 return함

  만약 OOV일 경우, 입력 단어의 n-gram vector들의 합산을 return함

subword를 이용해서 학습함

오탈자, OOV, 등장 횟수가 적은 학습 단어에 대해서 강세

FastText Model 중 jamo-advanced 이용하면 성능 좋음



## Word Embedding의 한계점

Word2vec이나 FastText와 같은 word embedding 방식은 동형어, 다의어 등에 대해선 embedding 성능이 좋지 못하다

주변 단어를 통해 학습이 이루어지기 때문에 `문맥`을 고려할 수 없다



한국어에서의 n-gram? 

기본은 음절을 단위로 n-gram을 시행. 

자소 단위로 나눠서 n-gram 하기도 함. 대신 n-gram의 개수를 6-12 gram으로 하는 등 바꿈. ex) 대한민국=>대한, ㅐ한ㅁ, ... 이런식



---



# [2강] 언어 모델(Language Model)

언어 모델: 자연어의 법칙을 컴퓨터로 모사한 모델



### Markov 확률 기반의 언어 모델

초기의 언어 모델은 다음 단어나 문장이 나올 확률을 통계와 단어의 n-gram을 기반으로 계산

딥러닝 기반의 언어모델은 해당 확률을 최대로 하도록 네트워크를 학습

![markov](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/markov.jpg?raw=true)



## RNN 기반의 언어 모델

### RNN의 구조적 문제점

* 입력 sequence의 길이가 매우 긴 경우, 처음에 나온 token에 대한 정보가 희석

* 고정된 context vector 사이즈로 인해 긴 sequence에 대한 정보를 함축하기 어려움

* 모든 token이 영향을 미치니, 중요하지 않은 token(조사 등..)도 영향을 줌



## Attention 모델

### RNN계열에 Attention 적용한 경우(RNN+Attention)

* 인간이 정보처리를 할 떄와 마찬가지로 중요한 feature는 더욱 중요하게 고려하는 것이 Attention의 모티브

기존 seq2seq에서는 RNN의 최종 output인 context vector만 활용

(즉, 앞 부분의 단어는 hidden vector로만 전달되고, output은 무시되었음)

Attention에서는 인코더 RNN 셀의 각각 output을 활용

Decoder에서는 매 step마다 RNN 셀의 output을 이용해 dynamic하게 context vector를 생성



Attention 모델을 이용한 딥러닝의 시각화 ---> 나중에 모델 만들고 발표할 때 쓰기 좋아보임

ex) NMT(Attention for neural machine translation), STT(Attention for speech to text)



장점: 문맥에 따라 동적으로 할당되는 encoder의 Attention weight로 인한 dynamic context vector를 획득해 기존 seq2seq의 encoder, decoder의 성능을 비약적으로 향상시킴

한계: 여전히 RNN이 순차적으로 연산이 이뤄짐에 따라 연산속도가 매우 느림



### Self-Attention

Attention is all you need 논문에서 소개

RNN을 encoder와 decoder에서 제거

위의 RNN 계열에 Attention을 적용한 것의 한계를 극복함



기존의 RNN + attention:

* decoder 결과가 정답과 많이 다르다면 좋지 못한 context vector -> 좋지 못한 attention weight

->Fully connected feed forward network에서 score를 조정 -> softmax 값이 조정됨에 따라 attention score도 조정됨

* RNN+attention에 적용된 attention은 decoder가 해석하기에 가장 적합한 weight를 찾고자 노력

만약, Attention이 decoder가 아니라 input인 값을 가장 잘 표현할 수 있도록 학습하면? -> 자기 자신을 가장 잘 표현할 수 있는 좋은 embedding-> self attention 모델의 탄생



Self-Attention의 과정

![selfattention1](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/selfattention1.jpg?raw=true)

![selfattention2](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/selfattention2.jpg?raw=true)

![selfattention3](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/selfattention3.jpg?raw=true)

이를 정리해서 보면 아래 사진과 같다.

![selfattention4](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/selfattention4.jpg?raw=true)

BERT는 multi-head attention: 12개, 앙상블 효과 노림..

Transformer 모델은 multi-head attention으로 이루어진 encoder를 여러 층(6개) 쌓아서 encoding을 수행



# [3강] BERT

BERT는 bi-directional Transformer로 이루어진 언어모델

잘 만들어진 BERT 언어모델 위에 1개의 classification layer만 부착하여 다양한 NLP Task를 수행



### 데이터의 Tokenizing

**WordPiece tokenizing**

입력 문장을 tokenizing하고, 그 token들로 'token sequence'를 만들어 학습에 사용

2개의 token sequence가 학습에 사용(두 문장)

WordPiece tokenizing: Byte Pair Encoding(BPE) 알고리즘 이용

빈도수에 기반해 단어를 의미 있는 패턴(subword)으로 잘라서 tokenizing

* BPE의 순서도

![bpe_process]()

학습 데이터를 이용해서 vocab을 만들기 시작하는데, 빈도수가 많이 등장하는 char들의 묶음을 계속 반복하면서 vocab 후보를 계속 업데이트 함



vocab만드는 과정

![wordpiecetokenizer1]

![wordpiecetokenizer2]

![wordpiecetokenizer3]

![wordpiecetokenizer4]

![wordpiecetokenizer5]



### Bert의 학습

input text를 예측하는 방식으로 학습이 이루어지게 된다.

마스크된 단어조차도 원래 단어를 예측하게 학습이 이루어진다.

두번째 학습은 isnext, notnext인지 레이블 값 예측



**Training options**

Batch size: 256 sequences(256 sequences * 512 tokens = 128000 tokens/batch)

steps: 1M

Epoch: 40 epochs

Adam lr: 1e-4

weight decay: 0.01

drop out probability: 0.1

Activation function: GELU



Environment setup

BERT_base: 4 Cloud TPUs (16 TPU chips total)

BERT_large: 16 Cloud TPUs(64 TPU chips total) ≒ 72 P100 GPU

Training time: 4 days





classification task시 4가지 방법 사용

sentence pair classification: 앞문장과 뒤 문장이 비슷한지, 자연스러운지 분류



question and answering: index 출력



BERT 적용 실험: 문장 유사도

Sent2vec + Bert Multi-lingual이 성능이 가장 좋았다.



tokenizing할 때 음절 단위로 자르는 게 한국어는 더 좋음



# [4강] 한국어 BERT

ETRI에서 공개한 korBERT

ETRI KorBERT의 입력은 형분석 이후 데이터가 입력되어야 함. wordpiece 알고리즘은 동일한데, 형태소 태그를 다 붙여 놓은 다음에 tokenize. 동음이의어 구분에 용이



형태소 분석 라이브러리로 Mecab이나 Khaii 등 있는데, ETRI에는 Khaii가 적합하다.



BERT 성능에 영향을 미치는 요인

* corpus 사이즈
* corpus 도메인
* corpus tokenizing(어절, BPE, 형태소)
* vocab 사이즈(영어 model: 30522, 다국어 모델:119547)
* 전처리...데이터 퀄리티가 제일 중요하다

​    (XLnet: aggresive하게 heuristic한 필터링...)











# [5강] BERT 실습



## Reference

BERT 톺아보기 http://docs.likejazz.com/bert/ -> 시각화에 대한 아이디어도 많음

 
