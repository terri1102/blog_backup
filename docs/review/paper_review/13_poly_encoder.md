---
layout: default
title: "[NLP] Poly-encoders: Architectures and pre-training
strategies for fast and accurate multi-sentence scoring 리뷰"
date: 2021-08-11
parent: Paper Reviews
grand_parent: Reviews
nav_order: 13
comments: true
use_math: true
---





#  🗝️ 논문 키워드

`encoder`, `poly-encoder`, `bi-encoder`, `cross-encoder` 



# 📑 논문 주제

Facebook AI Research팀에서 Bi-encoder보다 성능을 개선하고, Cross-encoder보다 예측 시간을 개선한 Poly-encoder 제안



# 📚 데이터셋

* Reddit

* Wiki + Toronto books corpus

  

# 내용 요약

## Abstract

Pairwise comparison이나 주어진 인풋을 관련 라벨과 매칭하는 테스크에 트랜스포머 기반 아키텍처를 사용한 2가지 방법이 주로 사용되었다. 그 중 <span style="background:#e8f7ff">**Cross-encoder**</span>는 시퀀스 쌍별로 self attention을 수행하는 방법으로 성능은 좋지만 Inference가 느리다는 단점이 있었고, <span style="background:#fff28c">**Bi-encoder**</span>는 시퀀스를 각각 인코딩하여 Inference는 빠르지만, Cross-encoder보다 성능이 떨어졌었다. 본 논문에서 제안하는 <span style="background:#ffe4de">**Poly-encoder**</span>는 토큰 레벨이 아닌 전역적으로 self attention 피처를 배우기 때문에 cross-encoder보다 빠르고 Bi-encoder보다 정확하다. 그 외 발견으로는 사전학습(pre-training) 단계에서도 다운스트림 테스트와 비슷한 데이터셋으로 pre-training할 때 성능이 좋다는 점을 소개한다.



* **Alias 정리:** 논문에서 Input context와 Candidate label을 가리키는 단어들이 다양하게 사용되기에 정리해두겠다.
  
  * Input context (인풋) : Input, Context(주로 임베딩된 것 의미)
  
  * Candidate (응답 후보) : candidate, response
  
  * Candidate label(라벨) :  label (정답인 문장) 
  
    

## 1. Introduction

**Multi-sentence scoring** : Input context가 주어졌을 때 응답 후보들의 점수를 매기는 것으로 Retrieval and dialogue task에 많이 사용된다. 이 Multi-sentence scoring 분야에서 최근 SOTA 모델들은 사전학습된 BERT모델들을 사용한 Bi-encoder나 Cross-encoder가 많이 이용된다. 



<span style="background:#e8f7ff">**Cross-encoder**</span>는 full (cross) self-attention을 주어진 인풋과 라벨 후보별 로 진행하기 때문에 성능이 좋지만 느리다.
<span style="background:#fff28c">**Bi-encoder**</span>는 인풋과 라벨 후보를 따로 self-attention을 진행한 후 이를 합쳐서 final representation을 만든다. 인코딩된 응답 후보들을 캐싱해서 이를 인풋에 조합할 때 재사용가능하기에 빠르다.

본 논문에서 소개하는 <span style="background:#ffe4de">**Poly-encoder**</span>는 self-attention을 수행하기 위해 전역적인 특성을 표현하는 추가로 학습한 attention 매커니즘을 사용하여 Bi-encoder보다 좋은 성능을 보여주었다.

위의 모델들을 비교하기 위해서 Dialogue and information retrieval(IR)에서 사용되는 4가지의 데이터셋을 통해 모델을 검증하는데 Reddit과 Wikipedia/Toronto Books로 각각 사전학습시켜서 다운 스트림과 비슷한 데이터로 사전학습했을 때의 결과를 비교한다. (wiki 데이터로 학습한 BERT와 다르게 대화형 데이터인 레딧 데이터 사용해 대화형 시퀀스 스코링다운스트림 테스크에 적용함) 우리의 모델 아키텍처와 새로운 사전학습을 통해서 새로운 SOTA를 달성했다. 

## 2. Related Work

머신러닝에 있어서 다양한 후보들을 스코어링하는 것은 아주 고전적인 문제이다. 이를 이산적인 multi-class 분류의 특수한 경우로 볼 수도 있지만, multi-class 분류를 좀 더 일반화된 형태의 응답 후보들이 클래스가 아닌 구조체인 경우로 볼 수도 있다. 이 연구에서 우리는 인풋과 후보 라벨(candidate label)을 텍스트 시퀀스로 고려할 것이다.

<span style="background:#fff28c">**Bi-encoder**</span> 
Input과 candidate를 각각 common feature space에 매핑한 후에 이 둘의 유사성을 내적이나 코사인 유사도를 통해서 구한다. 즉 임베딩 벡터가 Input과 candidate에서 각각 나온다. 논문에서 Next utterance prediction task를 위한 Bi-encoder 접근으로 메모리 네트워크, 트랜스포머 메모리 네트워크, LSTM, CNN 등을 고려했었다. Bi-encoder의 장점은 크고 고정된 사이즈의 candidate set을 캐싱한 후에 사용하여, input과 독립적이기에 evaluation 과정에서 연산 효율적이다.

<span style="background:#e8f7ff">**Cross-encoder**</span>
Input과 candidate를 concat한 후에 self-attention을 적용한다. 따라서 두 문장의 임베딩 간의 관계(유사도)를 구하지 않고 두 문장을 합친 것이 활성화 함수로 들어가서 하나의 임베딩을 얻게 된다. 이 경우 candidate들의 모든 단어와 input의 모든 단어 간의 상호작용을 알 수 있어서 Input sequence와 candidate sequence들의 관계를 잘 나타낼 수 있다. 

## 3. Tasks

* **ConvAI2 테스크**

![convai2](https://paperswithcode.com/media/datasets/ConvAI2-0000003063-95759eda.jpg)훈련할 때 5개 문장의 페르소나가 주어지고, 여기에 따른 대화 데이터가 제공된다. 평가시에는 20개의 선택지 중에 가장 적합한 응답 후보를 선택해야하는데, 이때 19개의 응답 후보는 트레이닝 셋에서 랜덤하게 추출한 것이다. 

* DTSC7 : Ubuntu chat log에서 추출된 대화들이며, 기술적 도움을 주고 받는 대화이다. 
* Ubuntu V2 : DTSC7과 비슷하지만 더 큰 사이즈의 데이터셋이다.
* Wiki Article : 한 아티클에서 문장이 쿼리로 주어지고, 이 문장이 나온 아티클을 찾는 테스크이다. 서치 쿼리를 통해 가장 관련있는 아티클을 검색하는 것으로,웹 검색과 비슷한 구조이다. 이 테스크에서는 StarSpace라는 모델이 가장 좋은 성능을 보였다.

**Dataset 설명**

![poly2](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/polyencoder2.jpg?raw=true)

## 4. Methods



### 4.1 Transformers and Pre-traininig Strategies

* **Transformers**

비교를 위해서 2가지 데이터셋을 이용해 트랜스포머를 훈련 

1\) BERT-base와 비슷하게 위키피디아와 토론토 북스 코퍼스에서 추출한 input, label을 스페셜 토큰[S]로 감쌈 <br>

2\) Reddit의 경우 input은 context이고 label은 next utterance


output은 3가지 임베딩의 합! (token embedding, position embedding, segment embedding) -> bert와 동일한듯

* **Input Representation**

pre-training input은 input과 label의 concat한 것. 모두 스페셜 토큰 [S]로 둘러싸여 있음(Lample&Conneau,2019) 참조.

* **Pre-training Procedure** : BERT와 동일한 MLM방식으로 사전학습. Wikipedia+Toronto books는 NSP테스크도 수행. Reddit의 경우 Next utterance prediction task로 수행. NSP와의 차이점은 Next utterance는 여러 문장일 수 있다는 것.  
* **Fine-tuning** : 3가지 아키텍처를 이용해서 미세조정했다.

### 4.2 Bi-encoder

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/bi-encoder.jpg?raw=true" alt="bi-encoder" style="zoom:100%;" class="center"  />



이제 위의 4.1에서 사전학습한 인코더를 Fine-tuning 과정에 대한 설명이다. 
Bi-encoder의 context embedding인

$$
y_{ctxt}
$$

는 Input을 입력받아서 벡터로 인코딩된 후(T1(ctxt))에 red(·) 함수를 거쳐서 하나의 벡터로 축소된 **임베딩 벡터**이다. (그림에서 ctxt Emb)


$$
y_{ct xt} = red(T_1(ctxt)), \quad y_{cand}=red(T_2(cand))
$$


* Context encoder : Bert와 동일하게 3가지의 값을 더해서 임베딩을 구함. 이때 input과 candidate는 각각 인코딩되기 때문에 segment tokens은 모두 0이다. 사전학습에서 비슷한 환경을 모방하기 위해 input과 candidate는 모두 스페셜 토큰 [S]로 둘러 싸인다. 

* T1, T2는 두 pre-trained된 트랜스포머로 둘 다 같은 가중치로 시작하지만 파인튜닝 과정 동안 달라질 것이다.

* T(x) = h1,...,h_n : 트랜스포머의 output (out1 == h1 == [S]의 임베딩값)

* red(·) : 벡터들의 시퀀스를 하나의 벡터로 축소시키는(reduce) 함수

  red로 사용할 함수로 

  1. 트랜스포머의 첫 번째 output을 취하는 함수

  2) 모든 output의 평균값을 취하는 함수
  3) 첫 m개의 output의 평균값을 취하는 함수 (m<N)

  를 고려했었는데, 미세하게 성능이 가장 좋은 1번 함수를 채택했다. 다른 함수를 사용했을 때의 결과는 Appendix에서 볼 수 있다.


Bi-encoder는 input과 candidate를 각각 인코딩하기 때문에 candidate의 representation은 캐싱한 후에 inference할 때 가져와서 사용할 수 있다. 


* **Scoring**

cand_i의 점수는 


$$
s(ctxt,cand_i)=y_{ctxt y}· y_{cand_i}
$$


Input의 임베딩과 candidate의 임베딩을 내적한 값이다. 모델은 cross-entropy loss를 최소화하는 방식으로 훈련되었다. 

로짓은 cand_i가 맞는 라벨이고, 나머지는 트레이닝 셋에서 가져옴.

훈련 과정 동안 다른 candidate들은 negative로 취급해서 훈련 속도를 높였다.(정답 한 개) 그리고 각 candidate별로 임베딩을 구해놓은 다음에 재사용하기 때문에 큰 배치 사이즈를 사용할 수 있게된다. 그래서 ConvAI2에서는 배치 사이즈가 512이다. 



* **Inference speed**

Inference 과정에서 이미 구해진 candidate들의 임베딩을 사용하기에, 남은 연산은 input 임베딩인 
$$
y_{ctxt}
$$
가 계산된 후에 이 둘의 내적을 구하는 것 뿐이다. 따라서 inference가 매우 빠르다. 

### 4.3 Cross-encoder

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/cross-encoder.jpg?raw=true" alt="crossencoder" style="zoom:100%;" class="center" />



input과 candidate는 사전학습 방법과 비슷하게 스페셜 토큰[S]로 둘러싸인다음에 concat되어 하나의 벡터가 된다. 이 벡터는 하나의 트랜스포머 인코더를 거쳐서 output으로 나온다. 우리는 첫 번째 벡터를 context-candidate 임베딩으로 취한다. 


$$
y_{ctxt,cand} = h_1 = first(T(ctxt,cand))
$$


이 방법을 통해 Cross-encoder는 input과 candidate 간의 self-attention을 수행할 수 있어서, 더 풍부한 관계를 표현할 수 있다. 이렇게 candidate label이 트랜스포머 레이어를 거칠 때 input context에 접근할 수 있기에 candidate에 맞춰진 input representation을 얻을 수 있다. 


* Scoring

Candidate를 스코어링할 때 linear layer W는 임베딩 y에 적용되어 벡터를 스칼라값으로 축소한다.


$$
s(ctxt, cand_i) = y_{ctxt,cand_i}W
$$



Bi-encoder와 마찬가지로 Cross-entropy loss를 최소화하는 방식으로 훈련되는데, 

cand_1이 정답이고 나머지는 negative일 때의 로짓은
$$
s(ctxt, cand_1), ..., s(ctxt, cand_n)
$$
이다. Bi-encoder의 경우와 다르게 같은 배치의 다른 label을 negative로서 재사용할 수 없어서 트레이닝 셋에서 주어진 "external negative"를 사용한다. Cross-encoder는 BI-encoder보다 더 많은 메모리를 사용하기 때문에 작은 배치 사이즈를 사용한다.

### 4.4 Poly-encoder

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/poly-encoder.jpg?raw=true" alt="polyencoder" style="zoom:100%;" class="center" />

Poly-encoder는 Bi-encoder와 Cross-encoder의 장점만을 가져온다. Bi-encoder처럼 Poly-encoder도 Candidate를 하나의 벡터로 표현하며 이를 캐싱해서 빠르게 예측가능하다. 그리고 Cross-encoder처럼 Input이 candidate와 함께 인코딩되어서 더 많은 정보를 추출하는 것이 가능하다. 

Poly-encoder는 Bi-encoder처럼 input과 candidate을 각각 인코딩할 때 서로 다른 트랜스포머를 사용한다. Candidate는 하나의 벡터 
$$
y_{cand_i}
$$
로 인코딩되고 이를 캐싱하여 재사용할 수 있다. 

<span style="background:#f1d9ff">하지만 Bi-encoder와의 차이는 (보통 candidate 보다 긴)  input을 m개의 벡터들로 표현한다는 것이다. </span> 이는 하나의 벡터로 표현하는 Bi-encoder와 차이가 있다. m을 어떤 값으로 설정하는지에 따라 인퍼런스 속도에 영향을 준다. 

이 input을 표현하는 **m global context features**(m개의 글로벌 피처)를 얻기 위해서 우리는 **m context code**를 학습하는데, 
$$
c_i
$$
는 이전 레이어의 모든 outputs를 통해서 추출한 
$$
y^i_{ctxt}
$$
 representation이다. 

즉, 우리는 context vector 
$$
y^i_{ctxt}
$$
를 아래의 식을 통해 얻을 수 있다.



$$
y^i_{ctxt} = \sum_jw_j^{c_i} \quad where \quad (w_1^{c_i}, ...,w_N^{c_i}) = softmax(c_i· h_1,)
$$


**m context code**는 처음에 랜덤하게 초기화되고 미세 조정(fine-tuning) 과정에서 학습된다. 이렇게 구한 **m global features**에 
$$
y_{cand_i}
$$
를 candidate를  쿼리로 사용하여 attend하면 candidate의 최종 스코어를 구할 수 있다. 


$$
y_{ctxt} = \sum_i w_iy^i_{ctxt} \quad where \\ \quad (w_1, ...,w_m) = softmax(y_{cand_i}·y^1_{ctxt},..,y_{cand_i}·y^m_{ctxt})
$$


N이 토큰의 개수일 때 m은 N보다 작은 수이고, input과 candidate의 어텐션 연산이 탑 레이어에서만 수행되기 때문에 Poly-encoder의 연산은 Cross-encoder보다 훨씬 빠르다.



## 5. Experiments

본 연구에서 사용한 평가지표는 Recall@k과 MRR(mean reciprocal rank)인데, Recall@k는 예시 C개 중 선택하는k개 candidate의 Recall값을 의미한다.  

**Recall**


$$
\frac{TP}{TP+FN}
$$


**Recall@k** 


$$
Recall@k = \frac{(number \; of \; recommended \; items \; @k \; that \; are \; relevant)}{(total \; number \; of \; relevant \; items)}
$$




**MRR** : 정보 검색을 평가하는 평가 지표. 

각 Query별 관련있는 response 중 가장 높은 위치를 역수로 계산 (1/k)하고,  Query마다 계산된 점수를 모아 평균을 낸다.


$$
MRR = \frac{1}{|Q|}\sum_{i=1}^{|Q|} \frac{1}{rank_i}
$$



| Query | Proposed Results  | Correct response | Rank | Reciprocal rank |
| ----- | ----------------- | ---------------- | ---- | --------------- |
| cat   | catten, cati,**cats** | cats             | 3    | 1/3             |
|torus | torii, **tori**, toruses | tori | 2 | 1/2 |
|virus | **viruses**, virii, viri | viruses | 1|1|

라고 할 때 이 모델의 MRR은 
$$
\frac{(\frac{1}{3} + \frac{1}{2} + 1)}{3} = \frac{11}{18}
$$
이다. 

본 논문의 설정에 적용해보면 Query 대신 Input이 들어가고, response 대신 candidate를 대입하면 될 것 같다.




### 5.1 Bi-encoders and Cross-encoders

훈련 세팅: 8 Nvidia Volta v 100 GPU, half precision operation(float16 operation)

Bi-encoder와 Cross-encoder를 먼저 미세조정한 결과에 대해 살펴보면, BERT의 가중치로 초기화한 후 두 모델을 미세조정하였다. 

<span style="background:#fff28c">**Bi-encoder**</span> 

Bi-encoder의 경우 배치의 다른 candidate들의 임베딩을 재사용하기에 학습할 때 많은 negatives를 사용할 수 있다. (배치 사이즈를 크게 할 수 있다) 아래는 ConvAI2의 배치 사이즈를 32, 64, 128, 256, 512으로 훈련한 결과이다.

![polyencoder3](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/polyencoder3.jpg?raw=true)

위의 표를 보면 배치 사이즈가 커질수록 성능이 좋아지는 것을 알 수 있다. 다른 테스크는 시퀀스가 더 길었기 때문에 (메모리를 더 많이 차지해서) 배치 사이즈를 256으로 설정하였다.

<span style="background:#e8f7ff">**Cross-encoder**</span> 

Cross-encoder는 연산이 임베딩 쌍을 매번 계산해야 하기에 배치 사이즈를 16으로 제한하였다. 이때 negative는 트레이닝 셋에서 랜덤으로 샘플한 15개의 candidate로 하였다. 

**Optimizer**

* BERT 가중치로 초기화한 모델

Adam with weight decay 0.01 (BERT에서 사용한 설정)



* Reddit으로 훈련한 모델

Adamax without weight decay



**learning rate**

* Bi-encoder와 Poly-encoder는 5e-5, warmup of 100 iteration

* Cross-encoder는  5e-5, warmup of 1000 iteration

learning rate decay: 한 에포크의 절반 당 valid set의 loss 0.4 이상 개선이 없으면 decay 

아래의 표를 보면 워드 임베딩을 제외한 전체 네트워크를 미세 조정하는 것이 성능이 가장 좋게 나왔다. 



![polyencoder4](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/polyencoder4.jpg?raw=true)



아래 표는 Bi-encoder, Cross-encoder를 이용해 미세 조정한 결과를 나타낸다. 예상한 것과 같이 Cross-encoder는 기존의 SOTA 모델과 우리의 Bi-encoder를 뛰어넘는 성능을 보여주었다. 

![polyencoder5](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/polyencoder5.jpg?raw=true)

### 5.2 <span style="background:#ffe4de">**Poly-encoder**</span>

Bi-encoder에서 사용한 것과 동일한 배치 사이즈와 옵티마이저를 사용하였다. 위의 Table 4에서 m의 사이즈에 따른 실험결과가 나와 있다.

Poly-encoder는 모든 테스크에서 Bi-encoder보다 좋은 성능을 보여주었고, 코드가 더 많을수록 많은 성능 향상을 보였다. 우리의 제안은 시간이 허락하는 한 큰 코드 사이즈를 사용하는 것이다. (to use as large a code size as compute time allows) 

논문의 결과 발표 이후 ConvAI2에 대한 사람 평가가 이루어졌는데, 여기서도 본 논문의 Poly-encoder 아키텍처가 다른 모델들보다 좋은 성능을 보여줬다고 한다. 

### 5.3 Domain-specific Pre-training

레딧 데이터로 사전 학습한 트랜스포머를 4가지 테스크에 대해 미세 조정한 것과 BERT와 동일한 데이터로 사전 학습한 트랜스포머와 비교해보았다.  우리의 레딧 데이터로 학습한 모델은 Adamax 옵티마이저 사용하고 임베딩을 포함한 모든 레이어를 학습시켰다. 이때 weight decay를 사용하지 않았기 때문에 마지막 레이어의 가중치는 BERT의 마지막 레이어 가중치보다 훨씬 크다. 이로 인한 어텐션 레이어의 saturation을 막기 위해 마지막 linear layer를 BERT의 표준편차에 맞게 스케일링했다. 

 \<Table 4>를 보면 레딧 데이터로 사전 학습한 모델이 BERT 데이터로 사전학습한 모델보다 훨씬 좋은 성능을 보이는 것을 알 수 있다. 따라서 사전학습에 사용되는 데이터가 성능에 영향을 주며, 다운 스트림 테스크와 비슷한 데이터로 사전학습하는 것이 중요하다고 할 수 있다.

### 5.4 Inference Speed

Poly-encoder의 초기 목적이 Cross-encoder보다 빠른 예측 시간을 갖게 하는 것이었기에 inference speed을 살펴보면, GPU연산의 경우 100k개의 candidate일 때 Poly-encoder는 Cross-encoder보다 1000배 이상 빠르고, 실시간 예측이 가능한 정도의 시간이었다. 

![polyencoder6](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/polyencoder6.jpg?raw=true)

또한 Appendix의 Training Time을 보면  Cross-encoder보다 Poly-encoder의 훈련 시간이 3-4배 빠른 것을 확인할 수 있다. (Bi-encoder와 비슷한 수준)

## 6. Conclusion

본 논문에서는 응답 후보 선정 테스크를 수행하는 bidirectional transformer의 1) 새로운 아키텍처와 2) 사전학습 전략을 소개하였다. Poly-encoder는 각 candidate의 representation을 미리 계산하면서도 label candidate를 이용한 input context의 임베딩을 얻을 수 있게 하였다. 또한 프로덕션 레벨의 예측 시간과 정확도를 달성하였다. 사전학습 전략으로는 다운 스트림 테스크와 비슷한 테스크로 사전학습을 하면 더 좋은 성능을 보여준다는 것을 증명하였다. 레딧 데이터로 처음부터 사전학습를 진행한 모델이 BERT 데이터로 사전학습한 모델보다 좋은 성능을 보여주었다. 본 논문의 결과는 대화 데이터셋 뿐만 아니라 다른 information retrieval 등의 scoring 테스크에 적용될 수 있을 것이다. 



## Appendix

### A Training Time

8 GPU Volta 100을 이용해서 훈련한 4가지의 모델의 훈련 시간

![poly7](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/poly7.jpg?raw=true)

### B Reduction layer

![poly8](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/poly8.jpg?raw=true)

Bi-encoder로 ConvAI2를 미세 조정했을 때 reduce 함수의 종류에 따른 성능 차이.

### C Alternative Choices for Context Vectors

Poly-encoder의 output에서 context vector를 얻는 여러 방법들이 있다.

Convai2와 DSTC7의 valid set을 이용해 평가한 결과 m이 커질수록 성능이 좋아지고, First m outputs나 Last m outputs를 이용한 방법의 성능이 좋았다.



# 🍖Takeaway

* Information retrieval 테스크를 수행할 때 Query(Input)과 관련된 응답 후보들을 스코어링할 때 Poly-encoder를 사용하자. Retrieval-base 챗봇에서 응답 후보를 고를 때 충분히 사용 가능할 것 같다.

* 사전학습이 가능한 환경이라면 다운스트림과 비슷한 테스크로 사전학습하자. 또한 데이터 형태 뿐만 아니라 사전학습과 미세조정 과정에서 스페셜 토큰을 붙이는 것도 비슷하게 만들자. 

* MRR이라는 매트릭을 사용해보자. 여태까지 읽은 논문들에는 Recall@k나 Hit@k가 많이 사용되었는데, MRR도 (자동화된 평가지표로) 비슷하지만 괜찮은 지표 같다. 

  

# Reference

**Cross-encoder**

Thomas Wolf, Victor Sanh, Julien Chaumond, and Clement Delangue. Transfertransfo: A transfer learning approach for neural network based conversational agents. arXiv preprint
arXiv:1901.08149, 2019.

**Bi encoder**

Pierre-Emmanuel Mazar´e, Samuel Humeau, Martin Raison, and Antoine Bordes. Training millions of personalized dialogue agents. In EMNLP, 2018.

SBERT Bi-Encoder 

https://towardsdatascience.com/advance-nlp-model-via-transferring-knowledge-from-cross-encoders-to-bi-encoders-3e0fc564f554

Augmented SBERT

https://karter.io/sentence-bert-augmented-sbert
