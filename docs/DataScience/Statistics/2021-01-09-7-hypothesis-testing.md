---
layout: default
title: "[Statistics] 7. 가설검정(1)(Hypothesis Testing)"
date: 2021-01-09
categories: [Statistics]
parent: Data Science
nav_order: 12
tags: [가설검정,t-test, z-score, 유의수준]
comments: true
---





---

## Hypothesis testing. Null vs alternative

## 가설검정. 귀무가설 vs 대립가설

---

* **Steps in data-driven decision-making**

   (1) 가설 세우기 -> (2) 적합한 검정방법 선택 ->(3) 가설 검정 -> (4) 결론 도출
   
   

### 가설이란?

: 테스트될 수 있는 아이디어

### 귀무가설과 대립가설: Null hypothesis & Alternative hypothesis

|                           귀무가설                           |                 대립가설                  |
| :----------------------------------------------------------: | :---------------------------------------: |
| 일반적으로 과거 이론이나 경험적으로 참이라고 믿어지지만 검정되어야 하는 가설 | 귀무가설에 대립하여 설정되는 또 다른 가설 |
|     H0 is true until rejected, 대부분 ho를 기각하고자 함     |                                           |

------

데이터의 종류에 따라 





---



# T-Tests 

T-검정: 두 집단의 평균차이를 검정

**독립변수**: 정성적 변수(명목, 서열 척도)

**종속변수**: 정량적 변수(등간, 비율 척도)

1. 귀무가설 설정
2. 대안 가설 설정
3. 신뢰도를 설정: 신뢰구간이 모수를 포함할 확률이 95%
4. p-value 확인



## T-test 사용 조건

1. 독립성

2. 정규성: t test에서는 중심극한정리를 사용하여 정규분포를 가정하기 때문에 표본수 30이상이어야 함 

   표본수 30미만일 경우 정규성 검정을 실시한다. (표본수가 10 이하면 윌콕슨순위합검정)
```python
#scipy.stats.normaltest(sample, axis=0, nan_policy='propagate')
from scipy.stats import normaltest
import numpy as np

sample = np.random.poisson(5, 1000) # normal 분포가 아님
normaltest(sample) 
```

3. 등분산성: levene's test

cf) 샘플의 size가 크면 pvalue가 0.1보다 클 확률이 커진다.



## Test statistics(검정통계량)

(observed data - what we expect if the null is true) / average variation

 ### Sample 그룹의 개수에 따라: One sample vs Two sample

---
#### One sample t-test

샘플 데이터의 평균이 popmean이랑 같은지 검정

```python
from scipy import stats
scipy.stats.ttest_1samp(sample, popmean, axis=0, nan_policy='propagate', alternative='two-sided')

sample = np.random.binomial(1,0.55,100)
stats.ttest_1samp(sample, popmean)  #sample: sample데이터, p: p=0.5인지 가설검정
>>>(statistic, pvalue) #t값(샘플데이터가 모수와 멀이질수록 커짐), p값
```



#### Two sample t-test

2개의 샘플 값들의 **평균**이 동일한지 비교한다

1. 귀무가설: 두 샘플의 P(H)의 평균은 같다
2. 대안가설: 같지 않다
3. 신뢰도: 95%

```python
np.random.seed(1)
coin1 = np.random.binomial(n = 1, p = 0.6, size = 500)
coin2 = np.random.binomial(n = 1, p = 0.5, size = 200)

stats.ttest_ind(coin1, coin2) #두 샘플은 독립적인가?
>>>statistic=2.521, pvalue=0.011 #귀무가설 기각->독립적이지 않다. 연관이 있다
```



### 가설의 종류에 따라: Two-sided test vs One sided test

---

### Two-sided test vs One-sided test(양측검정과 단측검정)

| 양측검정(2 tail test)                                        | 단측검정(one tail test)                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 대립가설에서 =/=의 형태로 모수의 영역 표시하면, 귀무가설의 data보다 작거나 큰 양쪽 영역을 다 포함하므로 양측검정 | 대립가설이 '<', '>' 와 같이 부등호로 표시되면 귀무가설은 한쪽 영역만 고려하므로 단측검정 |
| H0: data = data0                                             | H0: data = data0                                             |
| H1: data =/= data0                                           | H1: data > data0                                             |

standard error: because we have sample variance of two groups.

p value 값이**(1-Confidence)** 보다 작으면 귀무가설을 기각한다.

**standard Error(표준 오차, SE)**: 통계의 표본 분포의 표준 편차, 표본 평균 분포의 표준 편차

![{\displaystyle \sigma =\pm {\sqrt {\frac {\Sigma (x-{\bar {x}})^{2}}{n(n-1)}}}=\pm {\sqrt {\frac {\Sigma \nu ^{2}}{n(n-1)}}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/61735ebe993a9679c0dd7d468d67ba03a4f282c7)

(n-1로 나누는 거는 불편 추정량인 표본분산으로 구해서)

표준편차: 모집단에 속해 있는 하나의 자료가 가지고 있는 흩어짐의 정도. 모분산의 제곱근

표준오차:표본에서 얻은 추정량이 가지고 있는 흩어짐의 정도. 추정량 분산의 제곱근



 *편의 추정량은 n으로 나누지만 불편 추정량은 n-1로 나누며 통계분석에서는 일반적으로 편의 추정량 보다는 불편 추정량 사용

<img src="https://jungminded.weebly.com/uploads/1/3/5/8/13582285/7285766_orig.jpeg?160" alt="Standard Error of the Sample Mean" style="zoom:50%;" />

1. we can calculate the critical t-value and if our t-statistic is greater than the critical value we reject the null hypothesis
2. We can calculate the P-value from our T-statistic and we can reject the null hypothesis if the P-value is smaller than our chosen Alpha level.

Alpha(유의수준): arbitrary 하지만 보통 0.05

0.05로 두고 T-distribution에서 양 끝을 자름

**증거의 부재가 부재의 증거는 아니다**



paired t test 



T-statistics tell us how many standard errors away from the mean our observed difference is

<img src="C:\Users\Boyoon Jang\Pictures\normal distribution.PNG" alt="normal distribution" style="zoom:60%;" />

---



## A/B test

A/B test : 2표본 가설 검정

Control group(standard group)  vs Treatment group

1. 통제집단과 실험집단 size 같아야 함
2. 두 집단을 구성할 때 random하게 골라야 함
3. binomial

```
df = pd.DataFrame({action: [0,1,0,1,1], outcome: [1,1,0,0,1]})
df.action.value_counts()           #action을 취한 샘플의 개수
df.outcome.value_counts()          #outcome의 개수

#treatment그룹과 control그룹 나눠서 outcome 알아보기
treatment = df[df.action == 1].outcome.values
control = df[df.action == 0].outcome.values

len(tretment) == len(control)
>>>True

```

**P-value**

증명하고 싶은 가설: 대립가설(Alternative Hypothesis)

대립 가설을 부정하는 쪽: 귀무가설(Null Hypothesis)

귀류법으로 증명: 대립 가설을 증명하기 위해 귀무가설이 틀렸다는 것을 증명

귀무가설을 측정하는 척도가 P

P값: 특정 값이 확률분포의 어딘가에 해당하는지 나타내는 확률

유의 수준: 임계치로서의 P값



**세 개 이상의 집단을 분석하고 싶다면->ANOVA**



**Q-value**

다중 검정과 FWER

다중 검정: 여러 샘플에서 '유의'가 되는지 여부를 확인하는 것

FWER(Familywise error rete): 다중검정 중 어느 하나가 '유의'가 될 확률
$$
FWER: 1-(1-p)^n
$$

[^ ]: p: '유의'가 될 확률

FDP(False Discovery Rate): 다중성에 따라서 어느 정도는 틀려도 좋다는 발상에서 시작

FDR는 FP / (TP + FP)의 비율을 구하는 방법

[^ ]: FP(False Positive): 1종 오류, TP(True Positive): 귀무가설이 옳지 않을 때 이를 기각

>  그래서 FDR는 기대치로 구합니다. P(Provability) 값이 확률인데 반해, FDR는 기대치인 것입니다. 유의 수준 P와의 관계로 말하자면, FWER을 작게 하기 위해서는 P를 작게 할 필요가 있으며, P를 크게 하면 FWER은 커집니다. 한편 FDR 쪽은 P를 크게 하면 FDR은 커지고, P를 작게 하면 FDR은 작아집니다.

Benjamini-Hochberg(BH 법): FDR 제어법

1. N개의 귀무가설의 p를 계산

2. p를 오름차순으로 정렬, 정렬 순서를 i라고 설정

3. FDR의 임계치 결정(유의수준 결정)

4. 각 샘플의 p를 q값으로 변환
$$
q_i = p_i * \frac{N}{i}
$$
[^ ]: q는 FDR의 기대치

5. $q_i 와 q_i +1을 비교하여 qi > qi+1이면 qi = qi +1로 함
6. qi와 임계치를 비교하여 기각 판정  

Q-value: 검정 결과가 유의하다고 판단되는 최소의 FDR 임계치

출처: https://doooob.tistory.com/101