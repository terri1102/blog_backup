---
layout: post
title: "검증세트, Scaling, SelectKBest"
date: 2021-02-02
category: [Machine Learning]
MachineLearning : true 
excerpt: "검증세트, Scaling, SelectKBest"
tags: [validation set, scaling, standard scaler, minmax scaler]
comments: true
---



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

