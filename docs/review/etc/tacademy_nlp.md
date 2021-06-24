---
layout: default
title: "T academy 딥러닝을 활용한 자연어 처리 기술 실습"
date: 2021-06-21
parent: ETC
grand_parent: Reviews
nav_order: 5
comments: true
---



ddd

[딥러닝을 활용한 자연어 처리 기술 실습](https://tacademy.skplanet.com/live/player/onlineLectureDetail.action?seq=123)을 듣고 정리한 내용입니다. ㅁ



# [1강] 딥러닝 및 자연어처리 소개

## 자연어 처리의 목적

**이해하고 :** 문서 이해, 발화 이해, 질문 이해

**답하기:** 검색, 추론, 분류

이해하는 기술은 이제야 발전하는 중이다.



## 20년간 자연어 처리 문제

![nlp_20_years](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp_20_years.jpg?raw=true)

######    1강 딥러닝 및 자연어처리 소개 

도메인과 상관없이 공통적으로 사용할 만한 기술: 개체명 분석, 구문분석, 의존구문분석, 형태소분석 

개체명 분석만 잘 해도 어플리케이션 만드는 것이 쉬워진다..?

응용 단에 있는 task를 하기 위해서는 NLP stack 부분에 있는 기능을 다 구현해야함--옛날 방식. 하지만 딥러닝 같은 종단간 학습은 굳이 중간 단계가 없어도 됨.



## 어떤 형태로 풀 것인가?

대부분의 딥러닝 문제는 Classification 혹은 Clustering 문제로 바라보고 시도한다. Reinforcement Learning는 아직 성숙 단계는 아니다.

데이터 마이닝은 Clustering을 더 많이 쓰고 대부분의 문제는 Classification을 많이 쓴다.



Supervised Learning: 예제 문제를 풀게 하고 정답과 비교하여 잘 맞추는 방향으로 학습한다.

Classification: 정해진 클래스에 score를 부여한 후 가장 높은 scoring을 가진 클래스를 선택한다.



* score의 의미는?

다른 클래스 대비 상대적 우월 정도를 나타낸다. 그래서 절대적인 비교 불가능

* 모르는 것에 대해 모른다고 이야기 할 수 있을까? SQuad 2.0가 생각나네

기계는 정해진 클래스 안에서만 분류를 하기 때문에 모르는 것을 모른다고 하지 못한다.

* 어떻게 정답을 맞추는 방향으로 학습을 시킬 수 있을까?

1) Reference Representation : 정답을 어떻게 표현할 수 있을까? 

One-Hot Representation: 감정 극성을 하나만 가지게 표현. [1, 0, 0, 0]

2) Scoring Normalization

모델을 통해서 예측한 값은 $ -\infin ~ \infin$ 값을 가진다. 이 점수들을 softmax 함수를 통해서 0과 1 사이의 값으로 정규화시켜준다. 이 때 모든 클래스의 normalized된 score의 값을 모두 더하면 1이 된다.

**예측**: softmax 어떤 값이든 양수 값으로 만들어서 0~1 사이의 값으로 매핑

**정답:** [1, 0, 0, 0]

3) Cost Function Design

손실함수: 이제 예측값과 정답, 두 점수 간의 차이를 어떻게 수치화할 수 있을까?

두 점수는 discrete space에서의 확률분포라고 볼 수 있다. 따라서 Cross Entropy를 통해서 두 값의 차이를 손실함수로 나타낼 수 있다.

Cross Entropy는 Kullback-Leibler Divergence의 식에서 나오는데, 아래의 식에서 H(P, Q)가 Cross Entropy 값이다.

의미: 쿨백-라이블러 발산은 어떤 확률분포 P가 있을 떄, 샘플링 과정에서 그 분포를 근사적으로 표현하는 확률분포 Q를 P 대신 사용할 경우의 엔트로피 변화를 의미한다. 따라서 원래의 분포가 가지는 엔트로피 H(P)와 P 대신 Q를 사용할 때의 교차 엔트로피(Cross Entropy) H(P,Q)의 차이를 구하면 아래의 식이 나온다.
$$
DL_{KL}(P||Q) = - \Sigma_xp(x)\log q(x) + \Sigma_{x}p(x) \log p(x)
$$

$$
=H(P,Q) - H(P)
$$

$$
H(p,q) = H(p) + DL_{KL}(P||Q)
$$

P: true distribution

Q: model distribution



이제 손실함수의 Cost Value를 줄이는 방향으로 학습을 한다.

Gradient descent(기울기)가 작아지는 방향으로 학습한다. global minimum을 찾아서...

하지만 local minima에 빠지기도 한다. 하지만 최근에는 plateau 때문에 학습이 잘 안 된다는 의견도 많다.



4) Parameter Update-Backpropagation Algorithm

최종 결과물을 얻고, 그 결과물과 우리가 원하는 결과물과의 차이점을 찾은 후, 그 차이가 무엇으로 부터 생기는지, 역으로 내려가면서 추정하여 새로운 parameter값을 학습

Feed Forward and Prediction-Cost Function-Differentiation(미분)-Back Propagation-Weight Update



### 미니 배치

몇 개의 example를 살펴보고 model을 업데이트할 것인가?

만약에 점 하나하나로 학습을 하면 전체 모델 학습에 적합하지 않다.

보통 배치 사이즈는 50~128 정도로 사용한다.

parameter update = 선을 다시 한 번 새로 긋는 것

모든 점(훈련 예제에 잘 적용되도록)
$$
\theta^{'} = \theta + \alpha \Delta
$$

[^ ]:alpha: learning rate, Delta: Update factor

* 클래스 개수가 1인 경우는?



활성화 함수: 비선형..

선형으로만 이루어진 레이어는 결국 선이 하나임. 레이어를 쌓으려면 활성화함수를 거쳐서 비선형적으로 만들어준다. (explanation power가 생김)

활성화함수: sigmoid, tanh, relu 미분이 가능해야 함.-> 역전파해야하니까

깊게 쌓으면 그래디언트 소실 때문에 활성화되어있는 뉴런이 줄어들음.



---

# [2강] Seq2Seq 소개

Seq2Seq: 대표적인 자연어처리의 틀

![seq2seq](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/seq2seq.jpg?raw=true)

Sequence Feeding -> model -> Sequence Generation 문제

Seq2Seq의 종류

![seq2seq_cases](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/seq2seq_cases.jpg?raw=true)



자동 완성도 seq2seq 모델

우리가 살고 있는 시계열 축에서의 문제에 적합한 경우가 많음



### RNN

연산이 아주 복잡함

dense하게 붙여 놓음

인코딩 approach

인코딩: 모든 시퀀스를 요약한 벡터 - RNN의 마지막 히든 레이어



### Classification Formulation

end-to-end model: input - model - output

데이터의 차원을 줄이는 방법? max, average 다양한 값, **weighted sum** 

fully connected layers들로 이루어진 모델

행렬 연산을 GPU를 써서 하기 때문에 빠름



Context 를 반영

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/projection_w_context.jpg?raw=true" alt="projectionwithcontext" style="zoom:80%;" />

입력 데이터를 concatenate 시키고 weight matrix를 하나로 만들어서 처리



### Sequence Encoding

여러 개의 데이터를 요약하는 방법



### Data Transformation에 'Temporal 정보'를 반영

Temporal 정보 = context, 부가정보

context vector를 target vector 앞에 붙이면 됨



RNN 종류

Forward RNN, Backward RNN, Bidirectional RNN(Forward RNN, backward rnn을 concat), stacking RNN  



Global Summarization: Attention mechanism

Attention without Context 

소프트맥스를 취하면 일종의 중요도 weight 같이 나오게 됨.-> 어텐션 스코어

어텐션 스코어를 각 엘레멘트 별로 곱해서 새 벡터 만들고 이를 다시 더함...

Attention with Context

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/attention_w_context.jpg?raw=true" alt="attentionwithcontext" style="zoom:80%;" />

어텐션 셀에 들어온 정보가 C(context) 

context를 반영하는 방법들: concat, dot

보통 concat해서 하긴 하지만 다양한 방법이 있다.

![context](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/context%EB%B0%98%EC%98%81%EB%B0%A9%EB%B2%95.jpg?raw=true)

---



# [3강] 자연어처리 구현 실습 (1)



# [4강] 자연어처리 구현 실습 (2)


