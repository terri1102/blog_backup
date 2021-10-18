---
layout: default
title: "[NLP] PEGASUS"
date: 2021-08-11
parent: Paper Reviews
grand_parent: Reviews
nav_order: 13
comments: true
use_math: true
---





#  🗝️ 논문 키워드

`summerization`, `GSG`, 



# 📑 논문 주제





# 📚 데이터셋

* Pre-training dataset
* Fine-tuning dataset 
* 

 <span style="background:#e8f7ff">**Cross-encoder**</span>

# 내용 요약

## Abstract

## 3. Pre-training Objectives

GSG 와 더불어 BERT의 pre-training objective인 MLM을 GSG와 합쳐서 vs. PLM만 사용한 것을 살펴볼 것이다.



### 3.1. Gap Sentences Generation(GSG)

다운 스트림 태스크와 비슷한 pre-training objective를 사용하면 더 좋은 효과를 낼 것이라고 생각해서 seq2seq self-supervised objtective를 이용해서 abstractive summary없이 pre-train 하였다.

GSG: 문서의 문장을 골라서 통째로 [mask1] 토큰으로 바꿈. 그리고 gap-sentence들을 concat해서  pseudo-summary로 만듦. GSR(Gap sentences ratio)는 mask token ratio랑 비슷하게 마스킹된 문장 비율임.

요약과 비슷하게 만들려고 중요한 문장을 골랐음. 이를 통해서 마스킹의 이점과 다운스트림 태스크와 비슷한 구조로 만들려고 했음.

m gap sentence 고르는 전략 3가지 : 

\- Random : uniform하게 m개의 문장을 랜덤하게 고름 

\- Lead : 문서의 앞 문장 m개를 고름

\- Principal : 중요한 순서대로 top-m개의 문장을 고름. 중요도를 평가하기 위해 각 문장과 문서 간의 ROUGE1-F1을 구했음.

Principal 내에서의 4가지 전략 : Ind/Seq, Orig/Uniq



### 3.2 Masked Language Model(MLM)

BERT에서처럼 15%토큰을 골라서 그 중 80%는 마스킹 [MASK2] 하고 10 %는 랜덤 토큰으로 바꾸고, 10%는 아무 처리도 안 하시는 식으로 MLM을 구현했다. 하지만 MLM은 다운스트림 태스크의 성능을 개선하지 못했기 때문에 최종 모델인 pegasus_large에서는 제외하였다.





## 4. Pre-training Corpus

\- C4 : Colossal and Cleaned version of Common Crawl

\- HugeNews : 1.5B articles from news and news-like websites collected from 2013-2019.



### 6.2. Larger Model Results

### PEGASUS_BASE와 PEGASUS_LARGE의 Configuration

|                                             | PEGASUS_BASE           | PEGASUS_LARGE             |
| ------------------------------------------- | ---------------------- | ------------------------- |
| Hidden size of attention(H)                 | 768                    | 1024                      |
| Feed-forward layer size (F)                 | 3072                   | 4096                      |
| Number of self-attention (A)                | 12                     | 16                        |
| Number of layers for encoder and decoder(L) | 12                     | 16                        |
| Batch size                                  | 256                    | 8192                      |
| Pre-training steps                          | 500k                   | 500k                      |
| Pre-training objective                      | GSG with MLM, GSG only | GSG(Ind-Orig)             |
| Vocab                                       | BPE, Unigram           | Unigram vocab size of 96k |
| Number of parameters                        |                        | 568M                      |



모델이 더 잘 따라할 수 있게 하기 위해 선택된 문장들 중 20%는  [MASK1]으로 바꾸지 않고 그대로 두었다. 또한 최적으로 찾은 GSR인 30%을 위의 변화에 맞추기 위해 45%까지 올렸다.

간단한 하이퍼 파라미터 sweep을 통해서 학습률과 length penalty를 결정했다. 



몇 개의 데이터셋은 input token의 길이가 최대 인풋 길이인 512 토큰보다 길었는데, 이때 positional encoding을 (트랜스포머처럼) sinusoid positional encoding을 사용하면 fine-tuning할 때 더 긴 길이를 사용해도 잘 generalize하는 것을 확인했다.



PEGASUS_Large를 사용한 결과 작은 데이터셋으로 fine-tuning을 진행하더라도 좋은 성과를 보인 것을 보니 pre-training 과정에서 이점을 얻는다고 볼 수 있었다. 



### 6.3. Zero and Low-Resource Summerization

현실 세계에서는 요약 데이터셋이 작은 경우가 많다. 그래서 고맙게도 본 논문에서는 작은 데이터셋에서 fine-tunning하는 경우에 대한 실험도 진행하였다. 논문에서는 배치 사이즈 256, 학습률 0.0005로 2000 steps 훈련하였고 가장 좋은 validation checkpoint를 골랐다.

실험 결과를 보면 Pegasus_large는 100개의 샘플로 fine-tunning했을 때 Transformer_base를 20k~200k의 데이터셋으로 fine-tunning한 것과 비슷한 성능이 나왔다. 



### 6.4. Qualitative Observations and Human Evaluation

이전 연구 중 maximum likelihood 방식으로 훈련한 모델들이 반복적인 텍스트를 생성해내는 문제들을 가지고 있었던 것과 다르게 Pegasus는 이런 문제가 없어서 따로 이에 대한 해결책을 강구하지는 않았다고 한다. 

ROUGE 평가지표가 완벽한 건 아니지만, Perplexity-optimized models using aggregated ROUGE는 좋은 모델을 만드는 데 도움이 되었다. 

사람이 검수한 결과 Pegasus_large 모델들은 XSum, CNN/Daily Mail에서 reference summary와 비슷한 퀄리티를 보였지만 Reddit TIFU의 경우 다양한 문체로 이루어진 데이터셋이어서 그런지 그렇지 못 했다.



## 7. Conclusion

이 논문에서 seq2seq 모델인 PEGASUS를 소개한다. PEGASUS 모델은 abstractive summary(생성 요약) 태스크에 맞춘 gap-sentence generation이라는 pre-training objective을 가지고 훈련되었다. 





# Takeaway

작은 데이터셋으로 fine-tunning하는 것에 대한 내용이 있어서 도움이 많이 되었다. 하이퍼 파라미터 조정할 때 참고할 만할 것 같다.





# Reference

