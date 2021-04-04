---
layout: default
title: "[지도학습] 선형회귀모델"
date: 2021-01-28
category: [Machine Learning]
nav_order: 4
parent: Machine Learning
excerpt: "단순선형회귀와 다중선형회귀의 가정, 과정, 유의성 검정(오류지표와 결정계수)"
tags: [단순선형회귀모형, 다중회귀모형, OLS, category encoders]
comments: true
---



[TOC]

# 선형회귀(Linear Regression)

---

지도학습의 종류 중 하나(연속형 변수). 다른 지도학습으로는 classification(이산형 변수)이 있음
#### Classification vs Regression

| property                     | Supervised Classification              | Regression                            |
| ---------------------------- | -------------------------------------- | ------------------------------------- |
| Output type                  | Discrete(class labels)                 | Continuous                            |
| What are you trying to find? | Decision boundary                      | "Best fit line"                       |
| Evaluation                   | Accuracy, precision, recall, F1 method | "Sum of squared error" or "R squared" |



## 선형회귀의 가정

두 **연속형 변수** 간의 연관성을 분석하는 모형. 범주형 변수 처리는 원핫인코딩 등 뒤에서 논할 예정이다.

1. 오차항의 평균은 0이고, 분산은 sigma^2이다(모든 오차항의 분산은 등분산이다)
2. 오차항들은 서로 독립이다
3. 오차항은 정규분포를 따른다



### 기준모델(Baseline Model) 만들기

선형회귀 모델을 만들기 전에 기준모델을 만들면 baseline을 알기 좋다.

**분류**-범주형 데이터: 타겟의 최빈 클래스

**회귀**-연속형 데이터: 타겟이 평균

**시계열회귀**- 이전 타임스탬프의 값



## 선형회귀의 이론적인 과정

1. 최소제곱법으로 데이터를 fit하는 선찾기
2. 회귀계수 유의성 검정(오류지표와 R2 구하기)
3. R2의 p-value 구하기



**단순회귀분석**은 Y변수와 Y변수를 설명하기 위한 X변수로 구성된다.

**X변수**: 설명변수(explanatory variable), 예측변수(predictor variable), 독립변수(independent variable)

**Y변수**: 반응변수(response variable), 종속변수(dependent variable)



### 단순선형회귀모형: 설명변수가 반응변수에 직선의 방정식으로 표현할 수 있는 모형

$$
Y = \beta_0 + \beta_1X+\epsilon
$$

**beta0**은 미지의 모수이며 y축의 절편으로 X=0일 때 Y값, **beta1**은 미지의 모수인 기울기(slope)로 X가 1단위 증가할 때 Y의 변동량이다.

**오차**(epsilon)는 확률변수로 Y와 X의 선형관계롤 설명할 수 없는 나머지 부분. 오차의 평균은 0, 분산은 sigma^2, 독립변수 x와 연관성이 없는 것으로 가정

(특수한 경우를 제외하고는 회귀모형에서 X를 확률변수가 아닌 상수값으로 가정한다.)



### 단순선형회귀식

단순선형회귀식에서 단순선형회귀모형에 있던 오차항(epsilon)가 빠지는 것은 단순선형회귀모형의 가정을 만족할 때만 회귀식이 설명할 수 없는 오차를 무시할 수 있기 때문????????
$$
E(Y)= \beta_0 + \beta_1X
$$



### 추정된 단순선형회귀식

$$
\hat{y}=b_0+b_1x
$$



### 최소제곱법(OLS)으로 회귀모델 구하기

**최소제곱법:** 잔차의 제곱의 합을 최소로 만드는 회귀선 찾는 방법이다. 최소제곱법(OLS)는 가중최소제곱법(Weighted least squares method)과 다르게 가중치를 두지 않고, 모든 데이터 값의 오차를 같은 비중으로 취급한다. 
$$
(b_0, b_1)= argmin \sum^n_{i=1}\epsilon_i^2=argmin \sum^n_{i=1}(y_i-\beta_0-\beta_1x_i)^2
$$

$$
b_1 = \frac{\Sigma(x-\bar{x})(y-\bar{y})}{\Sigma(x-\bar{x})^2}
$$

$$
b_0 = \bar{y}-b_1\bar{x}
$$



**예시**
$$
\hat{y}= b_0 + b_1x
$$


| x      | y      | x-x_bar | y-y_bar | (x - x_bar) | (y-y_bar) | (x - x_bar)^2 | (x-x_bar)(y-y_bar) |
| ------ | ------ | ------- | ------- | ----------- | --------- | ------------- | ------------------ |
| 1      | 2      | -1      | -2      | -2          | 4         | 4             | 4                  |
| 2      | 5      | -1      | 0       | -1          | 0         | 1             | 0                  |
| 3      | 5      | 0       | 1       | 0           | 1         | 0             | 0                  |
| 4      | 4      | 1       | 0       | 1           | 0         | 1             | 0                  |
| 5      | 5      | 2       | 1       | 2           | 1         | 4             | 2                  |
| mean=3 | mean=4 |         |         |             |           | sum=10        | sum=6              |



따라서 여기서 b1은 6/10이기 때문에 회귀식의 기울기는 0.6이다. 회귀식은 x와 y의 평균을 지나기 때문에 회귀식에 x와 y의 평균인 (3, 4)를 대입해 b0을 구하면 된다.

$$
\hat{y} = b_0 + b_1x, \quad 4 = b_0 + 0.6*3
$$
따라서 b0 = 2.2이다.
$$
\hat{y} = 2.2 + 0.6x
$$

[^ ]: statquest



# Scikit-Learn으로 선형회귀 모델 만들기

나름대로 선형회귀분석의 순서를 만들어 보았는데, 앞으로 kaggle 등을 보고 보완할 예정이다.



## 1. EDA, Preprocessing, Feature Engineering

**특성을 고를 때 먼저 산점도를 그려봐서 `선형관계 확인`과 박스플롯과 describe로 `이상치` 확인 작업 먼저 할 것**! 

모델링하는 것보다 더 오래 걸리고 모델의 예측력을 높이는 데 중요한 작업이다.



**데이터의 성질 파악:** 결측치, numerical, categorical인지, 각 특성 간 관계는 어떤지 보기

```python
#!pip install -U pandas-profiling
from pandas_profiling import ProfileReport
profile = ProfileReport(df, minimal=True).to_notebook_iframe() #버전별로 다르니까 주의. 버전 안 맞으면 map이 없다며, minimal이 없다며... 실행 안 됨 
#df.profile_report()  #각 특성에 대한 분석을 상세하게 해준다

df.info()   #non-null object의 개수를 알려준다

df.describe() #4분위수, 최솟값, 최댓값, 평균 등 통계적 값을 알려준다
```



**결측치 처리**

```python
#데이터 사이즈가 크면 drop

#simple imputation

#multivariate imputation
```



**이상치 찾기**

```python
#1. 산점도로 파악

#2. boxplot으로 파악

#tuckey
```



**이상치 처리하기**

```python
sns.kdeplot(dataset1, shade=True)
```



**SelectKBest**로 모델에 사용할 특성 구하기

```python

```



**categorical variable 인코딩하기**(인코딩하고 안 하고의 차이를 보고 싶다면 나중에 모델 인스턴스 만들 때 하기)

```python
#nominal data를 onehotencode
from category_encoders import OneHotEncoder

encoder = OneHotEncoder(use_cat_names=True)
X_encoded = encoder.fit_transform(X)

#데이터를 이미 쪼갠 후에 인코딩
encoder = OneHotEncoder(use_cat_names=True)
X_train_encoded = encoder.fit_transform(X_train)
X_val_encoded = encoder.transform(X_val)   
```



**Scaling**

```python

```



**Feature Selection**

```python
features = ['사용할', 'features']
df = df[features]
label = df['target']
```



## 2. 데이터를 train, validation, test로 나누기

```python
from sklearn.model_selection import train_test_split

train, test = train_test_split(df, train_size = 0.8, test_size = 0.2, random_state = 2)
#df를 train과 test로 8:2로 쪼개줌

train, val = train_test_split(train, train_size = 0.8, test_size = 0.2, random_state = 2)
#train을 train과 val로 8:2로 쪼개줌

X_train, X_test, y_train, y_test = train_test_split(X, y, train_size = 0.8, test_size = 0.2, random_state = 1) #X, y df이 있을 때 X_train과 X_test, y_train과 y_test를 8:2의 비율로 쪼개줌
```



## 3. 기준모델 만들기

```python
#평균으로 기준모델 만들기
mean = y_train.mean()[0]           #mode: 최빈값, 여기서는 0

y_pred_b = [mean] * len(y_train)
```



## 4. 모델 인스턴스 만들기

```python
## Scikit-Learn에서 모델 import
from sklearn.linear_model import LinearRegression

#모델 인스턴스 만들기(instantiate model) #An instance is an exact replica of a model
model = LinearRegression()
```



## 5. 모델 학습

```python
#모델을 학습(훈련 데이터)
model.fit(X_train, y_train)
```

```python
#모델을 학습(검증 데이터): 모델을 검증 데이터에 최적화시키고 싶을 때! 하지만 (왠만해선) test 데이터는 학습시키면 안 됨
model2.fit(X_val, y_val)
```



## 6. 예측

```python
#train 데이터로 학습한 모델로 train 데이터 예측함
y_pred = model.predict(X_train) 

#val 데이터로 학습한 모델로 val 데이터 예측
y_pred2 = model2.predict(X_val)

#train 데이터로 학습한 모델로 val 데이터 예측 
y_pred3 = model.predict(X_val) #model2 아니라 model인 것 주의

#train 데이터로 학습한 모델로 test 데이터 예측
y_pred_t = model.predict(X_test)

#val 데이터로 학습한 모델로 test 데이터 예측
y_pred_t2 = model2.predict(X_test)
```

모델을 여러 개를 만들다보면 헷갈리니까 변수 설정에 신경쓰기!



## 7. 계수와 절편

**계수(coefficient)**


```python
#계수(coefficient): feature와 target의 관계, slope
model.coef_
```

**절편(intercept)**

```python
model.intercept_
```



## 8. 모델 비교, 평가(R2 score, MAE)





최적화 하기: val에 fit하기? 아니면 train+val로 fit하기? 즉 마지막 test set 의 mae, r2구하기 전 어떻게 하는 거지?

```python
#X 특성의 테이블, y 타겟 벡터 만들기
feature = ['X1']
target = ['y']
X_train = df[feature]
y_train = df[target]

#모델을 학습(fit)
model.fit(X_train, y_train)

X_test = [[x] for x in df_t['X1']]
y_pred = model.predict(X_test)

```



**산점도로 예측한 값과 실제 값 파악하기**

```python
#산점도: X_train, y_train
plt.scatter(X_train, y_train, color = 'blue', linewidth = 1)

#X_test, y_pred
plt.scatter(X_test, y_pred, color = 'red', linewidth =1)
```



## 선형회귀모델의 계수(Coefficients)와 절편(Intercept)





## 오류지표와 결정계수



## R^2 결정계수

| X    | y       | y-y_bar | (y-y_bar)^2 | y_hat | y_hat-y_bar | (y_hat-y_bar)^2 |
| ---- | ------- | ------- | ----------- | ----- | ----------- | --------------- |
| 1    | 2       | -2      | 4           | 2.8   | -1.2        | 1.44            |
| 2    | 4       | 0       | 0           | 3.4   | -0.6        | 0.36            |
| 3    | 5       | 1       | 1           | 4     | 0           | 0               |
| 4    | 4       | 0       | 0           | 4.6   | 0.6         | 0.36            |
| 5    | 5       | 1       | 1           | 5.2   | 1.2         | 1.44            |
|      | mean =4 |         | sum=6       |       |             | sum = 3.6       |

R^2 = 3.6/6 =0.6
$$
R^2 = \frac{\Sigma(\hat{y}-\bar{y})^2}{\Sigma(y-\bar{y})^2}
$$
R2이 1에 가까울수록 실제 값과 회귀선이 비슷하다, 더 좋은 fit라고 할 수 있다. 참고로 mean으로 만든 기준모델은 r2 결정계수의 공식상 r2 score가 0이 나온다.



**MSE(Mean Standard Error)**

위와 같은 예시로 설명

| X    | y    | y_hat | y_hat-y | (y_hat-y)^2 |
| ---- | ---- | ----- | ------- | ----------- |
| 1    | 2    | 2.8   | 0.8     | 0.64        |
| 2    | 4    | 3.4   | -0.6    | 0.36        |
| 3    | 5    | 4     | -1      | 1           |
| 4    | 4    | 4.6   | 0.6     | 0.36        |
| 5    | 5    | 5.2   | 0.2     | 0.44        |
|      |      |       |         | sum = 2.4   |

Standard Error of th Estimate =
$$
\sqrt{\frac{\Sigma(\hat{y}-y)^2 }{n-2}} = \sqrt{\frac{2.4}{5-2}} = \sqrt{0.8} =0.89
$$
[그림]



### bias and variance

bias: 모델이 train data와 잘 맞지 않는 것

variance: 모델이 test 데이터에 따라 얼마나 꾸준히? 맞는지..?얼마나 다른 데이터셋에 적용가능한지

bias는 내적타당성, variance는 외적타당성라고 이해했다.



단순선형회귀모델은 높은 편향(과소적합), 낮은 분산을 갖게 됨

비선형모델은 낮은 편향, 높은 분산을 갖게 됨(과적합)

statquest linear models pt1, pt2 보기



## 회귀진단 항목

* **모형의 선형성(linearity)**

* **오차의 정규성(normality), 등분산성(homoscedasticity), 독립성(independence)**

* **이상치(outlier)의 존재**

* **영향관찰치(influential observation)의 존재**

위와 같은 항목들을 진단하기 위해서는 **산점도**와 **박스플롯** 등 그래프를 그려봐야 한다. 



### 잔차그림: 오차의 등분산성 검정

X에 따른 잔차의 산점도를 x=X, y = e(y-y hat)로 그렸을 때, 어떠한 패턴도 보이고 있지 않다면 추정된 회귀식이 설명변수와 반응변수 간의 회귀모형을 적절하게 설명한 것이다. (산점도가 가로로 일자이거나 아예 패턴이 없는 경우)

만약 x 값이 증가할수록 잔차의 분산이 커지는 나팔관 형태라면 오차의 등분산 가정 위배(이분산)하고, 회귀식이 설명변수와 반응변수의 관계를 적절하게 설명하지 못하는 것이다.

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/img/residual_plot.png?raw=true" alt="residual_plot" style="zoom:67%;" />

[^ ]: 나팔관 형태의 분산

비선형관계, 이차 내지 고차방정식 유형으로 보일 때는 선형관계보다는 다항회귀나 비선형 모형으로 가정하고 재분석해야 한다. (산점도가 이차방정식 같은 고차방정식 모양처럼 생겼음)

---





# 다중선형회귀(Multiple linear regression model)

---



## 다중선형회귀모형의 가정

1. 오차항(epsilon_i)의 평균은 0: E(epsilon_i)=0
2. 오차항의 분산 sigma^2는 X_ij의 모든 값에 대하여 항상 같다(등분산 가정: Var(epsilon_i )= sigma^2)
3. 오차항들은 서로 독립적이다
4. 오차항은 정규분포를 따른다: epsilon_i ~N(0, sigma^2)



## 회귀진단 항목

**모형의 선형성(linearity)**

**오차의 정규성(normality), 등분산성(homoscedasticity), 독립성(independence)**

**이상치(outlier)의 존재**

**영향관찰치(influential observation)의 존재**

**다중공선성(multicollinearity)**



선형성, 이상치 존재여부, 영향관찰치의 존재여부: 산점도로 보기(pairplot으로 그려보기)

오차에 대한 가정사항 진단: 잔차그림을 그림(x=적합값, y=잔차):  하지만 차원이 높아져서 그리기 어려움



### 다중공선성 진단

1) 산점도행렬 또는 개별 X변수끼리의 산점도에서 거의 직선에 가까운 선형관계를 보임

2) X변수 하나를 뺄 때와 추가할 때 다른 X변수들의 기울기 추정값들이 많이 변동 

3) 분산분석표에 의해서 유의적으로 기울기들이 모두 0이 아니지만 문제가 되는 X변수의 기울기는 비유의적일 때

4) 분산확대인수(variance inflation factor) 계산하거나 통계적검정



## 가변수

회귀분석의 독립변수에 질적변수가 포함되었을 때 사용

원핫인코딩 등으로 가변수로 변환해서 사용

```python
!pip install category_encoders
from category_encoders import OneHotEncoder

encoder = OneHotEncoder(use_cat_names=True) #알아서 카테고리 변수만 변환해주니 pandas get dummies 보다 편하다
```

