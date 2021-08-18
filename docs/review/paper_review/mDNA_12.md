---
layout: default
title: "[NLP] Memory-Based Deep Neural Attention (mDNA) for Cognitive Multi-Turn Response Retrieval in Task-Oriented Chatbots 리뷰"
date: 2021-08-15
parent: Paper Reviews
grand_parent: Reviews
nav_order: 12
use_math: true
comments: true
---





# 논문 키워드

`mDNA`, `Bi-LSTM`, `memory`, `attention`, `dialog-system`, `retrieval`

# 논문 주제 





# 데이터셋

Ubuntu V2





# 내용 요약

---

## Abstract

최근 많이 사용되고 있는 Transformer based 모델들은 generative chatbot에는 잘 작동하지만 Retrieval-based model에서는 답변이 비일관적이거나 모호한 에러를 만들 수 있다고 한다. 그 원인은 'information loss'에 의한 것이다. 즉 트랜스포머로도 multi-turn 대화로 인한 정보의 소실을 막기 힘들어 정보 소실이 있다는 것이다. 본 논문에서는 Transformer-based retrieval chatbot을 memory-based deep neural attention(mDNA)를 이용해서 강화한다. mDNA는 Bi-LSTM, attention mechanism, memory for information retention in the encoder를 포함하는 간단한 encoder-decoder 모델이다.

## 1. Introduction

![mdna1.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c4e627fa-ecf6-41c7-aef7-5ba4141d8457/mdna1.jpg)

본 논문에서 사용한 모델 아키텍처의 선정 이유를 밝혀준다.

- **Retrieval model :** Seq2seq generative chatbot이 자주 사용되는 open domain과 달리 vertical domain에서는 retrieval-based system이 더 많이 사용된다.

- **Multi-turn dialogue :** DAM과 같은 최근 연구들은 multi-turn dialogue를 이용하는 것이 response selection에 있어서 더 좋은 성능을 가져온다는 것을 보여주었다. 이는 사람들의 대화 매너는 종종 이전 대화의 컨텍스트와 감정에 영향을 받는 다는 믿음에 기반한다.

- **mDNA(Memory-Based Deep Neural Attention)**

  - 최근  Transformer 모델 기반 SOTA multi-turn retrieval chatbot의 문제
    1. logical inconsistency error : 논리적 미스매치로 인한 retrieved response의 오류
    2. vague or fuzzy response candidate error: 모호한 대답 (i.e. I don't know)
  - mDNA는 Bi-LSTM 인코더에 attention 매커니즘을 결합한 DNN으로 위의 문제들을 해결

  mDNA는 메모리 모듈이 encoder에 적용되었는데 이는 중요 발화 워드 임베딩을 포착하고 저장해서 인지능력을(cognitive-like) 제공하기 위해서이다. 본 연구에서는 DAM 모델을 트랜스포머 기반 Retrieval 모델로 사용하고 이를 mDNA와 결합하여 unimodal late fusion method와 비슷하게 구현하였다.

장기 문맥을 형성하기 위해 (to match long-range contextual dependencies) retrieval process 동안 이전의 multi turn 대화 정보는 mDNA Bi-LSTM 인코더로 어텐션 매커니즘을 거쳐서 들어가고, 이후 DAM과 mDNA를 거친 응답 후보들은 소프트맥스를 거쳐 선택된다.



## 2. Related Work

현재 트랜스포머가 gated-recurrent 신경망의 대체제 취급을 받지만 novel retrieval chatbot 모델에 트랜스포머-어텐션 아키텍처를 적용한 모델들에는 위에서 언급한 문제들이 있다. 따라서 이 논문에서는 메모리를 gated RNN-based 모델에 적용하면 트랜스포머와 같은 성능 달성할 수 있을 거라 생각하여 메모리, gated-RNN, 트랜스포머의 어텐션 요소를 하나로 합치는 것을 제안한다.



## 3. Model Description

![mdna2.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3de337c5-9f84-478a-b83b-9dd38f830290/mdna2.jpg)

DAM과 mDNA를 합친 모델의 구조이다. 본 논문에서는 DAM의 구조 설명은 전혀 하지 않고 있는데, Figure 2.를 보면 트랜스포머의 인코더를 2개 쌓은 모양으로 보인다. 아래의 모델 설명은 mDNA 부분만 설명하며 1) 메모리 발화 임베딩, 2) 어텐션을 가미한 Bi-LSTM 인코딩, 3) 메모리와 응답 매칭 및 예측, 세가지 파트로 이루어진다.

**DAM 간단한 설명[(논문)](https://aclanthology.org/P18-1103.pdf)**

**DAM(Deep Attention Matching Network) :** self-attention과 cross-attention을 사용한 neural matching network이다. DAM은 발화나 응답의 모든 단어들을 모호한 semantic segment의 중심 의미로 사용하고, stacked self-attention을 통해서 계층적으로 representation을 개선(enrich)한다. 점진적으로 중심 단어를 둘러싼 segment representation을 정교하게 만든다. 각 발화와 응답은 관령성과 의존하는 정보에 기반해서 segment pair로 매치된다. 이 과정을 통해 DAM은 발화와 응답의 일치하는 정보를 찾아내게 된다.

(중심단어를 사용한다는 게 Word2Vec 얘기일까요..? DAM 논문을 다 읽어봐야 할 것 같습니다.)



### 3.1 Memory Utterance Embedding

이 논문에서는 중요한 발화를 기억하기 위해 메모리 유닛을 통해 발화 입력을 임시 저장하는 mDNA를 고안했다.

**mDNA의 구조**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b92dc2d3-ecc9-4d5f-b628-0746af164536/Untitled.png)

제가 이해한 바로는 Word2Vec으로 추출한 input `발화`의 임베딩(m)을 미리 학습된 워드 임베딩 테이블을 통해서 2개의 인풋 시퀀스를 만들어 냅니다. 이 두 개의 인풋 시퀀스는 M과 R로 나타냅니다. 메모리에 저장할 중요 발화를 필터링하기 위해 soft attention ($\alpha_M$)를 사용하는데 softmax 함수를 이용해서 어텐션을 구합니다.  `응답` 임베딩(r)은 바로 Bi-LSTM 인코더로 들어갑니다.

**m** : word2vec을 이용해 추출된(parsed) 인풋 발화 임베딩을  m으로 나타냄.
$$
m = (m_1,m_2,...,m_n)
$$
**r**: 응답 임베딩은 바로 BI-LSTM 인코더로 들어가고 이는 r로 나타냄.
$$
r = (r_1,r_2,...,r_n)
$$
**M, R** : pre-trained된 워드 임베딩 테이블을 이용해서 두 개의 인풋 시퀀스 M과 R을 생성.
$$
M = [e_{m,1},...,e_{m,n}], \quad R = [e_{r,1},...,e_{r,m}]
$$

$$
\alpha_M
$$
: soft attention. 중요한 인풋 발화를 필터링하는 어텐션. soft attention은 우리가 보통 아는 어텐션인데 모든 구간에서 미분 가능하며 그래디언트 전달이 가능한 어텐션이다. (hard attention은 히든 스테이트를 샘플링하며, 그래디언트 추정을 위해 몬테카를로 sampling 사용 )
$$
\alpha_M = softmax(W,e_{m,l})
$$
**W** : 가중치
$$
e_{m,l}
$$
 : 발화의 벡터 표현(m)과 각 단어의 길이(l)



### 3.2 Bi-LSTM Encoding with Attention

Bi-LSTM 사용 이유: LSTM이 GRU보다 깊은 문맥 이해에 좋은 성능을 보여줘서[(다른 논문의 연구결과 인용)](https://www.researchgate.net/publication/342598970_Are_GRU_Cells_More_Specific_and_LSTM_Cells_More_Sensitive_in_Motive_Classification_of_Text)

* LSTM: RNN의 그래디언트 소실, 폭발 문제 해결
* Bi-LSTM: 2개의 히든 레이어를 양방향에 넣어서 과거와 미래의 인풋 feature들을 같은 output에 연결 - 빠른 학습
* LSTM: GRU보다 좋은 성능

위의 3.1에서 설명한 m, r를 Bi-LSTM에 넣어서 contextual vector의 토큰들을 표현한다. 이때 히든 스테이트의 벡터들을 $m^s, r^s$ 로 표현한다.
$$
m^s_i = BiLSTM(M,i) \\ r^s_j = BiLSTM(R,j)
$$


3.2는 선정한 응답 후보들이 발화의 컨텍스트에 맞는 적절한 응답인지 알아보기 위해 발화-응답의 관계를 모델링하는 단계이다.

모든 단어들이 발화의 컨텍스트 표현에 동일한 영향을 주는 것이 아니기에 어텐션 매커니즘을 사용할 것이고, 본 논문에서는 multi-head 어텐션을 사용해 발화의 컨텍스트를 응답 후보들에 맞추고(align) 발화 단위별로 semantic 관계를 계산한다. 또한 soft alignment 레이어를 적용해 어텐션 가중치를 계산한다.
$$
e_{ij} = (m_i^s)^T*r^s_{j'}
$$

$$
e_{ij}
$$
는 발화와 응답의 관계를 나타낸다. 즉, 발화 컨텍스트의 히든 스테이트 $m^s_i$에 연관된 응답 후보들에 있는 관련된 semantic을 발견하고 구축해가는 과정이다.

이제 어텐션 가중치 벡터를 정규화 하면 아래의 식과 같다.
$$
\alpha_{ij} = exp(e_{ij}) / \sum^n_{k=1}exp(e_ik), m_i^d=\sum^n_{j=1}\alpha_{ij}r^s_{j'}
$$

$$
\beta_{ij} = exp(e_{ij})/\sum^w_{k=1}exp(e_{kj}, r^d_j=\sum^w_{i=1}\beta_{ij}m^s_{i'}
$$

alpha, beta는 정규화된 어텐션 가중치 벡터들로 각 발화와 응답에 대해 이를 계산한다. 이를 통해서 짝지어진 발화-응답 쌍의 단위별 의미 관계를 구할 수 있다.



### 3.3 Matching Composition and Prediction

최종 응답을 예측하는 과정에서 Bi-LSTM은 dense layer에서 매칭 벡터를 생성하게 된다. 이제 DAM과 mDNA의 representation (각각 f1, f2)을 얻었기 때문에 이를 이용해서 매칭 스코어 g를 구하면 된다.
$$
g(m,r) = softmax(W_2[f1,f2]+b_2)
$$


## 4. Experiments

### 4.1 Datasets

Ubuntu Dialogue corpus

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b33ce3a1-9b26-4b4c-af08-41b596894c4b/Untitled.png)

훈련을 위해 Ubuntu Dialogue Corpus를 사용하였는데, 이는 multi-turn 대이터셋으로 100만개의 발화-응답 쌍으로 이루어져 있으며 응답을 긍정/부정으로 분류하는 라벨이 붙어있다. train, validation, test set의 구성은 아래와 같다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b7b98101-93e8-41f8-a209-2a004c63fd9d/Untitled.png)

### 4.2 Hyperparameter Settings

- train set의 워드 임베딩을 pre-train하기 위해 word2vec을 사용
- Bi-LSTM의 히든 사이즈는 300
- training epoch: 15 with early stopping
- 배치 사이즈: 16
- optimizer: Adam
- vocab size: 100,000 - Ubuntu dataset이 크기 때문에 vocab 사이즈도 크게 설정

## 5. Results and Evaluation

**평가지표**: 
$$
R_n@k
$$
응답 후보 n개 중에서 top-k개 응답의 recall값
$$
R_n@k = \frac{TP}{TP+FN'}
$$


information retrieval task에서 자주 사용되는 평가지표라고 한다.

TP: 응답 후보 중에서 진짜 응답이 있는 경우를 true positive로 봄

**다른 모델과의 비교**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b6ba2682-3341-4005-8fab-1e352d04abe3/Untitled.png)

다른 모델과 비교해봤을 때 mDNA 모델이 대체로 우수한 성과를 보였다. 하지만 BERT Bi-Encoder의 경우 mDNA 모델과의 성능 차이가 미미했는데, 이는 BERT Bi-Encoder+CE 모델이 BI-GRU의 장점과 Bi-encoder 시스템의 장점을 활용했기 때문이라고 생각된다. mDNA_self는 DAM을 제거한 mDNA 모델이다.

(이 부분을 읽고나니까 그냥 BERT Bi-Encoder를 쓰는 것이 더 나아보이네요...mDNA라는 메모리를 결합한 복잡한 구조의 모델을 붙였는데도 성능차이가 별로 안나서 조금 아쉽습니다.)

다른 실험으로는 발화 임베딩의 max length에 따라 모델 성능이 달라지는지와 훈련 epoch에 따른 성능 차이를 살펴봤는데, 임베딩의 길이 차이는 성능에 영향을 미치지 못했지만 epoch은 3,6,9,15로 증가할수록 모델 성능이 계속 개선되었다.

## 6. Conclusions

transformer 기반 retrieval 챗봇을 개선하기 위해 mDNA를 사용하였고, 그 결과 응답 선정의 정확도와 일관성에 있어서 성능 개선이 있었다. 추후 과제로 Bi-LSTM 인코더를 사용하여 multi-model 인풋을 받는 모델을 연구해 볼 것이다.



------

메모리를 이용한 멀티턴 챗봇 모델에 관한 논문이어서 읽었는데, 장기 기억이라기 보다는 (논문에서 얼마나 길게 기억을 하는지는 안 나와 있지만) 한 대화 세션 동안의 중요 발화를 기억하는 모델인 것 같습니다. 그래도 메모리에 저장할 중요한 발화들을 필터링하기 위해 소프트 맥스를 이용한 어텐션값을 구하는 것은 참고할 만 하네요. [(참고)](https://wikidocs.net/22893#2-softmax-attention-distribution)







## 느낀점



# References

