---
layout: default
title: "[Statistics] 1. 수치적 기술통계"
date: 2021-01-02
parent: Data Science
nav_order: 6
tags: [statistics, 수치적 기술통계, 통계]
category: [Statistics]
comments: true
use_math: true
---



# EDA(탐색적 데이터 분석)



### 측정 단위(수준)

**Categorical variable(범주형 변수)**

1. **명목 척도(nominal measures):** 사람이나 사물의 속성을 구분하기 위해 이름을 부여한 척도

   퍼센트나 빈도로 보고한다. ex) 성별, 혈액형

2. **서열 척도(ordinal measures):** 사람이나 사물의 속성에 대하여 상대적인 서열을 표기하는 척도. 

   척도 단위 사이의 등간성이 존재하지 않는다. ex) 성적 순위, 상/중/하로 구분된 지표

**Numerical variable(수치형 변수)**

3. **등간 척도(interval measures):** 측정단위 간에 등간성이 유지되는 척도

   측정단위 간에 등간성이 유지되고 임의 단위가 있다. 덧셈은 가능하나 곱셈은 불가하다.

   ex)온도, 이산 데이터(횟수와 같은 정수 값만 취할 수 있는 데이터)

4. **비율 척도(ratio measures):** 절대 영점을 갖고 측정단위 간에 등간성을 유지하며 측정단위가 협의된 척도.

   등간성 있고 절대 영점 있으며, 임의 단위가 있어 덧셈, 곱셈 모두 가능하다.

   ex)무게, 길이, 나이, 거주 기간

수치형(연속, 이산)과 범주형(이진, 순서)로 나누기도 한다.



### **통계 분석의 종류**

**기술통계(descriptive statistics):** 자료의 특성을 정리하고 요약할 목적으로 사용된다. 1962년 존 투키가 논문 <데이터 분석의 미래>에서 통계학의 개혁을 요구하며 탐색적 데이터 분석이라는 분야를 정립했다.

**추리통계(inferential statistics):** 표본 자료에서 얻은 결과를 바탕으로 모집단에 대한 추리를 하는데 사용한다. 주로 고전적인 통계학에서는 이런 추리통계를 많이 사용했다.

<br>

## 1. (중심) 위치의 척도

데이터를 살펴보는 기초적인 단계로 각 피처(변수)의 대푯값(typical value)을 구하는 것이며, 이는 대부분의 값이 어디쯤 위치하는지(중심경향성)를 나타내는 추정값이다.



**평균(mean)**: 산술평균. 가장 많이 쓰이지만 특잇값이 있으면 왜곡된다. 특잇값의 영향을 줄이기 위해 위아래 극단값들을 제거한 절사평균(trimmed mean)을 사용하기도 한다. 그 외에도 가중평균을 이용해서 변동이 큰 변수에 더 작은 가중치를 주거나, 데이터가 부족한 소수 그룹에 더 높은 가중치를 두는 등 데이터에 맞게 변형해 사용한다.

```python
#절사 평균
from scipy.stats import trim_mean
trim_mean(df['number_of_population'], 0.1) # 상하단의 0.1%를 제거한 평균값
```

```python
#가중 평균
import numpy as np
np.average(df['number_of_population'], weights=df['State'])
```

**중간값(median)**: 데이터에서 가장 가운데 위치한 값. 특이하게 작거나 큰 값에 영향을 받지 않고 사용할 수 있는 값이다. 평균보다는 특잇값에 로버스트하다.

```python
#가중 중간값
!pip install wquantiles
import wquantiles
wquantiles.median(df['number_of_population'], weights=df['State'])
```

**최빈값(mode)**: 빈도수가 최대인 값. 명목형 자료에서 주로 사용

```python
df.discribe() #count, mean, std, min, quartiles, max
```

**로버스트(robust)**: 극단값들에 민감하지 않다.(유의어: resistant)



## 2. 변동성 척도(variability)

**변동성 척도(퍼짐척도)** : 데이터 값이 얼마나 밀집해 있는지 혹은 퍼져 있는지를 나타내는 산포도(dispersion)

**범위(range)**: 데이터의 최댓값과 최솟값의 차이. 이상값 존재시 자료의 범위가 왜곡된다.

**사분위간 범위**:  Q3-Q1, 특이값에 의한 왜곡 줄일 수 있다.

**편차(deviation)**: 관측값과 위치 추정값 사이의 차이

**분산(variance)과 표준편차(standard deviation)**: 모든 자료가 그 평균으로부터 떨어져 있는 거리를 제곱한 것의 평균값 & 그것의 제곱근이 표준편차


$$
variance = s^2 = \frac{\sum^n_{i=1}(x_i-\bar{x})^2}{n-1}
$$

$$
standard deviation = s = \sqrt{variance}
$$

```python
numpy.var(arr) # 분산
numpy.std(arr) # 표준편차
```

자유도: 추정값을 계산할 때 제약 조건(constraint)의 개수. **표본**의 분산을 구할 때 n-1로 나누는 이유. 만약 분산 수식에 n을 분모로 사용하면 모집단의 분산과 표준편차의 참값을 과소평가하게 되는 편향(biased) 추정이 일어난다. n-1로 나누면 이 분산은 비편향 추정(unbiased)이 된다. 표준편차는 표본의 평균에 따른다는 하나의 제약 조건을 가지고 있기 때문에 n-1의 자유도를 갖는다.

분산과 표준편차는 가장 많이 쓰이는 변이 측정 방법이지만 특잇값에 민감하다는 단점이 있다. 중간값과 백분위수로부터 평균절대편차와 중간값의 중위절대편차(MAD)를 구하는 것이 좀 더 로버스트 하다.

**중간값의 중위절대편차(MAD)**


$$
중위절대편차 = 중간값(|x_1-m|,|x_2-m|,...,|x_n-m|)
$$


```python
from statsmodels import robust
robust.scale.mad(df['number_of_population']) #MAD는 보통 표준편차의 절반 정도인데, 이는 표준편차는 이상치에 민감하기 때문
```

**백분위수(percentile)** : 전체 데이터의 P%를 아래에 두는 값 (유의어: 분위수 quantile)

```python
#백분위수 구할 때
df['number_of_population'].quantile([0.5, 0.75])
```



* **변동계수(Coefficient of variation)**:  측정 단위가 다른 자료를 비교
$$
(std/mean) * 100 %
$$

```python
scipy.stats.variation(arr, axis=0, nan_policy='propagate')
```

axis: 0이면 행, 1이면 열, None이면 전체 arr, nan_policy: propagate-nan으로 출력, omit-nan 무시



## 3. 특이값의 발견

**특잇값(outlier)**: 어떤 데이터의 집합에서 다른 값들과 멀리 떨어져 있는 값

**z스코어 값**이 -2에서 2, -3에서 3 사이의 범위를 벗어나는 것을 특이값으로 생각

$$
z-score: (x_i - \bar{x}) / std
$$

```python
from scipy.stats import zscore
zscore(a_region, axis=0, ddof=0, nan_policy='propagate')
```

보통 EDA를 통해 특잇값을 찾을 때 boxplot을 자주 이용한다.

```python
ax = (df['number_of_ppl']/100000).plot.box()
ax.set_ylabel('Population (millions)')
```

![boxplot](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/statistics/boxplot.jpg?raw=true)



## 그래프로 데이터 분포 탐색

**상자그림(boxplot)** : 데이터의 사분위수와 분포의 꼬리를 잘 표현하는 그림

**도수분포표(frequency table)** : 어떤 구간(bin)에 해당하는 수치 데이터 값들의 빈도를 나타내는 기록

```python
#df를 분위별로 잘라서 개수 세줌
binned = pd.cut(df['number_of_population'], 10) #cut: 값들을 구간에 매핑
binned.value_counts()
```

**히스토그램(histogram)** : x축은 구간, y축은 빈도수를 나타내는 도수 테이블의 그림

**밀도 그림(density plot)**: 히스토그램을 곡선으로 나타낸 그림, 커널밀도추정(kernel density estimation)을 주로 사용

```python
import matplotlib.pyplot as plt

fig, axs = plt.subplots(1, 2, figsize=(10,5))
#히스토그램
plt.style.use("ggplot")
ax1 = plt.subplot(1,2,1)
ax1 = (state['Population']/1000000).plot.hist()
ax1.set_xlabel('Population (millions)');

#밀도그림
plt.style.use("seaborn")
plt.subplot(1,2,2)
ax2 = state['Murder.Rate'].plot.hist(density=True, xlim=[0,12], bins=range(1,12))
state['Murder.Rate'].plot.density(ax=ax2);
ax2.set_xlabel("Murder Rate (per 100,000)")


plt.tight_layout()
plt.show()
```

![hist_kde](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/statistics/hist_kde.jpg?raw=true)



### 범주형 데이터 EDA

**최빈값(mode)**: 데이터에서 자주 등장하는 범주, 값

**기댓값(expected value)**: 범주에 해당하는 어떤 수치가 있을 때, 범주의 출현 확률(probability)에 따른 평균. 어떤 값과 그 값이 일어날 확률을 곱해 더한 값을 의미.

**막대도표(bar chart)**: 각 범주의 빈도수 혹은 비율을 막대로 나타낸 그림

**파이그림(pie chart)**: 각 범주의 빈도수 혹은 비율을 부채꼴 모양으로 나타낸 그림



## 4. 연관성 척도(measure of association)



### 1)순서형 자료에 대한 연관성 척도: 스피어맨 순위 상관계수

스피어맨의 로(Spearman's rho)나 켄들의 타우(Kendall's tau) 등은 데이터의 순위를 기초로 하는 상관계수이다. 이러한 추정법은 특잇값에 좀 더 로버스트하며 비선형 관계도 다룰 수 있다. 



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


$$
q_{jk}=\frac{1}{N-1}\sum^N_{i=1}(x_{ij}-\bar{x_j})(x_{ik}-\bar{x}_k)
$$




```python
numpy.cov(m, y=None, rowvar=True, bias=False, ddof=None, fweights=None, aweights=None)
```



### 상관계수

**상관계수(Correlation coefficient)**: 수치적 변수들 간에 어떤 관계가 있는지를 나타내기 위해 사용되는 측정량. (-1에서 +1까지의 범위). 주로 산점도나 히트맵을 통해서 시각화한다.



**피어슨상관계수(Pearson correlation coefficient)**: 

피어슨 상관계수를 계산하려면 변수1과 변수2 각각의 평균으로부터의 편차들을 서로 곱한 값들의 평균을 각 변수의 표준편차의 곱으로 나눠준다.


$$
r = \frac{\sum^n_{i=1}(x_i-\bar{x})(y_i-\bar{y})}{(n-1)s_xs_y}
$$


변수들이 선형적인 관계를 갖지 않는 경우, 상관계수는 유용한 측정지표는 아니다.



Cov(X,Y)의 X와 Y의 단위는 각각 다르기 때문에 공분산의 정도를 보여주지는 못 하기 때문에, 두 숫자형 변수 사이의 선형적 강도를 나타내기 위한 통계량으로 공분산을 각 변수의 표준편차로 나눈 것이다. 

$$
\rho _{XY} = \frac{σ_{XY}}{σ_X * σ_Y}
$$

###### σXY:(X,Y)의 모집단공분산, σX, σY: X와 Y의 표준편차

- 공분산은 계측단위에 따라 그 크기가 달라질 수 있으나 상관계수는 표준화된 척도로 계측단위에 영향을 받지 않는다.

```python
scipy.stats.pearsonr(x, y)         #x,y = array 
                                   #>>피어슨상관계수, pvalue 출력
```

<->스피어만 상관계수: 모집단의 결합분포를 알지 못하는 경우, 분포에 의존하지 않는 순위간의 상관을 이용(15. 비모수통계 참조)



```python
#히트맵
fig, ax = plt.subplots(figsize=(5, 4))
ax = sns.heatmap(etfs.corr(), vmin=-1, vmax=1, 
                 cmap=sns.diverging_palette(20, 220, as_cmap=True),
                 ax=ax)

plt.tight_layout()
plt.show()
```

![heatmap](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/statistics/heatmap.jpg?raw=true)

```python
#산점도
plt.style.use('seaborn-white')
ax = telecoms.plot.scatter(x="T", y="VZ", figsize=(4,4), marker = '$/u25ER$');
ax.set_xlabel('ATT (T)')
ax.set_ylabel('Verizon (VZ)')
ax.axhline(0, color='grey', lw=1);
ax.axvline(0, color='grey', lw=1);
```

![scatter](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/statistics/scatter.jpg?raw=true)





**X와 Y의 모집단상관계수 성질**


$$
-1 ≤ \rho _{XY} ≤ 1
$$

- 모집단상관계수는 단위가 없다.
- 0 < pXY ≤ 1: X와 Y는 양의 선형적 관계 (pxy=1 이면 완벽한 양의 선형관계(한 직선에 있음))
- -1 ≤ pXY < 0: X와 Y는 음의 선형적 관계
- pXY = 0:  X와 Y는 선형적 관계가 존재하지 않음
- 상관계수를 구할 때, 산점도는 항상 그려보아야 한다!





**X와 Y의 표본상관계수**


$$
\gamma_{xy} = \frac{s_{xy}}{std_x * std_y}
$$


###### s_{xy}는 (x,y)의 표본공분산 



## 다변량분석(multivariate analysis)

**육각형 구간(hexagonal binning)** : 두 변수를 육각형 모양의 구간으로 나눈 그림. 데이터의 개수가 많아서 산점도로 밀도를 알기 어려울 때 사용한다.

**등고 도표(contour plot)** : 두 변수의 밀도를 등고선으로 표시한 도표

**바이올린 도표(violin plot)** : 상자그림 + 밀도추정

```python
ax = sns.violinplot(airline_stats.airline, airline_stats.pct_carrier_delay, inner='quartile', color='white'); #quartile: 4등분 quantile: 등분
```



```python
#hexbin
ax = kc_tax0.plot.hexbin(x="SqFtTotLiving", y='TaxAssessedValue',
                         gridsize=30, sharex=False, figsize=(5,4))
ax.set_xlabel('Finished Square Feet')
ax.set_ylabel('Tax-Assessed Value')
```

![hexbin](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/statistics/hexbin.jpg?raw=true)



```python
#contour plot
fig, ax = plt.subplots(figsize=(4,4))
ax = sns.kdeplot(data=kc_tax0, x='SqFtTotLiving', y='TaxAssessedValue', ax=ax); #이거 엄청 오래 걸림.. 8분 2초 걸림
ax.set_xlabel('Finiahed Square Feet');
ax.set_ylabel('Tax-Assessed Value');
```

![contourplot](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/statistics/contourplot.jpg?raw=true)



## Reference

통계학 입문

데이터 과학을 위한 통계