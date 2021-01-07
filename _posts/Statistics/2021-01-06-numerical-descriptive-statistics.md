---
layout: post
title: "1. 수치적 기술통계"
date: 2021-01-06
excerpt: "수치적 기술통계에서 사용하는 중심위치 척도, 변동성 척도, 특이값, 연관성 척도, 분산"
tags: [statistics, 수치적 기술통계, 통계]
category: [Statistics]
comments: true
---



## 1. 중심위치의 척도

---

**평균(mean)**: 산술평균, 가장 많이 쓰이지만 특이값 있으면 왜곡됨

**중앙값(median)**: 특이하게 작거나 큰 값에 영향을 받지 않고 사용할 수 있는 값

**최빈값(mode)**: 빈도수가 최대인 값, 명목형 자료에서 사용



## 2. 변동성 척도

---

**변동성 척도(퍼짐척도)** : 통계자료의 변동 정도

* **범위(range)**: 최댓값 -최솟값 -> 문제: 이상값 존재시 자료의 범위가 왜곡됨

  

* **사분위간 범위**:  Q3-Q1, 특이값에 의한 왜곡 줄일 수 있음

  

* **분산(variance)과 표준편차(standard deviation)**: 모든 자료가 그 평균으로부터 떨어져 있는 거리를 제곱한 것의 평균값 & 그것의 제곱근이 표준편차
```
numpy.var(arr) # 분산
numpy.std(arr) # 표준편차
```


* **변동계수(Coefficient of variation)**:  측정 단위가 다른 자료를 비교
$$
(std/mean) * 100 %
$$

```
scipy.stats.variation(arr, axis=0, nan_policy='propagate')
```

[^ ]: axis: 0이면 행, 1이면 열, None이면 전체 arr, nan_policy: propagate-nan으로 출력, omit-nan 무시



## 3. 특이값의 발견

**z스코어 값**이 -2에서 2, -3에서 3 사이의 범위를 벗어나는 것을 특이값으로 생각
$$
z-score: (x_i - \bar{x}) / std
$$

```
from scipy.stats import zscore
zscore(a_region, axis=0, ddof=0, nan_policy='propagate')
```



## 4. 연관성 척도(measure of association)



### 1)순서형 자료에 대한 연관성 척도: 스피어맨 순위 상관계수

비모수 통계에서 공부할 예정



### 2)연속형 자료의 선형적 관계

#### 공분산(Covariance): 2개의 확률변수의 상관정도

1. 모집단 공분산
$$
Cov(X,Y) = σ_{XY} = \frac{1}{n}\sum_{i=1}^{n}(x_i −μ_x)(y_i−μ_y)=E{(X−μX )(Y−μY )}
$$
σ_XY > 0 : X와 Y는 양의 선형적 관계

σ_XY < 0 : X와 Y는 음의 선형적 관계

σ_XY = 0 : X와 Y는 선형적 관계를 갖지 않는다.



2. 표본공분산
   <img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/4d158b1ec5a3c6d1de84b9d59f604d8170a51407" alt=" q_{jk}=\frac{1}{N-1}\sum_{i=1}^{N}\left(  x_{ij}-\bar{x}_j \right)  \left( x_{ik}-\bar{x}_k \right), " style="zoom:67%;" />

```
numpy.cov(m, y=None, rowvar=True, bias=False, ddof=None, fweights=None, aweights=None)
```



#### 상관계수(Correlation coefficient)

**피어슨상관계수(Pearson correlation coefficient)**: 두 숫자형 변수 사이의 선형적 강도를 나타내기 위한 통계량으로 공분산을 각 변수의 표준편차로 나눈 것
$$
\rho _{XY} = \frac{σ_{XY}}{σ_X * σ_Y}
$$

[^ ]:  σXY:(X,Y)의 모집단공분산, σX, σY: X와 Y의 표준편차

- 공분산은 계측단위에 따라 그 크기가 달라질 수 있으나 상관계수는 표준화된 척도로 계측단위에 영향을 받지 않는다.

```
scipy.stats.pearsonr(x, y)         #x,y = array 
                                   #>>피어슨상관계수, pvalue 출력
```



**X와 Y의 모집단상관계수 성질**
$$
-1 ≤ \rho _{XY} ≤ 1
$$

- 모집단상관계수는 단위가 없다.
- 0 < pXY ≤ 1: X와 Y는 양의 선형적 관계
- -1 ≤ pXY < 0: X와 Y는 음의 선형적 관계
- pXY = 0:  X와 Y는 선형적 관계가 존재하지 않음
- 상관계수를 구할 때, 산점도는 항상 그려보아야 한다!



**X와 Y의 표본상관계수**
$$
\gamma_{xy} = \frac{s_{xy}}{std_x * std_y}
$$
[^ ]: s_{xy}는 (x,y)의 표본공분산 

**간단한 계산인 분산, 표준편차, 공분산은 scipy에서 deprecate할 거라 함**