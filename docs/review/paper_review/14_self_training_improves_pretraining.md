---
layout: default
title: "[NLP] Self training improves pre-training for natural language understanding"
date: 2021-09-12
parent: Paper Reviews
grand_parent: Reviews
nav_order: 14
use_math: true
comments: true
---



# **Self-training improves pre-training for natural language understanding**

이 논문은 다운스트림 태스크의 도메인에 맞는 데이터 증강을 하는 방법을 제안하고, pre-training과 더불어서 사용할 수 있을 것 같아서 읽게 된 논문이다.

{:toc}



# **논문 키워드**

`unsupervised pre-training`, `self-training`, `semi-supervised learning`, `SentAugment`



# **논문 주제** 

<span style="background:#FFD9EC">1.Self-training의 개념과 효용성:  Self-training은 pre-training과 보완적인가? </span>

<span style="background:#FFD9EC">2. SentAugment: 특정 도메인의 라벨링 안 된(unannotated) 데이터를 증강하는 방법</span>

논문의 내용이 복잡한 것은 아니지만 개인적으로 논문 구성이 깔끔한 느낌은 아니어서 위의 두 가지 주제를 기억하면서 아래 정리된 내용을 읽으면 좋을 것 같다.



# **내용 요약**



# **Self-training**

**Self-training :**  레이블된 데이터로 학습한 선생 모델을 이용해서 인공 라벨을 생성하는 준지도학습(semi-supervised method)의 한 종류이다. 이렇게 생성한 라벨링된 데이터를 이용해서 학생 모델을 훈련한다. 최근 자연어 이해 분야에서 자주 사용되는 기법은 unsupervised pretraining이지만, 이 논문에서는 self-training도 라벨 없는 데이터를 배우는데 효과적인 방법이며 pretraining을 보완해서 사용할 수 있음을 보여준다. 



## **Semi-supervised learning(준지도학습)**

레이블링된 데이터와 레이블링이 안 된 데이터를 모두 훈련에 사용하는 방법

**Self-training :** 학생 모델 >= 선생 모델

**Knowledge distillation :** 학생 모델 < 선생 모델



### Pre-training  

준지도학습의 대체품으로 Pre-training 방법이 있다. Pre-training은 처음에 auxiliary task로 학습한 다음에 downstream 태스크로 파인튜닝하는 방법이다. 

이미지 처리 분야에서는 Self-training 방법이 Pre-training 방법에 보완적으로 사용될 수 있다는 연구가 있었지만, 이는 라벨링된 데이터로 pre-train한 것으로, 자연어 처리 분야의 pre-training이 unlabeled data를 대상으로 이루어지는 것과 차이가 있다. 



# SentAugment

인터넷에 엄청 많은 정보들이 널려있는데 이를 통으로 모은다음에 필요한 in-domain 데이터들만 모을 수는 없을까? 

많은 연구를 통해 다운스트림 태스크와 훈련 데이터의 도메인을 맞추는 것이 중요하다는 것은 알려져 있다. SentAugment는 task-specific 쿼리 임베딩을 이용해서 open-domain의 unlabeled data를 in-domain labeled data로 만들 수 있게 해준다. 즉, Self-training을 하려면 in-domain의 unlabeled data가 필요하기 때문에 in-domain 데이터를 만들어주기 위해 사용한다.



### SentAugment의 과정

1) Large-scale sentence bank  

웹에서 크롤링한 CommonCrawl 데이터로 다양한 도메인과 다양한 스타일의 문장으로 이루어져 있다.  각 문장을 universal paraphrastic sentence encoder를 통해서 임베딩했다. 이 인코더는 다운스트림 태스크에 영향을 받지 않으며, word2vec, uSIF, **SASE**를 사용해서 결과를 비교했다. 훈련시 x, y, yc의 triplet loss를 이용했고, x,y는 positive pair (연관이 있는 문장)이며 yc는 미니배치 안의  hard negative 샘플이다. 아래의 Loss function을 보면 x,y와 코사인 유사도는 높이고 x와 yc의 코사인 유사도는 낮추는 방향으로 학습이 일어난다.



$$
L(x, y) = max(0, \alpha - cos(x, y) + cos(x, y_{c}))
$$



2) Downstream task embeddings

1번에서 사용한 인코더 모델을 사용해 각 다운스트림 태스크마다 임베딩을 구한 후, 이 태스크 임베딩을 쿼리로 이용해서 sentence bank로부터 코사인유사도를 비교해서 비슷한 문장들을 뽑았다. 다운스트림 태스크의 임베딩을 구할 때 all-average, label-average, per-sentence 방법을 모두 사용해봤는데 label-average가 대체로 좋은 성능을 보였다.



3) Unsupervised data retrieval

위의 2번 과정을 통해 거대한 large sentence bank에서 in-domain 문장을 뽑아서 semi-supervised learning에 필요한 후보군을 몇 백만 개로 추리는 과정이다. 2번 과정을 뽑은 몇 십억 개의 문장을 다 사용할 수는 없기에... scalability를 위해 만든 unannotated data를 줄이기 위해 선생 모델의 컨피던스값을 이용해서 클래스의 비율을 유지하면서 필터링했다.  작은 태스크를 위한 데이터 구성시 threshold를 이용해 원래의 훈련 데이터의 100배 정도의 크기를 갖게 했다.

---



## Self-training 과정

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sentaugment.jpg?raw=true" alt="sentaugment" class="center" style="zoom:110%;" />

1. 선생 모델인 RoBERTa-large 모델을 크로스 엔트로피 loss로 레이블링된 다운스트림 태스크에 맞춰서 파인튜닝함
2. <span style="background:#FFD9EC">Open-domain data 중에서 태스크에 맞는 데이터(in-domain data)를 뽑아서 unannotated 데이터셋을 만듦 </span>
3. 2에서 얻은 데이터를 선생 모델이 레이블링하고, 각 클래스별 top k의 샘플을 뽑아서 final dataset을 구성함
4. 학생 모델인 RoBERTa-large가 KL-divergence loss로 final dataset에 파인튜닝함.





### KL divergence를 사용하는 이유?

논문에서는 너무 자연스럽게 KL divergence를 썼다고 나와서 왜 그런지 생각해봤다... 

크로스 엔트로피 개념을 다시 살펴보자

$$
CE : H(P,Q)= -\sum_{x }P(x)\;log \; Q(x)
$$

> CE는 Q라는 모델의 결과에 대해 P라는 이상적인 값을 기대했을 때 우리가 얻게 되는 놀라움의 정보량을 표현 한 것

위의 식에서 P(x)는 진짜 확률분포이고 Q(x)는 우리가 추정한 확률 분포이다. 따라서 Crossentropy는 이 둘을 통해 데이터의 uncertainty를 측정한다.



KL divergence는 위의 CE식과 비슷하지만 최적이 아닌 인코딩(크로스 엔트로피)와 최적의 인코딩(엔트로피)의 차이를 구하는 식이다. 만약 뒤의 엔트로피가 상수라면 CE와 KL은 같다.


$$
D_{KL}(P||Q) =-\sum_{x}P(x)\;log \; Q(x) + \sum_{x}P(x)\;log \; P(x)
$$


KL divergence를 쓴 이유는 (내가 생각하기로는)  인공 데이터를 사용하기 때문에 엔트로피를 상수라고 생각할 수 없어서 KL divergence를 쓴 거 같다. 





### Knowledge-distillation

Self-training과 비슷하지만 학생 모델로 더 작은 RoBERTa 모델을 사용하고, 라벨로서 연속된 확률분포(soft label) 를 이용해서 학습한다.



### Few-shot learning

Few-shot learning은 라벨 데이터가 아주 적을 때 사용하는 방법으로, 논문에서는 각 클래스별로 아주 적은 데이터가 있을 때로 가정하였다. 데이터 증강과 self-training을 이용해서 데이터를 늘리고, 이를 선생 모델 학습(파인튜닝)에 사용한다.



# **Experiments**

**Large-scale bank of sentences**

* CommonCrawl에서 중복된 문장을 제거
* CC-100M, CC-1B, CC-5B 3가지의 corpora 사용
* 문장을 가져올 때 다운스트림 태스크에 있는 문장들과 중복된 문장 제거



**평가를 위해 사용한 다운스트림 태스크**

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sase1.jpg?raw=true" alt="sase1" class="center" style="zoom:110%;" />



**Sentence Embedding 모델**

* **SentAugment Sentence Encoder(SASE)**: 

  여러 데이터(NLI, MRPC, QQP, Round-trip translation, OpenSubtitles 데이터)를 이용해서 훈련함. 

  MLM 방식으로 pretrain 한 후, 문장 임베딩의 코사인 유사도를 구하기 위해 triplet loss를 이용해서 fine-tunning함. Commoncrawl의 긴 문장들로 훈련했기 때문에 다른 모델들(word2vec, uSIF )보다 좋은 성능 보임(STS 벤치마크로 평가시 )

* word2vec : 주변단어로 중심단어를 예측하거나(CBOW), 중심단어로 주변단어를 예측해서(Skip gram) 단어의 임베딩을 구하는 방법
* uSIF(Unsupervised smoothed inverse frequency) : 단어 생성의 확률은 단어와 문장 임베딩의 angular distance(각 거리)와 반비례하다는 아이디어로 단어 임베딩을 구하는 방법



**학생 모델 파인튜닝**

fairseq와 RoBERTa-Large model을 PLM으로 가져와서 사용했음. 그리고 각 다운스트림 태스크에 따라 파인튜닝함.

* Optimizer: Adam
* learning-rate schedule: 1e-5
* bach-size 16 with dropout rate 0.1
* Loss function : KL divergence



**Few shot learning 실험 설계**

Few shot learning은 선생 모델이 약하기 때문에 다른 조건을 추가했음

* Discrete label 사용
* 훈련 데이터셋에 Ground truth 데이터 포함
* 훈련 데이터셋으



# Analysis and Results



**Self-training의 결과**

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sase2.jpg?raw=true" alt="sase2" class="center" style="zoom:100%;" />

Self-train이 그저 in-domain 데이터를 늘리는 것과 얼마나 차이가 있을지를 알아보기 위해 ICP 모델과 비교했다.

ICP(In-domain Continued Pretraining): RoBERTa-Large를 증강한 데이터로 **pretrain**한 것

ST(Self-training): 선생 모델이 라벨링한 데이터로 학생 모델이 **fine-tunning**한 것. 

ICP 모델과 ST모델의 차이를 보면 단순히 in-domain 데이터가 많아져서 그런 것이 아닌 Self-training 알고리즘이 같이 사용될 때 더 높은 성능을 보여준다.



**Few-shot learning**

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sase3.jpg?raw=true" alt="sase3" class="center" style="zoom:110%;" />

40-200개의 샘플만을 이용해서 RoBERTa-Large 모델을 파인튜닝해서 선생 모델로 사용하였고,  Self-training한 경우가 더 좋은 성능(F1 score)과 낮은 variance를 보였다. 



**Knowledge distillation의 결과**

![sase4](https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sase4.jpg?raw=true)

GT : ground-truth 데이터로 훈련한 distilled RoBERTa(in-domain 데이터)

RD : 랜덤하게 뽑힌 데이터로 훈련한 distilled RoBERTa(다른 도메인도 섞여있음)

SA : 평균적으로 ground-truth보다는 못하지만 RD보다는 개선되어서, 어느정도 도메인을 비슷하게 맞춘 것을 알 수 있다.

데이터 증강을 더 한 경우 RD와 SA 모두 더 높은 성능을 보였다.





---

## Ablation study

| Componenets              |Approaches| Best Result            | Remarks |
| ---|-------------------- | ---------------------- |---|
| Task-specific retrieval |all-average, label-average, per-sentence| label-average approach | NER 태스크에서는 sentence별 query embedding이 효과적 |
| Sentence embedding space | average of fastText, uSIF-ParaNMT, SASE |SASE| word2vec 보다는 uSIF_PRAaNMT가 더 좋은 성능 보임 |
| Scaling bank size | 50m lines, 250m lines, 1B lines, 5B lines |1B| 1B에서 5B로 증가할 때 saturate됨 |
| Continuous labels | Continuous labels(class probabilities), discrete labels |continuous labels|  |
| Computational cost of self-training | Filter based on classifier confidence, few task-specific query embedding(label-average) |label-average| label-average방법으로 1B 문장 라벨링하는데 1분 걸릴 때 classifier confidence는 86시간 걸림 |

Task specific retreival

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sase5.jpg?raw=true" alt="sase5" class="center" style="zoom:110%;" />



Sentence Embedding space

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sase6.jpg?raw=true" alt="sase6" class="center" style="zoom:110%;" />



Scaling bank size

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sase7.jpg?raw=true" alt="sase7" class="center" style="zoom:110%;" />



Continuous labels

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sase8.jpg?raw=true" alt="sase8" class="center" style="zoom:110%;" />



Computational costs

* SentAugment의 prefiltering을 통해 선생 모델이 라벨링할 데이터의 숫자를 줄를 줄이는 방법을 통해서 연산 시간을 많이 줄일 수 있었다. 



## Analysis of Similarity search

STS 벤치마크를 통해 우리의 SASE 모델의 성능을 보여주겠다.



### SASE(SentAugment Sentence Embedding)

SASE: SentAugment에서 문장 임베딩을 위해 사용한 인코더.

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sase10.jpg?raw=true" alt="sase10" class="center" style="zoom:100%;" />

2012-2016년의 STS 태스크의 평균 f1 score 및 STS-Benchmark 태스크의 f1 score 모두 SASE가 가장 우수하다. 

nearest neighbor를 가져오는 것은 좋은 퀄리티의 paraphrase를 가져옴.

SentAugment는 large-scale similarity search를 임베딩 스페이스를 이용해  몇 십억 개의 문장 중 in-domain 문장을 찾는 데에 이용한다.

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/nlp/sase9.jpg?raw=true" alt="sase9" class="center" style="zoom:100%;" />

Sentence level queries: 문장 레벨의 쿼리를 통해 가져온 문장은 문장의 의미를 유지하거나 의미를 강화하는 좋은 paraphrase 문장 가져옴.

Label-level queries: 라벨 임베딩 쿼리를 통해 가져온 문장들은 다운스트림 태스크의 도메인과 잘 맞는 문장들이었다



## **Reference**

논문 : https://arxiv.org/abs/2010.02194

KL divergence: https://stats.stackexchange.com/questions/265966/why-do-we-use-kullback-leibler-divergence-rather-than-cross-entropy-in-the-t-sne

KL divergence: https://angeloyeo.github.io/2020/10/27/KL_divergence.html

uSIF: https://aclanthology.org/W18-3012/





