---
layout: post
title: "선형회귀모델"
date: 2021-01-28
category: [Machine Learning]
MachineLearning : true 
excerpt: "단순선형회귀와 다중선형회귀의 가정, 과정, 유의성 검정(오류지표와 결정계수)"
tags: [단순선형회귀모형, 다중회귀모형, OLS, category encoders]
comments: true
---



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

**beta0**은 미지의 모수이며 y축의 절편으로 X=0일 때 Y값, **beta1**은 미지의 모수인 기울기(slope)로 X가 1단위 증가할 때 Y의 변동량

**오차**(epsilon)는 확률변수로 Y와 X의 선형관계롤 설명할 수 없는 나머지 부분. 오차의 평균은 0, 분산은 sigma^2, 독립변수 x와 연관성이 없는 것으로 가정

(특수한 경우를 제외하고는 회귀모형에서 X를 확률변수가 아닌 상수값으로 가정한다.)



### 단순선형회귀식





# 최소제곱법으로 회귀모델 구하기

**최소제곱법:** 잔차의 제곱의 합을 최소로 만드는 회귀선 찾는 방법

최소제곱법(OLS)는 가중최소제곱법(Weighted least squares method)과 다르게 가중치를 두지 않고, 모든 데이터 값의 오차를 같은 비중으로 취급한다. 

회귀선은 x, y의 점들의 평균점을 지나야 함


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

$$
b_1 = \frac{\Sigma(x-\bar{x})(y-\bar{y})}{\Sigma(x-\bar{x})^2}
$$

따라서 여기서 b1은 6/10. 즉 기울기는 0.6

이제 b1을 알았으니 회귀식에 x와 y의 평균인 (3,4)을 대입해 b0을 구하면 된다.
$$
\hat{y} = b_0 + b_1x, \quad 4 = b_0 + 0.6*3
$$
따라서 b0 = 2.2

[^ ]: statquest

# Scikit-Learn으로 선형회귀 모델 만들기

## 1. Feature Engineering

```python
**특성을 고를 때 먼저 산점도를 그려봐서 선형관계 확인과 박스플롯과 describe로 이상치 확인 작업 먼저 할 것**
이상치 찾기


```

selectKBest



## 2. 데이터를 train, validation, test로 나누기

```python

```



## 3. 기준모델 만들기



## 4. 모델 인스턴스 만들기



## 5. 모델 학습



## 6. 예측



## 7. 계수와 절편



## 8. 모델 비교, 평가(R2 score, MAE)





최적화 하기: val에 fit하기? 아니면 train+val로 fit하기? 즉 마지막 test set 의 mae, r2구하기 전 어떻게 하는 거지?

```python
## Scikit-Learn에서 모델 import
from sklearn.linear_model import LinearRegression

#모델 인스턴스 만들기(instantiate model) #An instance is an exact replica of a model
model = LinearRegression()

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

```python
#산점도: X_train, y_train
plt.scatter(X_train, y_train, color = 'blue', linewidth = 1)

#X_test, y_pred
plt.scatter(X_test, y_pred, color = 'red', linewidth =1)
```



## 선형회귀모델의 계수(Coefficients)와 절편(Intercept)

**계수(coefficient)**


```python
#계수(coefficient): feature와 target의 관계, slope
model.coef_
```

**절편(intercept)**

```python
model.intercept_
```





# 2. 오류지표와 결정계수



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
fit을 알아보는데에 사용

R2이 1에 가까울수록 실제 값과 회귀선이 비슷하다. 더 좋은 fit이다.



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



bias and variance:

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

위와 같은 항목들을 진단하기 위해서는 산점도와 박스플롯 등 그래프를 그려봐야 한다. 



### 잔차그림: 오차의 등분산성 검정

X에 따른 잔차의 산점도를 x=X, y = e(y=y hat)로 그렸을 때, 어떠한 패턴도 보이고 있지 않다면 추정된 회귀식이 설명변수와 반응변수 간의 회귀모형을 적절하게 설명한 것이다. 

만약 x 값 증가할수록 잔차의 분산이 커지는 나팔관 형태라면 오차의 등분산 가정 위배(이분산)하고, 회귀식이 설명변수와 반응변수의 관계를 적절하게 설명하지 못하는 것이다.

비선형관계, 이차 내지 고차방정식 유형으로 보일 때는 선형관계보다는 다항회귀나 비선형 모형으로 가정하고 재분석해야 한다. 

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

오차에 대한 가정사항 진단: 잔차그림을 그림(x=적합값, y=잔차)



### 다중공선성 진단

1) 산점도행렬 또는 개별 X변수끼리의 산점도에서 거의 직선에 가까운 선형관계를 보임

2) X변수 하나를 뺄 때와 추가할 때 다른 X변수들의 기울기 추정값들이 많이 변동 

3)분산분석표에 의해서 유의적으로 기울기들이 모두 0이 아니지만 문제가 되는 X변수의 기울기는 비유의적일 때

4) 분산확대인수(variance inflation factor) 계산하거나 통계적검정



## 가변수

회귀분석의 독립변수에 질적변수가 포함되었을 때 사용

원핫인코딩 등으로 가변수로 변환해서 사용

```python
!pip install category_encoders


```

