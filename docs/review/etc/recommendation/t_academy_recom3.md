---
layout: default
title: 추천시스템의 이해3
date: 2021-04-17
grand_parent: Reviews
parent: ETC
comments: true
nav_order: 12
---



# 평가함수



## 평가함수를 다양하게 알아야 하는 이유

평가함수는 추천시스템의 모델을 생성하고 해당 모델이 얼마나 잘 추천하고 있는지에 대해 평가를 도와주는 함수. 도메인이나 목적에 따라서 다른 평가 함수를 도입해서 얼마나 잘 추천이 되는지 평가하는 것이 중요.

* 내가 추천해준 아이템을 고객이 구매했나?
* 내가 추천해준 아이템을 고객이 높은 점수로 평가했나?

<br>

### Accuracy

$$
accuracy(y,\hat{y}) = \frac{1}{n_{samples}\sum_{i=0}^{n_{samples}-1}1(\hat{y_i}=y_i)}
$$



* 내가 추천해준 영화를 고객이 봤나? vs 보지 않았나?
* 내가 추천해주는 영화를 많이 볼수록 추천하지 않은 영화를 보지 않을수록 정확도는 상승
* 하지만, 추천하지 않은 영화의 수는 추천한 영화의 수에 비해 굉장히 많고 편향된 결과를 얻을 수 있음

그래서 추천해 준 영화 중 본 영화로만 평가를 해야한다. 근데 이렇게 해도 문제가 있다. 모든 상품을 추천하면 정확도는 1이 나오기 때문. 상위 n개의 상품만 추천한다고 했을 때 어느 정도의 정확도를 갖는지 판단하는게 제일 정확한 값을 얻을 수 있음. 또한, 추천순위를 고려하지 못함

<br>

### MAP

```
evaluate_func = evaluate(recs=total_rec_list, gt = gt, topn=200)
evaluate_func._evaluate()
```

| Recommendations | Precision @k's | AP@3-사용자 3명에 대한 평균 |
| --------------- | -------------- | ------------------ |
| [0,0,1]         | [0,0,1/3]      | (1/3) * (1/3) = 0.11 |
|[1,1,0]|[1/1, 2/2, 2/3]|(1/3)[1/1 + 2/2 + 2/3] = 0.89|
|[1,1,1]|[1/1, 2/2, 3/3]|(1/3)[1+2/2+3/3] = 1|

* Recommendations: 추천을 했는데 맞은 경우 1, 틀리면 0
* AP: Precision @k's를 평균낸 값 (추천한 K개의 영화의 Precision을 평균)
* MAP@4: 4명의 사용자의 AP를 평균낸 값 (Precision을 평균낸 AP를 4명의 사용자에 대해 평균)

MAP의 경우 추천의 순서에 따라서 값의 차이가 난다. 또한 상위 k개의 추천에 대해서만 평가하기에 k를 바꿔가면서 상위 몇 개를 추천하는 게 좋을지 결정할 수 있다.

<br>

### NDCG

MAP를 보안한 것.

<br>

NDCG(Normalized Discounted Cumulative Gain) : ranking quality measure, 검색 알고리즘에서 성과를 측정하는 평가 메트릭



추천엔진은 user와 연관있는 documents의 집합을 추천해주기 때문에, 단순히 문서 검색 작업을 수행한다고 생각할 수 있다. (검색과 관련있는 문서들을 추천) 따라서 NDCG를 사용하여 추천엔진을 평가할 수 있다. NDCG를 이해하기 위해선 Cumulative Gain과 Discounted Cumulated Gain을 이해해야 함.

<br>

**CG (cumulative gain):**
추천해줬을 때 맞은 순서를 더해주는 것

$$
Cumulative \; Gain(CG) = \sum^n_{i=1} relevance_i
$$

* relevance scores: 타겟 그룹이 추천 아이템에게 보여주는 긍정/부정적인 피드백 기반 스코어

set A = [2,3,3,1,2] <br>

set B = [3,3,2,2,1] <br>

CG_A = 2 + 3 + 3 + 1+ 2 = 11 <br>

CG_B = 3 + 3 + 2 + 2 + 1 = 11 <br>

 눈으로 봤을 때 B가 A보다 더 나은 추천 결과인 것을 알지만 CG의 관점에서 둘은 같은 점수. 이를 보완하기 위해 문서의 위치에 따른 Score를 반영해 줄 필요가 있음



**DCG(Discounted Cumulative Gain):**

CG에 문서의 위치 반영한 스코어. 앞에서 맞을수록 분모값이 더 작아짐 <br>
$$
DCG = \sum_{i=1}^n \frac{relevance_i}{log_2(i+1)}
$$
DCG_A = 6.64 <br>

DCG_B = 7.14 <br>

DCG_B가 더 높은 점수

<br>

**NDCG **: 

DCG를 이상적인 IDCG(가장 최적의 추천의 DCG)와 비교 <br>

Recommendations Order = [2,3,3,1,2] <br>

Ideal Order = [3,3,2,2,1] <br>
$$
DCG = \frac{2}{log_2(1+1)} + \frac{3}{log_2(2+1)} + \frac{3}{log_2(3+1)} + \frac{1}{log_2(4+1)}  + \frac{2}{log_2(5+1)}  
\approx 6.64 \\
iDCG  \frac{3}{log_2(1+1)} + \frac{3}{log_2(2+1)} + \frac{2}{log_2(3+1)} + \frac{2}{log_2(4+1)}  + \frac{1}{log_2(5+1)}  
\approx 7.14 \\

NDCG = \frac{DCG}{iDCG} = \frac{6.64}{7.14} \approx 0.93
$$


---

## 실습

데이터 탐색을 먼저 해야함



베이스라인 모델: 통계기반 추천



