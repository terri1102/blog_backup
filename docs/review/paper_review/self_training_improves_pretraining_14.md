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



# Self-training improves pre-training for natural language understanding

이 논문은 데이터 증강을 하는 괜찮은 방법 같아보이고, pre-training과 더불어서 사용할 수 있을 것 같아서 읽게 된 논문이다.



# **논문 키워드**

`unsupervised pre-training`, `self-training`, `semi-supervised learning`, `SentAugment`



# **논문 주제** 

> Do pre-training and self-training capture the same information, or are they complementary?

> How can we obtain large amounts of unannotated data from specific domains?





# **내용 요약**

---

논문의 두 가지 메인 아이디어 1) Self-training의 개념 + 효용 설명 2) SentAugment: 데이터 증강 방법



최근 자연어 이해 분야에서의 트랜드는 unsupervised pretraining이다. 이 논문에서는 self-training도 라벨 없는 데이터를 배우는데 효과적인 방법이라는 것을 설명한다. 

많은 연구를 통해 다운스트림 태스크와 훈련 데이터의 도메인을 맞추는 것이 중요하다는 것은 알려져 있다. SentAugment는 task-specific 쿼리 임베딩을 이용해서 open-domain의 unlabeled data를 in-domain labeled data로 만들 수 있게 해준다. 또한 Self-training을 통해서 pre-training을 보완할 수도 있다.. 



# **Introduction**

**Self-training :**  레이블된 데이터로 학습한 티처 모델을 이용해서 인공 라벨을 생성하는 준지도학습(semi-supervised method)의 한 종류이다. 이렇게 생성한 라벨링된 데이터를 이용해서 학생 모델을 훈련한다.



### **Semi-supervised learning(준지도학습)**

레이블링된 데이터와 레이블링이 안 된 데이터를 모두 훈련에 사용하는 방법

**Self-training :** 학생 모델 >= 선생 모델

**Knowledge distillation :** 학생 모델 < 선생 모델



### Pre-training  

준지도학습의 대체품으로 Pre-training 방법이 있다. Pre-training은 처음에 auxiliary task로 학습한 다음에 downstream 태스크로 파인튜닝하는 방법이다. 

이미지 처리 분야에서는 Self-training 방법이 Pre-training 방법에 보완적으로 사용될 수 있다는 연구가 있었지만, 이는 라벨링된 데이터로 pre-train한 것으로, 자연어 처리 분야의 pre-training이 unlabeled data를 대상으로 이루어지는 것과 차이가 있다. 

그렇다면 unlabeled data를 이용한 Pre-training과 Self-training 방법을 상호보완적으로 사용할 수 있을까? 

또한, 특정 도메인의 unannotated data를 어떻게 증강할 수 있을까?





## SentAugment

(위의 2번에 해당하는 과정)

인터넷에 엄청 많은 정보들이 널려있는데 이를 통으로 모은다음에 필요한 in-domain 데이터들만 모을 수는 없을까?

SentAugment 방법: 인터넷에서 스크래핑한 데이터를 통해 "in-domain" 데이터를 만들어냄. 인터넷에서 긁어온 데이터들은 다양한 정보를 포함하고 있는데, 이를 다운스트림 태스크와 같은 도메인의 데이터로 만드는 방법이다. Self-training을 하려면 in-domain의 unlabeled data가 필요하기 때문에 in-domain 데이터를 만들어주기 위해 사용한다.



### SentAugment의 과정

1. Large-scale sentence bank  

   웹에서 크롤링한 CommonCrawl 데이터로 다양한 도메인과 다양한 스타일의 문장으로 이루어져 있다.  각 문장을 universal paraphrastic sentence encoder를 통해서 임베딩했다. 이 인코더는 다운스트림 태스크에 영향을 받지 않으며, word2vec, uSIF, SASE를 사용해서 결과를 비교했다. 훈련시 triplet loss를 이용했다. x,y는 연관이 있는 문장(paraphrases or parallel)이며, yc는 미니배치 안의 네거티브 샘플이다.

    


$$
L(x, y) = max(0, \alpha - cos(x, y) + cos(x, y_{c}))
$$



(x,y)는 positive pair 비슷한 문장. yc는 배치 안의 hard negative



2. Downstream task embeddings

각 다운스트림 태스크마다 임베딩을 구한 후 이 태스크 임베딩을 쿼리로 이용해서 sentence bank로부터 코사인유사도를 이용해서 비슷한 문장들을 뽑았다.

다운스트림 태스크의 임베딩을 구할 때 all-average, label-average, per-sentence 방법을 모두 사용해봤는데 label-average가 대체로 좋은 성능을 보였다.

3. Unsupervised data retrieval

거대한 large sentence bank에서 in-domain 문장을 뽑아서 semi-supervised learning에 필요한 후보군을 몇 백만 개로 추렸다. Scalability를 위해 이렇게 만든 unannotated data를 줄여야 함. 이를 위해 선생 모델의 컨피던스를 이용해서 클래스의 비율을 유지하면서 필터링했다. 

---



## Self-training 과정

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/sentaugment.jpg?raw=true" alt="sentaugment" class="center" style="zoom:110%;" />

1. 선생 모델인 RoBERTa-large 모델을 크로스 엔트로피 loss로 레이블링된 다운스트림 태스크에 맞춰서 파인튜닝함
2. Open-domain data 중에서 태스크에 맞는 데이터(in-domain data)를 뽑아서 unannotated 데이터셋을 만듦 (SASE)
3. 2에서 얻은 데이터를 선생 모델이 레이블링하고, 각 클래스별 top k의 샘플을 뽑아서 final dataset을 구성함
4. 학생 모델인 RoBERTa-large가 KL-divergence loss로 final dataset에 파인튜닝함.





### KL divergence를 사용하는 이유?

크로스 엔트로피 개념을 다시 살펴보자


$$
CE : H(P,Q)= -\sum_{x \in \Chi}P(x)\;log \; Q(x)
$$

> CE는 Q라는 모델의 결과에 대해 P라는 이상적인 값을 기대했을 때 우리가 얻게 되는 놀라움의 정보량을 표현 한 것

위의 식에서 P(x)는 진짜 확률분포이고 Q(x)는 우리가 추정한 확률 분포이다. 따라서 Crossentropy는 이 둘을 통해 uncertainty를 측정한다.



KL divergence는 위의 CE식과 비슷하지만 최적이 아닌 인코딩(크로스 엔트로피)와 최적의 인코딩(엔트로피)의 차이를 구하는 식이다. 만약 뒤의 엔트로피가 상수라면 CE와 KL은 같다.
$$
D_{KL}(P||Q) -\sum_{x \in \Chi}P(x)\;log \; Q(x) + \sum_{x \in \Chi}P(x)\;log \; P(x)
$$
KL divergence를 쓴 이유는 (내가 생각하기로는)  인공 데이터를 사용하기 때문에 엔트로피를 상수라고 생각할 수 없어서 KL divergence를 쓴 거 같다. 





### Knowledge-distillation

Self-training과 비슷하지만 학생 모델로 더 작은 RoBERTa 모델을 사용하고, 라벨로서 연속된 확률분포(soft label) 를 이용해서 학습함



### Few-shot learning

Few-shot learning은 라벨 데이터가 아주 적을 때 사용하는 방법. 각 클래스별로 아주 적은 데이터가 있을 때로 가정하였음. 데이터 증강과 self-training을 이용해서 데이터를 늘리고, 이를 선생 모델 학습에 사용함.



# **Experiments**

**Large-scale bank of sentences**

* CommonCrawl에서 중복된 문장을 제거
* CC-100M, CC-1B, CC-5B 3가지의 corpora 사용
* 문장을 가져올 때 다운스트림 태스크에 있는 문장들과 중복된 문장 제거



**평가를 위해 사용한 다운스트림 태스크**

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/sase1.jpg?raw=true" alt="sase1" class="center" style="zoom:110%;" />



**Sentence Embedding 모델**

SentAugment Sentence Encoder(SASE)를 여러 데이터를 이용해서 훈련함. MLM 방식으로 pretrain했음. 그후 문장 임베딩의 코사인 유사도를 구하기 위해 triplet loss를 이용해서 훈련함. STS 벤치마크로 평가했음. 

Commoncrawl의 긴 문장들로 훈련했기 때문에 다른 모델들(word2vec, uSIF )보다 좋은 성능 보임.

* word2vec : 주변단어로 중심단어를 예측하거나(CBOW), 중심단어로 주변단어를 예측해서(Skip gram) 단어의 임베딩을 구하는 방법
* uSIF(Unsupervised smoothed inverse frequency) : 단어 생성의 확률은 단어와 문장 임베딩의 angular distance(각 거리)와 반비례하다는 아이디어로 단어 임베딩을 구하는 방법



**학생 모델 파인튜닝**

fairseq와 RoBERTa-Large model을 PLM으로 가져와서 사용했음. 그리고 각 다운스트림 태스크에 따라 파인튜닝함.

* Optimizer: Adam
* learning-rate schedule: 1e-5
* bach-size 16 with dropout rate 0.1
* Loss function : KL divergence

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/sase2.jpg?raw=true" alt="sase2" class="center" style="zoom:100%;" />

Self-train이 그저 in-domain 데이터를 늘리는 것과 얼마나 차이가 있을까?이를 알아보기 위해 ICP 모델과 비교 : RoBERTa-Large를 증강한 데이터로 **pretrain**한 것

ST: 선생 모델이 라벨링한 데이터로 학생 모델이 **fine-tunning**한 것. 

ICP 모델과 ST모델의 차이를 보면 ST가 더 높은 성능을 보여준다.



**few shot learning**

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/nlp/sase3.jpg?raw=true" alt="sase3" class="center" style="zoom:110%;" />





knowledge distillation

![sase4]

RD : 랜덤한 문장을 뽑아서 증강시킴

SA : 같은 도메인의 문장을 이용해 증강



---

## Ablation study

| Componenets              |Approaches| Best Result            | Remarks |
| ---|-------------------- | ---------------------- |---|
| Task-specific retrieval |all-average, label-average, per-sentence| label-average approach | NER 태스크에서는 sentence별 query embedding이 효과적 |
| Sentence embedding space | average of fastText, uSIF-ParaNMT, SASE |SASE| word2vec 보다는 uSIF_PRAaNMT가 더 좋은 성능 보임 |
| Scaling bank size | 50m lines, 250m lines, 1B lines, 5B lines |1B| 1B에서 5B로 증가할 때 saturate됨 |
| Continuous labels | Continuous labels(class probabilities), discrete labels |continuous labels|  |
| Computational cost of self-training | Filter based on classifier confidence, few task-specific query embedding(label-average) |label-average| label-average방법으로 1B 문장 라벨링하는데 1분 걸릴 때 classifier confidence는 86시간 걸림 |





## Analysis of Similarity search

STS 벤치마크를 통해 우리의 SASE 방법의 성능을 보여주겠다



SASE



large-scale similarity search

sentence level : 쿼리문장의 의미를 유지하거나 의미를 강화하는 좋은 paraphrase 문장 가져옴.

label-level queries: 라벨 임베딩 쿼리를 통해 가져온 문장들은 다운스트림 태스크의 도메이놔 잘 맞는 문장들이었다




Semi-supervised techinique

	* pre-training
	* self-training
	* knowledge distillation





https://stats.stackexchange.com/questions/265966/why-do-we-use-kullback-leibler-divergence-rather-than-cross-entropy-in-the-t-sne

https://angeloyeo.github.io/2020/10/27/KL_divergence.html

https://aclanthology.org/W18-3012/
