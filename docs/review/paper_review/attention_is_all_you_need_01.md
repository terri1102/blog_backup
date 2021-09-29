---
layout: default
title: "Attention is all you need"
date: 2021-05-16
parent: Paper Reviews
grand_parent: Reviews
nav_order: 1
comments: true
---





논문 키워드





논문에 사용된 모델





논문 주제 

https://arxiv.org/abs/1706.03762



## Background

기존 RNN : 거리가 먼 단어의 관계를 계산하는 데 어려움이 있음

트랜스포머는 operation 수를 줄이면서 Multi-head attention으로 해결



Self-attention: 하나의 시퀀스에 어텐션을 취해서 표현을 얻는 방법

Transformer is the first transduction model relying entirely on self-attention to compute representations of its input and output without using sequencealigned RNNs or convolution.



## Model Architecture

인코더는 representation (x1, x2...,xn)을 z(z1,...,zn)으로 매핑한다. 디코더는 output 시퀀ㅅ (y1,...,yn)을 만들어낸다. 각 스탭바다 모델은 auto-regressive하게 이전에 만들어진 심볼을 더해서 다음 output을 생성한다. 
$$
y_t = f(y_{1:t-1})
$$




### 인코더 

6개의 동일한 레이어를 쌓아서 구성.(그림에서는 N x로 표현됨) 각 레이어별 2개의 서브 레이어 구성. 서브 레이어는 multi-head self-attention와 position-wise fc ff nn임. 

FF: h1과 h2 등 옆의 관계를 못 봄

multi-head: h1, h2 옆의  위치관계를 봄..?

* Residual and layernorm
  * LayerNorm(x + sublayer(x))
  * residual connection: 그래디언트 보존하여 레이어 깊게 쌓을 수 있게 해줌
  * 임베딩 아웃풋: 512, sublayeroutput: 512 -> 인코더는 다 512



### 디코더

6개의 동일한 레이어 쌓아서 구성. 

서브레이어 : 

* Masked multi-head self-attention
* Multi-head encoder-decoder attention : 인코더에서 K, V 들어옴
* Position-wise fully connected feed-forward network

디코더 차원도 다 512차원



### 3.2.1 Scaled Dot-product Attention

query별 모든 key와의 내적을 구하고 이를 dk squared로 나눔. 그리고 소프트 맥스를 취해서 weight를 구함. 

일반적으로 사용하는 Dot-product attention과 동일하지만 dk로 나눈 것만 다름. dot -product attention이 additive attention보다 빠르고 space-efficient(메모리 적게 사용)하기 때문에 사용함. 또한 scaling하면 성능 비슷해짐.

성능이 좋아지는 이유: scaling한 이유는 소프트맥스하기 전에 attention score의 그래디언트가 아주 작아지는 것을 방지하기 위해서.???



### 3.2.2 Multi-Head Attention

선형적으로 queries, keys, values를 정사영(project)한 후 각각 dk, dk, dv 차원으로 만듦. 이렇게 만든 projected version을 이용해서 attention function을 병렬적으로 시행할 수 있음. 이렇게 병렬적으로 attention을 시행한 결과를 concat해서 다시 project해서 final values를 구함

모델 차원인 (d_model)512개를 64(d_k,d_v)로 쪼개서 attention 여러번 시행. 



### 3.2.3 Application of Attention in our Model

Multi-head attention : 특징 벡터를 뽑는 것이 목적. representation 뽑기 . 

마스킹은 -inf 값을 넣어서 함.

| ㅇ   | Bos  | I    | am   | a    | boy  | Eos  |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Bos  | 10   | -inf | -inf | -inf | -inf | -inf |
| I    |      |      |      |      |      |      |
| am   |      |      |      |      |      |      |
| a    |      |      |      |      |      |      |
| boy  |      |      |      |      |      |      |
|      |      |      |      |      |      |      |

이를 weight sum 하면 -inf는 0이 됨-> softmax할 때 0이어서 빠짐



### 3.3 Position-wise Feed-Forward Network

두 개의 선형 변환을 하는데, 렐루 활성화가 중간에 끼어있음. 즉 dense 레이어 두 개인데 첫 레이어에 activation이 렐루임.

```python
class PositionWiseFFN(nn.Module):
    """Positionwise feed-forward network."""
    def __init__(self, ffn_num_input, ffn_num_hiddens, ffn_num_outputs,
                 **kwargs):
        super(PositionWiseFFN, self).__init__(**kwargs)
        self.dense1 = nn.Linear(ffn_num_input, ffn_num_hiddens)
        self.relu = nn.ReLU()
        self.dense2 = nn.Linear(ffn_num_hiddens, ffn_num_outputs)

    def forward(self, X):
        return self.dense2(self.relu(self.dense1(X)))
```



RELU








$$
Max(0,xW1 + b)
$$


아 이게 렐루 의미구나. 아래는 렐루를 사이에 낀 선형 변환 2개
$$
FFN(x) = max(0, xW1 + b1)W2 + b2
$$


포지션별 선형 변환은 같지만 레이어별로 다른 파라미터 사용함. 이는 convolution을 kernel size 1인 걸로 수행하는 것과 같음. 그래서 input과 output 차원은 512이며, inner-layer는 2048임


$$
ff1 = xW1+b1
$$

$$
ff2 = max(0, xW1 + b1)W2 + b2
$$

라고 할 때 ff1의 차원 수가 2048임 



### 3.4 Embeddings and Softmax

learned embedding을 사용하여 input token과 output token을 벡터화함. usual leaned linear transformation과 softmax 함수를 이용해서 디코더 output을 다음 토큰 예측에 사용함. 

웨이트를 공유함(단어 임베딩벡터(e)과 마지막 단어 출력시(e^T)(softmax 전의 레이어)): 단어 벡터와 임베딩 벡터가 비슷해야함.

weight sharing
$$
e \approx> e^T
$$
공유되었다는 의미





### 3.5 Positional Encoding

시퀀스 내의 토큰의 위치를 알기 위해 positional encoding함

Position Embedding을 학습하는 것과 sinusoid(encoding, 고정값)를 사용하는 것과 결과가 거의 동일함. 시퀀스가 길었을 때도 sinusoid 쓸 수 있다. 반면 임베딩은 학습 데이터 길이에 맞춰지기 때문에 학습 안 된 부분은 랜덤값으로 됨. 따라서 학습 시퀀스보다 긴 시퀀스들어오면 성능이 안 좋았음 (하지만 요즘은 다 학습하는 embedding 쓴다고 함)



## 4 Why Self-Attention

Computational complexity

이때 당시 n < d(512) 였음. 지금은 n이 점점 커지고 있음(2048 등)

self attention은 n^2 * d였음. Recurrent는 n* d^2. 

cnn : log_k(n) 

restricted attention: 관심 단어 몇 개만 보는 거(r 개만). 더 긴 시퀀스볼수있음???



## 5 Training

Wordpiece는 정확한 스택을 구글이 공개는 안 했음. ##입니다

BPE : _나는, _입니다



## 

h: 8 헤드 개수

dv: 64

dropout : 0.1



Adam Optimizer: 

loss를 가지고 backprop을 하면 그래디언트가 나오는데

x_t  -> delta t

x t+1 -> delta t+1 : 모멘텀을 계산하기 위해 beta1, beta2, epsilon 설정



learning rate : 레이어가 깊어진 경우 필요. 초반에는 선형적으로 러닝레이트 증가시킴. (예를 들어 10000 스텝 학습시 4000스텝까지 0.001까지 러닝레이트 증가, 후 점점 떨어짐)-> 미니멈 식



Regularization

Residual dropout : output of sub-layer , sums of `embeddings` and `positional encoding`, p_dropout= 0.1  연결된 노드의 10%의 가중치를 0으로 만들어서 학습에서 제외시킴

Label smoothing: eps=0.1 :정답 레이블(vocab 32007 - 패딩 2개  = 32005인데 0.1에서 32005를 나눈 값으로 나눔. 그래서 레이블값이 원핫이었다가 이제는 정답은 0.99 나머지는 0.0001.. 이런식으로 바뀜)



근데 checkpoint averaging은 어떻게 한 걸까?



## 6.3 Constituency parsing

constituency parsing : 문장에서 관계를 파악하는 태스크. 즉 번역 뿐만 아니라 다른 태스크에서도 잘 작동하는지 확인하려고 실험.

태스크에 맞추지 않아도 잘 작동했다.





https://d2l.ai/chapter_attention-mechanisms/transformer.html

---

linear projection 찾아보기 : 일단 벡터를 쪼개주는(multi head attention) 연산인듯

wordpiece와 BPE 차이 https://lovit.github.io/nlp/2018/04/02/wpm/

label smoothing 찾아보기
