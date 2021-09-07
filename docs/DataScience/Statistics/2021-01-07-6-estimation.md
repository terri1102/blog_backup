---
layout: default
title: "[Statistics] 6. 추정(Estimation)"
date: 2021-01-07
category: [Statistics]
parent: Data Science
nav_order: 11
tags: [점추청, 구간추청, 신뢰구간, CI]
use_math: true
comments: true
---



> 표본에서 얻은 통계량(statistic)으로 모수(parameter)를 추론하는 데 사용한다면 이것은 추정량(estimator)으로 불린다. 이와 같이 표본에서 얻은 정보를 이용하여 모집단에 대해 알아보려고 하는 것을 추론(inference)이라고 한다. **추론**에는 **추정(estimation)**과 **검정(testing)**이 있다. **추정**은 모수를 점(point) 또는 구간(interval)으로 추측하려는 것이다. **검정**은 모집단에 대한 가설을 세운 후 그 가설이 표본에 의해 입증되는지 알아보려는 것이다.



유의성 검정: 샘플이 모수를 얼마나 반영하는지 검정 <->가설 검정



# 1. 점추청

---

**점추청(point estimation)**: 모수를 어떤 하나의 값으로 추정



### 좋은 추정량의 기준

#### 1. 비편향성(unbiasedness): 우리가 추정하려는 것을 추정하고 있는지 판단

표본으로부터 얻어지는 점추정에는 항상 표집오차(sampling error)가 존재한다. 모집단 관심 모수 

$$
\theta
$$
에 대한 추정량을 
$$
\hat{\theta}
$$
이라고 하면 추정량의 표집오차는 
$$
\theta - \hat{\theta}
$$
로 정의되고, 이는 변동과 편향으로 분해될 수 있다.

추정량의 표집오차 = 
$$
\hat{\theta}-\theta
$$
= 변동 + 편향

변동 = 
$$
\hat{\theta}-E(\hat{\theta})
$$

편향 = 
$$
E(\hat{\theta})-\theta
$$

비편향추정량: 편향이 0인 추정량

**비편향추정량**: 만약 
$$
E(\hat{\theta})-\theta
$$
이면, 
$$
\hat{\theta}
$$
은 
$$
\theta
$$
에 대한 비편향추정량이다. 

일치하지 않으면 편향추정량. 

* 표본분산 S^2 를 정의할 때 n으로 나누지 않고 n-1로 나누는 이유는, n으로 나누면 편향추정량이 되고 n-1로 나누어야지만 비편향추정량이 되기 때문이다. 
* *표본분산: 

$$
\frac{n}{n-1}\sigma^2
$$



#### 2. 효율성(relative efficiency): 추정량의 작은 분산

추정량 변동의 제곱의 기대값은 그 추정량의 분산으로 표현될 수 있고, 따라서 변동을 줄이려면 추정량의 분산을 줄이면 된다. 어느 추정량의 분산이 다른 추정량의 분산보다 작은 경우 이를 상대적 효율성이 높다고 표현한다. 

추정량이 같은 형태의 표본함수라면, 표본수 n이 클 때 분산(\sigma^2/n)은 작아진다. 즉, 표본 크기가 클수록 그에 따른 표본평균은 더 효율적인 추정량이다. 같은 표본에서 도출된 비편향추정량 중에서 분산이 최소가 되는 추정량이 있다면, 이를 최소분산비편향추정량이라 하며, 이는 일반적으로 점추정에서 가장 이상적으로 추구하는 추정량의 하나이다. 



#### 3. 일치성(consistency): 추정량의 모수로의 확률적 수렴성

**확률적 수렴**은 임의 양의 상수 
$$
\epsilon
$$
에 대하여 
$$
lim_{n->\infty}Pr(|\hat{\theta}-\theta|)>\epsilon) =0
$$
이 성립하는 것

[^ ]: n이 무한대로 갈 때 Pr()가 1이 되는 theta_hat은 일치추정량임

표본평균, 표본중위수 또는 n-1로 나눈 평균은 모두 
$$
\mu
$$
에 대한 일치추정량



# 2. 구간추정(interval estimation)

---

### 신뢰구간(confidene interval, CI)

**신뢰수준**: 신뢰구간이 모수를 포함할 비율

**신뢰구간 및 신뢰수준의 의미!**  신뢰수준이란 신뢰구간이 모수를 포함할 비율<->~~모수가 구간에 포함될 확률~~(모수는 불변이니까)

95%의 신뢰수준일 때 똑같은 방법으로 100차례 표본을 추출하여 신뢰구간을 구하면 그 100개의 신뢰구간 중 95개가 모수 
$$
\mu
$$
를 포함하게 됨.



**오차한계(d)**: 신뢰구간 폭의 절반

![](C:\Users\Boyoon Jang\Pictures\모평균 구간추정에 사용하는 통계량.PNG)





$$
\theta
$$
를 알 경우: z-score 사용 
$$
(\bar{X}-z_{\alpha/2}\frac{\sigma}{\sqrt{n}}, \bar{X}+z_{\alpha/2}\frac{\sigma}{\sqrt{n}})
$$

[^ ]: 모집단의 정규분포 구/간을 모평균에 대한 구간처럼 만든 것

$$
(\bar{x}-z_{\alpha/2}\frac{\sigma}{\sqrt{n}}, \bar{x}+z_{\alpha/2}\frac{\sigma}{\sqrt{n}})
$$

[^ ]: 표본평균값X_bar의 실측치  x_bar으로 대체한 것



$$
\theta
$$
를 모를 경우: t-score 사용

$$
(\bar{x}-t_{\alpha/2}\frac{\sigma}{\sqrt{n}}, \bar{x}+t_{\alpha/2}\frac{\sigma}{\sqrt{n}})
$$

 



#### 신뢰구간(CI) 

$$
(\bar{X}-z_{a/2}\frac{\sigma}{\sqrt(n)}, \bar{X}+z_{a/2}\frac{\sigma}{\sqrt(n)})
$$

**t분포**: 모분산을 모르는 경우 t-score
$$
\frac{\bar{X}-\mu}{s/\sqrt{n}}
$$
 (s는 표준편차)는 z-score 
$$
\frac{\bar{X}-\mu}{\sigma/\sqrt{n}}
$$
와 달리 표준정규분포와 조금 다른 형태인 t분포를 따르는데, 이 t분포는 표본수에 따라 분포 모양이 조금씩 달라진다.  s는 표준편차(여기선 **t-score**로 구하는 것)고 sigma는 **z-score**로 구하는 것 의미 . s는 sigma보다 덜 편향적임. sigma는 주로 모집단의 표준편차 의미.

이는 자유도
$$
(\nu=n-1)\geq30
$$
에 의해 t분포 모양이 결정되기 때문이다. 표본수가 어느정도 큰 경우 표준정규분포와 비슷해지며 n>30인 경우 표준정규분포를 이용해 확률을 구해도 큰 차이를 보이진 않는다. t분포는 표준정규분포와 전체적인 형태는 같으나 옆으로 넓게 퍼진 형태를 취하고 있다.

```python
from scipy import stats

def confidence_interval(data, confidence = 0.95):
      data = np.array(data)
  mean = np.mean(data)
  n = len(data)
  
  # std / sqrt(n)
  stderr = stats.sem(data) 
  # Standard Error of Mean (https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.sem.html)
  # s / sqrt(n)

  # length_of_one_interval
  interval = stderr * stats.t.ppf( (1 + confidence) / 2 , n - 1) # ppf : inverse of cdf     n-1:dof
  return (mean, mean - interval, mean + interval)
```



## 4. 모비율 p에 대한 구간 추정

---

위의 모평균에 대한 구간 추정과 비슷하다. x를 p로 바꾸면 됨 



## 5. 표본수의 결정

---



### 큰 수의 법칙: 

sample 데이터의 수가 커질 수록, sample의 통계치는 점점 모집단의 모수에 가까워진다.

**오차한계(margin of error)**의 정의상 표본수가 늘어날 수록 오차한계는 작아지고 신뢰구간은 좁아진다.
$$
d = z_{a/2}\frac{\sigma}{\sqrt{n}}
$$

$$
z-score: \frac{x-\mu}{\sigma}
$$

[^ ]: z-score: 표본데이터를 표준화한 값, 모수가 알려져 있을 때 z-score, 알려져 있지 않다면 t-score를 사용

```python
import numpy as np
population = np.random.normal(loc=50,scale=10,size=1000) 
#평균이 50이고 표준편차 10인 데이터 1000개로 이루어진 정규분포를 따르는 array 생성 
np.random.seed(100)
population.var()  #sigma^2 = 90.98 (random이어서 고정은 아님)
```

```python
np.random.seed(100)
np.random.choice(population, 5).var() # s^2: 64     
np.random.choice(population, 200).var() # s^2: 105
#데이터가 큰 200이 더 모분산에 가까운 것을 알 수 있다.
```



### 중심극한정리: 
동일한 확률분포를 가진 독립 확률 변수 n개의 **평균**의 분포는 n이 적당히 크다면 정규분포에 가까워 진다.

효용: 샘플의 분포에 상관없이 샘플평균의 분포는정규분포를 따르니 분석시 샘플의 분포에 대해 고민할 필요가 적어진다. 보통 n이 30이상일 때 사용된다.



위 두 정리 덕분에 t-test나 ANOVA 검정시 표본의 수가 크면 정규성 검정 생략 가능