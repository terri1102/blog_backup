---
layout: default
title: 2021년 9월 셋째주 회고
date: 2021-09-14
comments: true
nav_order: 18
parent: TIL
use_math: true
---



## 21.09.13

오늘 배운 것

* 자연어 태스크는 1. Token classification과 2. Sentence classification으로 나뉜다. Generative model도 token classification으로 볼 수 있을 듯.

* 텐서플로 파이프라인





---

## 21.09.14

### 오늘 배운 것

* Language model은 언어의 확률분포를 추론하는 것

  P(사과를 먹다) > P(사과를 막다)



### Language model 종류

* N-gram Language Model
* Fixed window 가진 Neural Language Model
*  

N-gram Language Model :샘플의 토큰 숫자를 세는 것만으로도 통계적으로 언어 분포를 추론할 수 있다.

I want to eat sushi
$$
p(sushi|I \;want \; to \; eat)\; \dot{} \; p(eat\;|I\; want\; to) ) \; \dot{} \;  p(to|I\; want))\; \dot{} \;  p(want|I)
$$

$$
p(w^{(i)},w^{(2)},...,w^{(i-1)} )=\prod^n_{i=1}p(w^{(i)}|w^{(1)},...,w^{(i-1)})
$$
계산을 좀 더 쉽게 하기 위해 고정된 몇 개(n개)의 이전 단어에 의존함



bi-gram은 n^2



보통 unigram, bigram, Trigram

bigram = p(w^{i}|w^{i-1})



n-gram 모델을 사용해서 text generation도 가능하다

그리디 서치로 다음 토큰 선택. 이렇게 만든 문장들은 문법적으로는 말이 되도 내용이 일관성이 없음.



#### fixed window neural language model

윈도우 위치에 따라 weight가 바뀌는 문제

긴 시퀀스 처리 어려움

-> 길이에 상관없이 처리 가능한 nn이 필요함



#### RNN

문장 앞 부분에  [BOS] 토큰을 줌. 

마지막 히든 벡터는 모든 시퀀스 정보를 갖고 있음 -> 이를 이용해서 단어의 확률 분포를 추론



한 단어의 로스의 평균: 크로스 엔트로피

-log : 확률이 클수록 로그값은 더 작아짐. 이건 로그함수를 봐야 이해 잘 됨(x가 1일 때 y가 0인 거 생각)



* Generation model

그리디하게 뽑지 않고 샘플링을 함. 그리디하게 뽑으면 항상 같은 단어 예측. [EOS] 토큰 나올 때까지 예측.(또는 최대길이까지) 



PPL

확률적으로 정답(다음 단어)을 얼마나 잘 예측하는가

PPL이 낮은 게 좋은 거긴 한데, 낮다고 해서 accuracy가 높은 건 아님. PPL은 참고용.


$$

$$
이전 단어를 보고 다음 단어를 예측하는데, 그 확률값의 역수를 단어 전체 개수 normalize. 확률값이 커지면 PPL이 줄어들음

로그 안에 곱하기 있으면 sum으로 바뀜 

log값은 loss함수값임





---

Language model은 subcomponent로 많이 쓰임









---

질문

1. PLM에 vocab 추가 가능? https://github.com/google-research/bert/issues/396
2. BERT에서 마지막 히든 벡터의 첫번째 벡터 이용한 sentence classification?



