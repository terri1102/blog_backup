---
layout: default
title: "[NLP] Toxic Speech Detection 리뷰"
date: 2021-06-16
parent: Article Reviews
grand_parent: Reviews
nav_order: 4
comments: true
---



Toxic Speech Detection. Animesh Koratana & Kevin Hu. Stanford University 2016. URL https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1194/reports/custom/15744362.pdf

# 논문 키워드

`hate speech detection`, `logistic regression`, `CNN`, `RNN`, `VDCNN`

<br>

# 논문에 사용된 모델

* logistic regression: robust한 classic 모델로서 baseline으로 사용됨

* RNN

* Convolutional Bidirectional GRUwith Attention layers and Very Deep Convolutional Neural Networks (VDCNN) 

<br>

# 논문 주제 

VDCNN이 혐오발언을 잘 검출하는지 검증해보고자 한다.

<br>

# 데이터셋

 Google Jigsaw에서 2017년에 Kaggle에 발표한[ “Toxic Comment ClassificationChallenge” ](https://www.kaggle.com/c/jigsaw-toxic-comment-classification-challenge)데이터셋

<br>

# 내용 요약

## 1. Introduction

다양한 온라인 플랫폼에서 혐오발언은 점점 더 문제가 되고 있다. 여러 기업에서는 혐오 발언 검출을 위해서 인간이 직접 검출하거나 자동화된 알고리즘으로 검출하는 방법을 사용하고 있다. 자동화된 알고리즘의 경우 정규표현식을 사용하거나, 블랙리스트에 있는 단어를 거르는 식으로 사용하는데 이 경우 다양한 형태의 혐오 발언을 거르지 못하는 경우가 많다. 반면 딥러닝을 이용한 모델들은 이런 문제는 개선했지만 많은 파라미터를 이용하기 때문에 연산 효율성이 떨어지는 경우가 많다.

본 논문에서는 baseline으로 logistic regression 모델을 사용하고, Attention-based RNN, CNN 분류기의 성능을 비교하며, 모델별 연산 효율성에 대해 알아볼 것이다.

<br>

## 2. Related Work

* Logistic Regression: 기준 모델로 클래식한 분류 모델인 logistic regression 모델사용
*  Conneau’s VDCNN for text classification: 이미지 분야에서 CNN 모델의 성공에 히입어 CNN 모델을 사용해 볼 것. 하지만 Conneau's VDCNN은 character-base이기 때문에 연산 효율성이 떨어지기 때문에 이를 word-based로 바꾸고 fasttext 임베딩을 사용할 것이다.
*  bi-directional GRU: 자연어처리에서 RNN 계열의 모델들의 성공에 힘입어 bi-directional GRU를 사용할 것이다.

<br>

## 3. Approach

### 3.1 Baselines 

본 논문에서 사용할 베이스라인 모델은 logistic regression with word and char n-gram features이고, 학습을 위해서 그래디언트 디센트 업데이트를 이용한다.
$$
θ_j := θ_j +
\frac{α}{m} \Sigma_{i=1}^m
(hθ(x^{(i)}) − y^{(i)})x_y^{(i)}
$$
여기서 사용할 특성으로 단어와 character n-gram을 모두 사용한다. 그리고 특성의 가중치를 알아보기 위해(weight the features) TF-IDF를 사용한다. TF-IDF는 importance of each of the words and n-grams in the corpus을 알게 해준다. 이를 위해 scikit-learn의 TfidfVectorizer를 사용했다.

### 3.2 GRU & LSTM with Attention

FastText의 라이브러리를 이용해 300차원의 벡터 표현을 구하고 이 word embedding을 이용해서 bi-directional GRU나 LSTM을 통과시켜 sentence embedding을 구한다. 이 임베딩은 scaled dot product attention layer에 넣어서 attention score를 구한다. 

scaled attention layer: 현재 decoder의 상태를 query q로 두고 각 인코더의 상태를 key로 둘 때, 각 key 별로 attention score는 아래와 같이 구한다.
$$
Attention Score = \frac{q^Tk_i}{\sqrt{d_k}}
$$
key의 차원의 루트값으로 나누는 이유(scaled dot product attention을 쓰는 이유)는, 차원이 커질수록 내적값이 많이 커지고 그 결과 softmax함수가 엄청 작은 그래디언트를 갖게 되기 때문(그래디언트 소실)

모델의 결과는 fully connected linear decoder를 거쳐 n차원의 output 공간에 매핑되고 softmax함수에 의해 확률값으로 변한다. 이 모델은 CE loss와 SGD optimizer를 이용해서 학습했으며 PyTorch의 기본 ResNet 학습 코드의 accuracy function을 차용했다.



### 3.3 VDCNN

각 코멘트는 character의 시퀀스로 간주하며 고정된 1024의 길이에 맞추기 위해 자르거나 패딩했다. 각 character마다 16 사이즈의 임베딩을 했다. 이 임베딩은 윈도우 사이즈 3, 64 output 채널을 갖는 1d conv layer를 거친다.(64 x 1024) 그 후 4개의 convolutional block를 거치게 되는데 각 블럭마다 구조는 Conv1d(D,2D,3) -> Batch Norm -> ReLU -> Conv1d(2D,2D,3) -> Batch Norm -> ReLU 이렇게 된다. The idea of each convolutional block is to double the output channel afterwhich a pooling layer will reducing the number of embeddings by half through downsampling. (conv1d의 세번째 파라미터는 윈도우사이즈) Conv 블럭들을 다 지나고 나면 결과 사이즈는 512 x 128이다. 이제 여기에 k-max pooling layer를 붙여서 차원을 512 x 8로 줄인다. 그리고 flatten 시켜 4096 크기의 벡터로 만든 후 세 개의 linear layer를 거친다. 차원의 변화는 Linear(4096, 2048) –> ReLU –> Linear(2048, 2048) –> ReLU –> Linear(2048, n) 이렇게 된다. (n은 이진분류니까 2차원) 마지막으로 이를 소프트맥스 함수에 넣어서 확률값을 구한다. CE loss와 Adam optimizer을 이용해 학습했다.



<br>

## 4. Experiments

### 4.1 Data

사용한 데이터는 Google Jigsaw에서 2017년에 Kaggle에 발표한[ “Toxic Comment ClassificationChallenge” ](https://www.kaggle.com/c/jigsaw-toxic-comment-classification-challenge)데이터셋이다.  이 데이터셋은  223,549개의 user comments를 포함하고 7가지의 중복 가능한 레이블(clean, toxic, obscene, insult, identity hate, severe toxic, and threat)을 가진다.  이 데이터셋은 실제로 수집된 것이며 대부분의 샘플의 레이블은 clean이다. 본 논문에서는 레이블을 clean과 그 이외의 레이블로 나누어서 이진분류를 하였다.



### 4.2 Evaluation Method

모델의 성능은 f1 score와 accuracy로 평가할 것이다.



### 4.3 Experimental Details

**4.3.1 Architectures**

* Logistic regressor
* VDCNN-9(Conneau et al. [2016])
* Bidirectional GRU and LSTM(Chung et al.[2014])



**4.3.2 Hyperparameters**

VDCNN으로 하는 모든 실험: batch size: 128, SGD with a momentum of 0.9, weight decay: 1e-4, learning rate: 0.01 for 15 epochs w/ milestones for learning rate decay of a factor of 10 at epochs 3,6,9,12 and 15 의 하이퍼 파라미터로 진행되었다. 

GRU 와 LSTM 모델은 batch size: 64, SGD with a momentum of 0.9, seight decay: 1e-2,learning rate: 0.001로 시작해 plateaus일 때 10의 약수로 decay(decay by factor of 10 on plateaus)

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