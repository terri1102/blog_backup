---
layout: default
title: "NLP 태스크의 여러 평가지표(BLEU, PPL, ROUGE)"
date: 2021-10-13
grand_parent: Deep Learning
parent: NLP
comments: true
nav_order: 7
---



## BLEU



## ROUGE

ROUGE는 Recall Oriented Understudy for Gisting Evaluation의 약자로 주로 자동화된 요약 모델의 평가 지표로 사용된다. (roozh로 발음함). 주어진 문서 D와 모델을 통해 생성된 요약문을 X라고 할 때 

1. N명의 사람들이 D의 요약문(Reference)를 생성한다.
2. 모델을 통해서 생성된 요약문 X를 얻는다
3. X의 n-gram이 Reference에서 나타나는 비율을 구한다.



bi-gram을 이용하는 ROUGE-2의 식은 아래와 같이 쓸 수 있다.



## Reference



---

bert



Abstract

Pre-trained된 모델 위에 

output layer 추가->Task aware input transformation

다양한 태스크에서 엄청난 개선을 보여줌

\1. Introduction

LM을 pre-train하는 것은 여러 태스크에 있어 효과적.

pre-train에는 두 가지 전략 있음: feature-based 와 fine-tuning. 하지만 이 두 전략은 unidirectional한 LM이기 때문에 pre-train 아키텍처에 제한이 있음. 한방향이기 때문에 이전에 나타난 토큰들과만 self-attention 수행

우리가 제안하는 BERT는 양방향에서 보며 MLM objective으로 pre-train함.

이에 더해서 next sentence prediction 태스크도 pre-train 과정에서 수행함



## 2.1 Unsupervised Feature-based Approaches

- Sentence embedding 학습 방법은 다음 문장 순위 매기기, 다음 문장 생성, auto-encoder 등 사용 
- ELMo는 forward rnn과 backward rnn을 concat해서 썼다. 
- Cloze task를 통해 text generation 모델의 선응과 안정성을 높일 수 있음. (MLM과 같은 태스크임)





## 2.2 Unsupervised Fine-tuning Approaches

최근 연구는 contextual token representation을 만드는 pre-trained 모델들은 fine-tuning을 통해 적은 파라미터만 처음부터 학습하게 됨.

Unlabeled text라는 거는 인간이 붙인 라벨이 없다는 의미이고, 프로그램이 생성한 라벨은 있음. -학습시 라벨 생성.

Left-to-right LM과 (단방향) auto-encoder objective이 이런 모델들을 pre-train하는 데 사용됨. auto-encoder는 GPT 얘기 아님..다른 모델들



## 2.3 Transfer Learning from Supervised Data

NLI나 MT에서 지도학습으로 전이학습을 하면 효과가 좋다는 연구가 있었다.



## 3. BERT

BERT의 프레임워크는 두 단계로 이루어짐: Pre-training, fine-tuning

Pre-training: 레이블 없는 데이터들로 다양한 pre-training 태스크 학습.

Fine-tuning: 모델이 pre-trained된 파라미터로 초기화됨

model architecture : 



Input/Output Representation

BERT가 다양한 task를 잘 처리하도록 문장 하나 or 문장 쌍을 하나의 token sequence로 처리함

Sentence는 실제 문장이 아닌 연속된 텍스트

WordPiece 사용. vocab size: 30000 -> (huggingface tokenizer- model- wordpiece 참고)

sequence의 처음은 special classification token [CLS] 토큰! 이 CLS 위치의 마지막 hidden state는 sequence 정보를 모아 classification task에 사용됨

([CLS] a ([SEP]) (b) [SEP]) : segment embedding(cls, a와 sep까지 0, b와 sep까지 1)

(GPT는 [BOS] a ([SEP]) (b) [EOS] 였음) -> cls_hidden에서 마지막 토큰 값을 사용했었음



## 3.1 Pretraining BERT

TASK 1 Maksed LM 

양방향 학습을 위해 랜덤하게 일부 토큰을 [MASK] 토큰으로 가리고 이를 맞추게 하였다. WordPiece 토큰의 15%를 랜덤하게 골라서 마스킹함. denoising auto-encoders와 다른 점은 denoising auto-encoder는 전체 input을 다시 만들지만(전체 시퀀스 예측), BERT는 마스킹된 토큰만 예측한다는 것이 다름. 

이런 MLM 기법의 단점은 Pre-train과정과 fine-tuning 과정에서 모델이 보게되는 데이터의 형식이 다른 것인데, 이를 완화하기 위해서 선택된 15%의 토큰 중 80% 는 [MASK]로 바꾸고 10%는 랜덤한 다른 토큰으로 바꾸고 10%의 경우 그대로 두었다. 이렇게 선정된 15%의 토큰의 원래 token을 예측하였다. Appendix를 보면 이런 비율로 하는 것이 가장 성능이 좋았다고 한다. 



TASK2 Next Sentence Prediction(NSP)

다음 문장 예측 태스크는다음 문장을 두 가지로 예측하는 것인데, 50%의 경우 이어진 문장은 IsNexT로 진짜 다음 문장이고 50%는 NotNext로 다음 문장이 아닌 랜덤한 문장이다. 이런 훈련 목표는 QA와 NLI 태스크에 있어서 좋은 영향을 주었다. 앞선 연구와 다르게 BERT는 fine tuning base이다. (feature base가 아니라) 하지만 벡터 공간 C의 경우 fine-tuning없이는 의미없는 sentence representation이다. 

- 다른 논문에서는 NSP가 별 효과가 없다는 얘기도 있다



## 3.2 Fine-tuning BERT

Input : sentence pair task, single sentence task

Output : token level task, classificatin task



# 4 Experiment

## 4.1 GLUE

SuperGlue도 있음

마지막 hidden의 첫 번째 벡터 C를 이용

softmax를 취해서 

러닝레이트도 좀 작은 값 씀

BERT base와 BERT large의 2.5% 차이도 큰 것



## 4.2 SQuAD

softmax 확률 예측

S : start를 체크하는 가중치(S_w : h차원)

E : end를 체크하는 가중치

Start의 확률 분포
$$
S \cdot T_i
$$

$$
i \leq j
$$

$$
softmax 확률예측: p_i = \frac{e^{S\cdot T_i}}{\sum_j e^{S \cdot T_j}}
$$



차원이 같기 때문에 transpose해서 dot product하면 스칼라값이 나옴. 

![image-20211013221249134](C:\Users\Boyoon Jang\AppData\Roaming\Typora\typora-user-images\image-20211013221249134.png)

End의 확률분포



max score: 
$$
S \cdot T_i + E \cdot T_j
$$


소프트맥스 아니고 logit 씀



## SQuad 2.0

답이 없을 확률 : 2.0에서 새롭게 추가된 거

정답 span : squad 1.0과 같음

정답이 있을 경우 : threshold 타우 값은 dev set에서 f1이 가장 좋은 값으로 선택 
