---
layout: default
title: "[NLP] Self training improves pre-training for natural language understanding"
date: 2021-09-12
parent: Paper Reviews
grand_parent: Reviews
nav_order: 14
comments: true
---





# **논문 키워드**

`unsupervised pre-training`, `self-training`, `semi-supervised learning`, `SentAugment`



# **논문 주제** 

> Do pre-training and self-training capture the same information, or are they complementary?

> How can we obtain large amounts of unannotated data from specific domains?





# **내용 요약**

---

논문의 두 가지 메인 아이디어 1) Self-training의 개념 + 효용 설명 2) SentAugment: 데이터 증강 방법



최근 자연어 이해 분야에서의 트랜드는 unsupervised pretraining이다. 이 논문에서는 self-training도 라벨 없는 데이터를 배우는데 효과적인 방법이라는 것을 설명한다. 또한 Self-training을 통해서 pre-training을 보완할 수도 있음. 



많은 연구를 통해 다운스트림 태스크와 훈련 데이터의 도메인을 맞추는 것이 중요하다는 것은 알려져 있다. 





Semi-supervised learning

Self-training

Knowledge distillation





![sentaugment]

## SentAugment

인터넷에 엄청 많은 정보들이 널려있는데 이를 통으로 모은다음에 필요한 in-domain 데이터들만 모을 수는 없을까?



1. Large-scale sentence bank 
2. Downstream task embeddings


$$
L(x, y) = max(0,a - cos(x, y) + cos(x, yc))
$$


(x,y)는 positive pair 비슷한 문장. yc는 배치 안의 hard negative



## 처음엔 self-training과 sentaugment설명하고, 이제 전체 프로세스 설명(프로세스 순서상 sentaugment 먼저 설명)



### KL divergence (쿨백 라이블러 발산)

크로스 엔트로피 개념을 다시 살펴보자
$$
CE = \sum_{x \in \Chi}(-P(x)log(Q(x)))
$$

> CE는 Q라는 모델의 결과에 대해 P라는 이상적인 값을 기대했을 때 우리가 얻게 되는 놀라움의 정보량을 표현 한 것



KL divergence

이산확률분포 P와 Q가 동일한 샘플 공간 
$$
\chi
$$
에서 정의된다고 하면 KL divergence는 다음과 같다.
$$
D_{KL}(P||Q)= \sum_{x \in \chi}P(x)log_b(\frac{P(x)}{Q(x)})
$$

$$
D_{KL}(P||Q)= -\sum_{x \in \chi}P(x)log_b(\frac{Q(x)}{P(x)})
$$

$$
- \sum_{x \in \chi}P(x)log_bQ(x)+\sum_{x \in \chi}P(x)log_bP(x)
$$

$$
-E_P[log_bQ(x)]+E_P[log_bP(x)]
$$
기댓값 연산자 E에 붙은 ...



## Semi-supervised learning 





# Experiments



few shot learning



knowledge distillation

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







https://angeloyeo.github.io/2020/10/27/KL_divergence.html

https://aclanthology.org/W18-3012/
