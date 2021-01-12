---
layout: post
title: "2021년 1월 셋째주 TIL"
date: 2021-01-11
category: [TIL]
excerpt: "2021.01.11-2021.01.17까지 TIL"
tags: [ANOVA, levene, one-hot coding]
comments: true
---

## 2021.01.11(월)

---

자세한 코드나 오류 등은 Github TIL 레포에 저장하고 있고, 블로그 TIL엔 간단하게 오늘의 소감 정도만 적어야 겠다. 

##### 5F 회고법에 맞게 작성해보자

**Fact(사실)** : 오늘 스프린트 챌린지를 하면서 문제를 다 못 풀었다.

**Feeling(느낌)** : :3rd_place_medal: 개념을 헷갈려서 문제를 다 못 푼 것 같다. 

**Finding(교훈)** : :triangular_flag_on_post:

1.ANOVA가 단순히 t-test의 샘플이 많을 때 하는 검정이라고 알고 있었는데, ANOVA의 한 종류인 일원배치분산분석은  세 개 이상의 집단으로 구성된 하나의 독립변수에 따라, 종속변수의 평균이 유의미한 차이가 있는지 검증할 때 쓴다는 것을 알게 되었다.

```python
from scipy import stats
fvalue, pvalue = stats.f_oneway(sample1, sample2, sample3, sample4) #깔끔하게 프린트 된다
print("fvalue:", fvalue, "pvalue:",pvalue)  
```

2.t-test 쓰기 위한 조건이 독립성, 정규성, 등분산성인데 등분산성 테스트(levene's test)는 오늘 처음 써 봤다.

```python
stats.levene(subset1,subset2,subset3,subset4) #statistic값과 pvalue 나옴
```



**Future action(행동)** : 질문 or 더 찾아볼거리

1. categorical feature은 chi2_ind로, continuous feature은 ANOVA로 pvalue를 구했는데 이 두 개를 섞어서 비교 가능? 
2. ANOVA 제대로 썼는지... 

**Feedback(피드백)** : :ear: 앞으로 통계공부할 때는 이 개념을 언제 쓸 수 있는지, 어떻게 설정하는지, 사용할 수 있는지 여부를 염두에 두고 공부하라는 말을 들었다.



git hub 블로그 너무너무 어렵다...마크다운 편집기에서 작성한 거랑 왜 이렇게 다르게 보이는지...심지어 숫자도 달라져!

---

## 2021.01.12(화)

---

**Fact**: 오늘은 선형대수(기초 행렬, 벡터)를 numpy로 구현하는 법을 배웠다.

**Feeling**: :2nd_place_medal:통계학을 배울 때처럼 헷갈리는 개념은 별로 없었다.  gem도 깔고 config도 수정했는데 jemoji를 넣는 길은 요원하다...

**Finding**: 다른 분이 질문해주신 벡터 차원 관련 질문에서 tensor라는 개념을 새로 알게되었다!

* **tensor:** 선형관계를 나타내는 다중선형대수학의 대상

vector는 항상 1 tensor의 형태로 나타나지만, 다양한 차원을 가질 수 있다. 예를 들어 좌표평면에서 x,y를 표현할 때 주로 쓰는 2x1 벡터는 2차원이다.
$$
\vec{V1}= [0,1,1,3] \quad 1차원
$$

$$
\vec{V2} = [[0],[1]] \quad 2차원
$$

cf)tf.Tensor 객체의 랭크는 그 차원의 수이지만 수학에서 쓰는 행렬의 랭크와 다른 의미이다. 랭크가 1인 Tensor의 Type은 벡터라고 https://www.tensorflow.org/guide/tensor?hl=ko에서도 나와있다.



* **MAE와 MSE**

**MSE**(mean squared error, 평균 제곱 오차): 잔차(오차)의 제곱에 대한 평균을 취한 값 
$$
MSE = E [ (X - \hat{X})^2] = \frac{1}{N}\Sigma(X-\hat{X})^2
$$
**MAE**(mean absolute error, 평균절대오차) : 모든 절대오차의 평균
$$
MAE = \frac{1}{N}\Sigma(|X-\hat{X}|)
$$


**사용례**

**평균 제곱 오차(MSE)**는 회귀에서 자주 사용되는 **손실 함수**입니다. 정확도 개념은 회귀에 적용되지 않습니다. 일반적인 **회귀 지표**는 **평균 절대 오차(MAE)**입니다. MSE는 손실함수로써 쓰이고  MAE는 회귀지표로써 사용된다. 

[^ ]:https://blog.naver.com/PostView.nhn?blogId=heygun&logNo=221516529668&parentCategoryNo=&categoryNo=56&viewDate=&isShowPopularPosts=true&from=search

**Future action** 선형대수 정리한 것 복습

**Feedback** :woman_shrugging: