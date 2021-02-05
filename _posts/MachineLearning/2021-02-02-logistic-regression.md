---
layout: post
title: "Logistic Regression"
date: 2021-02-02
category: [Machine Learning]
MachineLearning : true 
excerpt: "Logistic Regression and scaling"
tags: [logistic Regression, validation set, scaling, standard scaler, minmax scaler]
comments: true
---



# Logistic Regression



**로지스틱회귀와 선형회귀의 차이**

1) 로지스틱회귀는 복잡한 모델을 단순한 모델과 바로 비교할 수 없다. 대신 변수가 예측에 영향을 미치는 것이 0보다 아주 큰지만 알 수 있음 (Wald's Test)

2) 선이 데이터에 어떻게 fit되는지 다름. 선형회귀는 least squares 방법. 로지스틱회귀는 maximum likelihood

*Maximum likelihood: maximum likelihood이 select됨

x_i일 때 y_i가 1일 확률을 구함, 이것으로 y_i가 0일 때 x_i인 likelihood 구함. 이를 모든 x값에 대해서 하고, 이 모든 likelihood들을 곱함->그게 이 전체 데이터의 likelihood임, 

라인을 바꿔보며 likelihood를 여러 개 구해보며 최대값을 찾음

linear regression: y값이 0과 1이어서 타겟 변수 값이 1인지 0인지 알기 어려움



공통점: continuous data와 discrete data 모두 설명변수로 사용가능



로지스틱 회귀:

classification에 쓰임

y변수는 0,1의 값만 가짐. X변수는 continuous, discrete data 모두 사용 가능

그래프에서 y축은 0~1까지의 구간인데, x_i가 1일 확률을 나타냄

어떤 변수가 classifying하는데 유용한지 알려줌

---

logistic regression details

pt1. coefficients



### logistic regression

```python
from sklearn.linear_model import LogisticRegression

logistic = LogisticRegression()
logistic.fit(X_train, y_train)

print('훈련세트 정확도', logistic.score(X_train, y_train))
```

```python
y_pred = model.predict(X_train_scaled)
accuracy_score(y_train, y_pred)
```



상관계수의 의미



Scaler

https://scikit-learn.org/stable/auto_examples/preprocessing/plot_all_scaling.html#sphx-glr-auto-examples-preprocessing-plot-all-scaling-py



만약 y의 클래스가 3개 이상이라면?

https://www.shuhanyu.com/2018/07/12/LogisticRegressionInMultiClassClassificationProblems/





## 검증 세트(Validation set)

Validation vs Testing

Validation: allowing redemption. 모의고사

Testing: 진짜 테스트, 이 모델이 적합한지 평가

검증데이터의 용도는 지금 내가 설계한 모델이 잘 작동하는지 확인 후 fine tuning 작업을 위해 필요함

테스트 데이터는 마지막에 단 한번의 평가로 모델이 적합한지 평가



data set is divided to training, validation, testing sets

**Training set:** construct classifier

**Validation set:** pick algorithm + knob settings(more rigid, flexible 하게 바꾸던가)

* pick best-performing algorithm
* fine-tune knobs(tree depth, k in kNN, c in SVM)

Testing set: estimate future error rate 

* never report best of many runs
* run only once, or report results of every run

split randomly to avoid bias

테스트 데이터로 예측한 후 모델을 수정해 또 테스트 데이터로 예측하는 것을 피해야 하는 이유는

테스트 데이터로 튜닝됐기 때문에 이 모델이 새로운 데이터를 잘 예측해줄지 판단할 수 없음



시계열 데이터를 훈련/검증/테스트 세트로 나눌 때 주의할 점

시계열 데이터를 순서대로 훈련, 검증, 테스트로 나눠야 함



**오즈와 확률의 차이**



오즈: 사건이 발생/ 사건이 발생 않음
$$
odds(사건\;발생) = \frac{Pr(사건 \;발생)}{Pr(사건 \;발생 \;안함)} \; = \; \frac{Pr(사건\;발생)}{1-Pr(사건 \;발생)}
$$


확률: 사건이 발생/ 전체 사건(사건 발생+사건 발생 않음)
$$
Probability(사건\;발생) = \frac{Pr(사건\;발생)}{1}
$$
P(사건 발생)으로 odds(사건 발생) 구할 수 있음

odds = p/(1-p)



**오즈비**: 두 오즈를 비율로 계산한 것 



**로그 오즈**

odds for our team winning: 1~infinity

odds against our team losing: 0~1

로그를 취하면 이 둘의 scale 차이를 없애줘서 대칭으로 만들어줌

ex) odds = 1/6 = 0.17 -> log(odds) = log(0.17) = -1.79

​      odds = 6/1 = 6 -> log(odds) = log(6) = 1.79

log(odds)= logit = log(p/(1-p))



**로그 오즈**를 쓰는 이유: 정규분포를 따르게 만들어서 yes/no 같은 이산형 문제를 해결하는데 도움이 된다. 로지스틱 회귀모델에서 회귀계수는 로그 오즈가 사용된다.

[^ ]: StatQuest: Odds and Log(Odds), Clearly Explained!!!



로짓변환을 하는 이유: 회귀분석의 반응변수의 범위는 -무한대, +무한대 인 것에 비해 로지스틱 회귀분석의 오즈 범위는 (0, 무한대)이기 때문에 오즈의 범위를 회귀분석과 동일한 (-무한대, +무한대)로 변환하여 회귀분석처럼 해석할 수 있게 된다.





[사진]



[사진]

## 로지스틱 회귀모델에서의 회귀계수

로지스틱 회귀모델에서 OLS를 쓰지 못하는 이유는 OLS의 범위가 무한대이기 때문이다. 

ss(mean)대신 ll(log(odds))를 구한다.(가로선 나옴) 각 샘플을 이 선에 project한다. 그리고 로그오즈를 다시 확률로 바꾼다. 

확률 = 로그오즈/(1+로그오즈)

로그 오즈를 구한다.