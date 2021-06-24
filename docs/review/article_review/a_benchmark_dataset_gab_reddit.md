---
layout: default
title: "[NLP] A Benchmark Dataset for Learning to Intervene in Online Hate Speech 리뷰"
date: 2021-06-23
parent: Article Reviews
grand_parent: Reviews
nav_order: 5
comments: true
---



A Benchmark Dataset for Learning to Intervene in Online Hate Speech. Jing Qian, Anna Bethke, Yinyin Liu, Elizabeth Belding, William Yang Wang. University of California, Santa Barbara 2019. URL https://arxiv.org/pdf/1909.04251v1.pdf

# 논문 키워드

`hate speech detection`, `dataset`, `generative intervention`, `crowd sourcing`, `Reddit`,`Gab`

<br>

# 논문에 사용된 모델

* logistic regression: robust한 classic 모델로서 baseline으로 사용됨

* RNN

* Convolutional Bidirectional GRUwith Attention layers and Very Deep Convolutional Neural Networks (VDCNN) 

<br>

# 논문 주제 

Hate Speech를 막기 위해(intervene) 자동으로 응답을 생성하는 모델을 만드는 것. 

<br>

# 데이터셋

Reddit

Gab: 트위터랑 비슷한 플랫폼. 트위터에서 블락 당하면 여기로 간다고 하더라구요...

These datasets provide conversation segments, hate speech labels, as well as intervention responses written by Mechanical Turk Workers

<br>

# 내용 요약

## 1. Introduction



<br>

## 2. Related Work

* 

<br>

## 3. Approach





<br>

## 4. Experiments





### 

<br>

## 5. Results

성능: Bi-LSTM with attention + FastText Embeddings가 가장 높은 정확도(0.989)fmf qhduTek.

**5.1 Pretrained Embeddings**

**5.2 Attention**

**5.3 Inference Speeds**

**5.3.1 Cascading Model**





## 6. Conclusion

<br>

# References

Toxic Speech Detection. Animesh Koratana & Kevin Hu. Stanford University 2016. URL https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1194/reports/custom/15744362.pdf

paperswithcode Hate Sppech https://paperswithcode.com/dataset/hate-speech

[Hugging Face의 bert-base-uncased-hatexplain-rationale-two](https://huggingface.co/Hate-speech-CNERG/bert-base-uncased-hatexplain-rationale-two) 논문에서 사용한 모델은 아닌데 최근 모델 같아서 써볼 예정