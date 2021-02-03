---
layout: post
title: "정규화 선형회귀"
date: 2021-02-01
category: [Machine Learning]
MachineLearning : true 
excerpt: "Ridge Regression, Lasso Regression, Elastic Net Regression"
tags: [SelectKBest, Ridge, Lasso, Ealstic, lambda]
comments: true
---



# Regularized Regression



# Regularized Regression(정규화 선형회귀)

정규화 선형회귀 방법은 선형회귀 계수에 대한 제약 조건을 추가해서 bias를 높이고, variance를 줄이는 방식으로 과적합을 막는 방법이다. 여기서는 Ridge regression, Lasso regression, Elastic net에 대해 정리하겠다.



# Ridge Regression



## Feature selection

**SelectKBest**

```python
## f_regresison, SelectKBest
from sklearn.feature_selection import f_regression, SelectKBest       #f_regression은 selectKbest 안에서 쓰는 거, regression 모델 쓸 때 사용 <->f_classification

## selctor 정의합니다.
selector = SelectKBest(score_func=f_regression, k=10)

## 학습데이터에 fit_transform 
X_train_selected = selector.fit_transform(X_train, y_train)          #label 넣어줌, label과 feature의 상관관계를 계산하기 때문

## 테스트 데이터는 transform
X_test_selected = selector.transform(X_test)                         #fit은 y_test를 attribute로 갖지 않음


X_train_selected.shape, X_test_selected.shape    
```

```python
# target(Price)와 가장 correlated 된 features 를 k개 고르는 것이 목표입니다.

## f_regresison, SelectKBest
from sklearn.feature_selection import f_regression, SelectKBest       #f_regression은 selectKbest 안에서 쓰는 거, regression 모델 쓸 때 사용 <->f_classification

## selctor 정의합니다.
selector = SelectKBest(score_func=f_regression, k=10)

## 학습데이터에 fit_transform 
X_train_selected = selector.fit_transform(X_train, y_train)          #label 넣어줌, label과 feature의 상관관계를 계산하기 때문

## 테스트 데이터는 transform
X_test_selected = selector.transform(X_test)                         #fit은 y_test를 attribute로 갖지 않음


X_train_selected.shape, X_test_selected.shape    
```



Ridge 회귀를 사용하는 이유는 무엇일까요? Ridge 회귀는 **과적합을 줄이기 위해서** 사용하는 것입니다. 과적합을 줄이는 간단한 방법 중 한 가지는 **모델의 복잡도를 줄이는 방법**입니다. 특성의 갯수를 줄이거나 모델을 단순한 모양으로 적합하는 것입니다.

앞서 우리는 모델학습에 있어서 편향(bias)과 분산(variance)의 영향에 대해 배웠습니다. Ridge 회귀는 이 **편향을 조금 더하고, 분산을 줄이는 방법**으로 정규화(Regularization)를 수행합니다. 여기서 말하는 정규화는 모델을 변형하여 과적합을 완화해 일반화 성능을 높여주기 위한 기법을 말합니다.



Regularization : desensitization

Regularization pt1: Ridge regression

ols: overfit & high variance

Ridge Regression: ols보다 bias를 좀 높이고, variance를 낮추는 방법, 일반화가 더 잘 된다
$$
the\; sum\; of\; the\; squared\; residuals + \lambda*Slope^2
$$
를 최소화하는 선 찾기



lambda: 사용자가 정해야 하는 parameter, hyper parameter

일반 parameter: 학습과정에서 스스로 알려줌



**변수가 많을 때**


$$
\beta_0 + slope1*X1 +slope2*X2+...+slopen*Xn = y
$$
일 때,
$$
the\;sum\;of\;the\;squared\;residuals +\lambda*(slope1^2+slope2^2+...+slopen^2)
$$
를 최소화하는 선을 찾는다.





Ridge regression: less fit, less variance

the sum of the squared residuals를 최소화하는 ols와 다르게 ridge regression은  sum of the squared residuals와 lambda*the slope^2를 최소화함

slope2으로 패널티를 주고, lambda는 얼마나 패널티를 줄지 결정함



패널티 

기울기가 가팔라지면 y는 x에 대해 sensitive함

기울기가 완만해지면 y는 x에 대해 덜 민감함

ridge regression: less sensitive to x



lambda?

0~infinity

람다가 커질수록 기울기는 완만해진다. 람다가 커질수록 y는 x에 덜 민감해짐

람다는 lowest variance를 내는 값, 10 fold cross validation으로 구함



#discrete variable에도 work함



샘플 수가 파라미터 수보다 적어도 ridge regression 사용 가능함

ridge regression penalty를 통해서 가능함

When the sample size are relatively small, then Ridge Regression can improve predictins made from new data(i.e. reduce Variance) by making the predictions less sensitive to the Training Data

```python
from sklearn.metrics import r2_score

for alpha in [0.001, 0.005, 0.01, 0.02, 0.03, 0.1, 0.15, 0.17, 0.2, 100.0, 1000.0]:
        
    print(f'Ridge Regression, alpha={alpha}')

    # Ridge 모델 학습
    model = Ridge(alpha=alpha, normalize=True)  
    model.fit(X_train, y_train)
    y_pred = model.predict(X_test)

    # MAE for test
    mae = mean_absolute_error(y_test, y_pred)
    r2 = r2_score(y_test, y_pred)
    
    print(f'Test MAE: ${mae:,.0f}')
    print(f'Test R2: {r2:,.3f}')
    
    # plot coefficients
    coefficients = pd.Series(model.coef_, X_train.columns)
    plt.figure(figsize=(10,3))
    coefficients.sort_values().plot.barh()
    plt.show()

    #람다값이 올라갈수록 회귀계수가 줄어든다. 작은 값들은 더 작아지고, 중요값만 남게 됨->어느정도 ridge가 학습을 하면서 feature selection 역할을 한다고 할 수 있음
    #람다 너무 높으면 mae 높아짐
    #선형회귀: 특성들이 서로 영향을 안 준다는 조건, 영향 주면 다중공선성 문제 생김. 
    #릿지회귀: 중요 특성만을 뽑아냄으로서 다중공선성 문제를 약간? 해결하게 됨
    #다중공선성은 다음에 더 다루게 됨

    #지금은 람다가 0.1~1일 때 가장 오류 적은 것 알 수 있음
    #릿지cv가 정확하게 찾아줌

```



# Cross validation

어떤 머신러닝 방법이 가장 잘 맞을지 결정

1)Estimate the parameters for the machine learning methods

*estimating parameters: training the algorithm

2)Evaluate how well the machine learning methods work

*evaluating a method: testing the algorithm

이때 데이터를 10개로 나눈 후에 한번의 시행 당 9개 블럭은 train data로 나머지 한 블럭은 test data로 시행해서 (총 10번 시행) 그 결과를 보여주는 것이 10 fold cross validation

Ridge Regression에서 lambda값을 정하는 데 사용됨

```python
from sklearn.linear_model import RidgeCV

RidgeCV(alphas=0.1, 1.0, 10.0, *, fit_intercept=True, normalize=False, scoring=None, cv=None, gcv_mode=None, store_cv_values=False, alpha_per_target=False)
```









#정규화 방법 중 하나인 라쏘, 엘라스틱넷 찾아보기

엘라스틱넷은 라쏘랑 릿지를 합친 것

