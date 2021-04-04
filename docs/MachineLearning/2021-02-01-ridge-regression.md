---
layout: default
title: "[지도학습] 정규화 선형회귀"
date: 2021-02-01
category: [Machine Learning]
parent: Machine Learning
nav_order: 5
tags: [SelectKBest, Ridge, Lasso, Ealstic, lambda]
comments: true
---



# Regularized Regression

![regularization1](https://github.com/terri1102/terri1102.github.io/blob/master/assets/img/regularization1.png?raw=true)

# Regularized Regression(정규화 선형회귀)

Regularization : desensitization

정규화 선형회귀 방법은 선형회귀 계수에 대한 제약 조건을 추가해서 bias를 높이고, variance를 줄이는 방식으로 과적합을 막아 모델의 일반화 성능을 높여주는 방법이다. 또한, 특성이 너무 많을 때, 이를 줄이는 데 도움을 주기도 한다.(Feature explosion can be limited via techniques such as: regularization, kernel method, feature selection.) 여기서는 Ridge regression, Lasso regression, Elastic net에 대해 정리하겠다.



## Feature selection

https://scikit-learn.org/stable/modules/feature_selection.html

**SelectKBest**

```python
# target(Price)와 가장 correlated 된 features 를 k개 고르는 것이 목표

# f_regresison, SelectKBest
from sklearn.feature_selection import f_regression, SelectKBest       #f_regression은 selectKbest 안에서 쓰는 거, regression 모델 쓸 때 사용 <->f_classification

## selector 정의
selector = SelectKBest(score_func=f_regression, k=10)

## 학습데이터에 fit_transform 
X_train_selected = selector.fit_transform(X_train, y_train)          #label 넣어줌, label과 feature의 상관관계를 계산하기 때문

## 테스트 데이터는 transform
X_test_selected = selector.transform(X_test)                         #fit은 y_test를 attribute로 갖지 않음


X_train_selected.shape, X_test_selected.shape    
```

**SelectKBest로 선택된 특성 보기**

```python
all_names = X_train.columns

## selector.get_support()
selected_mask = selector.get_support()            #그냥 selector.get_support()를 쓰면 boolean 값 나온다 함

## 선택된 특성들
selected_names = all_names[selected_mask]

## 선택되지 않은 특성들
unselected_names = all_names[~selected_mask] 

print('Selected names: ', selected_names)
print('Unselected names: ', unselected_names)
```



# Ridge Regression

**Ridge 회귀를 사용하는 이유:**  과적합을 줄이기 위해서 & Feature Selection (모델 복잡도 줄이기)

**OLS와 비교**하자면 RSS를 최소화하는 ols와 다르게 ridge regression은  RSS + lambda * the slope^2를 최소화한다. Ridge 회귀는 ols보다 bias를 좀 높이고, variance를 낮추는 방법으로 모델 일반화가 더 잘 된다. 그리고 OLS보다 이상치의 영향을 덜 받는다.



### Ridge Regression의 비용함수



$$
\beta_{ridge}:  argmin[\sum_{i=1}^n(y_i - \beta_0 - \beta_1x_{i1}-\dotsc-\beta_px_{ip})^2 + \lambda\sum_{j=1}^p\beta_j^2]
$$



즉,  Ridge Regression의 비용함수는

$$
RSS(the\; sum\; of\; the\; squared\; residuals) + \lambda*Slope^2
$$
를 최소화하는 선 찾는 것이다.



여기서 **lambda**는 사용자가 정해야 하는 parameter로 hyper parameter이다.  lambda는 OLS의 기울기를 조절해서 분산을 줄이는 패널티 값이다. alpha로 나타내며 alpha=0이면 OLS와 동일하고, 기울기가 완만해지면(alpha값이 커지면 회귀계수가 작아짐.  회귀식이 2차일 땐 기울기로 볼 수 있음) y는 X에 대해 덜 민감해진다. 최적의 alpha값은 RidgeCV로 구한다.



### RidgeCV

```python
from sklearn.linear_model import RidgeCV                                                 #cv 순차적으로 나눔, 모든 데이터를 train에 사용할 수 있다는 장점이 있음

alphas = [0.01, 0.05, 0.1, 0.2, 1.0, 10.0, 100.0]  #미리 지정해줘야 함, range 함수로 줘도 됨, hyper parameter tuning 사람이 직접 지정해줘야 하는 파라미터

ridge = RidgeCV(alphas=alphas, normalize=True, cv=3) #cv=3: 데이터를 세 가지로 나눔, 데이터가 충분하면 cv값을 더 높여도 됨
ridge.fit(X_train, y_train)
print("alpha: ", ridge.alpha_)
print("best score: ", ridge.best_score_)
```



람다 외의 일반 parameter는 학습과정에서 스스로 결정된다.



**변수가 많을 때의 경우**



회귀선이

$$
y = \beta_0 + slope_1*X1 +slope_2*X2+...+slope_n*Xn
$$
일 때,
$$
the\;sum\;of\;the\;squared\;residuals +\lambda*(slope_1^2+slope_2^2+...+slope_n^2)
$$
를 최소화하는 선을 찾는다.



### Ridge Regression의 특성

이산형 변수에도 사용가능하다. 샘플 수가 파라미터 수보다 적어도 penalty를 통해서 분산이 줄어드니까 ridge regression 사용 가능하다.



### Sklearn으로 Ridge regression

#### alpha 값 모를 때

```python
from sklearn.metrics import r2_score
from sklearn.linear_model import Ridge


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
    #alpha는 릿지cv가 정확하게 찾아줌

```



#### RidgeCV로 최적의 alpha를 구했을 때

```python
#OLS랑 과정 동일함
from sklearn.metrics import r2_score, mean_absolute_error
from sklearn.linear_model import Ridge

#Ridge 모델 객체 생성
model = Ridge(alpha=alpha, normalize=True)   #여기에 최적의 alpha 넣으면 됨
#RidgeCV(alphas=0.1, 1.0, 10.0, *, fit_intercept=True, normalize=False, scoring=None, cv=None, gcv_mode=None, store_cv_values=False, alpha_per_target=False)


# Ridge 모델 학습
model.fit(X_train, y_train)

#학습한 모델로 예측
y_pred = model.predict(X_test)

#평가지표 : mae, r2
mae = mean_absolute_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
    
print(f'Test MAE: ${mae:,.0f}')
print(f'Test R2: {r2:,.3f}')

#특성별 회귀계수
coefs = model.coef_
print(f'Number of Features: {len(coefs)}')

#CV best score
print(f'cv best score: {model.best_score_}')
```



특성별 회귀계수를 통해 feature selection이 되었다고 볼 수 있다.

```python
coefs.sort()
plt.plot(coefs)
```



#### Cross validation

1) Estimate the parameters for the machine learning methods

*estimating parameters: training the algorithm

2) Evaluate how well the machine learning methods work

*evaluating a method: testing the algorithm





# Lasso Regression

Ridge regression 모델의 slope에 제곱 대신 절댓값을 씌우면 **라쏘 정규화 모델**이 된다. 

**릿지 회귀와 비교:** 릿지는 각 상관계수들이 타겟을 맞추는 데 중요하기에 약한 영향 주는 설명변수도 아예 없애진 않는다.  즉,  각 특성의 회귀 계수는 0에 가까운 값을 가질 수 있지만 0은 되지 못한다. 릿지는 많은 특성이 타겟에 영향을 줄 때 사용한다.

**라쏘**는 일부 특정한 특성들이 타겟에 강하게 영향을 줄 때 더 적절하다. 릿지와 다르게 기울기를 0으로 만들 수 있다. 특성 앞에 회귀계수가 0이 되면 아예 없애주기에 완벽한 특성 선택효과를 가진다. 계산시 절댓값은 미분 안 되는 구간 있어서 컴퓨터가 알아서 잘 구함


$$
RSS + \lambda * (|slope|)
$$

```python
from sklearn.linear_model import Lasso

model = Lasso(alpha=1.0, *, fit_intercept=True, normalize=False, precompute=False, copy_X=True, max_iter=1000, tol=0.0001, warm_start=False, positive=False, random_state=None, selection='cyclic')
```





# Elastic Net

어떤 거 써야 할지 모를 떄는 엘리스틱넷을 쓴다.  엘라스틱넷은 라쏘랑 릿지를 합친 것이다. 그래도 Elastic Net이 항상 가장 좋은 건 아니고 데이터 특징에 따라 가장 잘 맞는 모델 있음
$$
RSS + \lambda_1 * (|variable_1|+...+|variable_n|) + \lambda_2 * (variable_1^2 + ...+variable_n^2)
$$
Ridge와 Lasso 각각 Lambda를 갖게 되고, CV를 통해서 두 Lambda의 조합을 찾게 됨



### Elastic-Net 회귀의 장점

파라미터들 간에 상관관계가 있을 때 더 적합하다. by combining Lasso and Ridge regression, Elastic-NEt Regression groups and shrinks the parameters associated with the correlated variables and leaves them in equation or removes them all at once.



### ElasticNetCV

```python
from sklearn.linear_model import ElasticNetCV

ecv = ElasticNetCV(*, l1_ratio=0.5, eps=0.001, n_alphas=100, alphas=None, fit_intercept=True, normalize=False, precompute='auto', max_iter=1000, tol=0.0001, cv=None, copy_X=True, verbose=0, n_jobs=None, positive=False, random_state=None, selection='cyclic')

ecv.fit(X,y)
```



### ElasticNet

```python
from sklearn.linear_model import ElasticNet

model = ElasticNet(alpha=1.0, *, l1_ratio=0.5, fit_intercept=True, normalize=False, precompute=False, max_iter=1000, copy_X=True, tol=0.0001, warm_start=False, positive=False, random_state=None, selection='cyclic')

#l1: lasso penalty
#l2: ridge penalty
```





## References

https://youtu.be/NGf0voTMlcs

https://youtu.be/1dKRdX9bfIo