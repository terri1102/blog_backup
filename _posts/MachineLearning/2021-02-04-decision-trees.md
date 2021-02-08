---
layout: post
title: "[Machine Learning]의사결정트리 모델"
date: 2021-02-04
category: [Machine Learning]
MachineLearning : true 
excerpt: "의사결정트리 모델"
tags: [불순도, impurity, 지니지수, gini index,information gain]
comments: true
---



# Decision Trees(의사결정 트리)
대답은 numeric, category 다 가능



어떤 질문을 root node에 해야 할까?

샘플을 가장 균일하게 나눠주는 질문으로



impure: leaf node가 하나의 정보만 담고 있지 않을 때(100% heart disease or vice versa)

불순도(impurity) 



1.yes/no data(답이 두 개)

2.Numerica and continuous variable

1) 특성에 따라서 샘플을 오름차순으로 정렬

2)모든 인접한 샘플들의 평균을 구한다

3) 샘플들 간 평균을 기준으로 불순도를 계산한다. (즉 평균이 구분 기준이 되는 셈)

4) 평균들의 불순도를 비교해서 가장 낮은 불순도를 갖는 평균이 cutoff 포인트가 됨

3.ranked data/여러 nominal data

경우의 수를 나눈다. 예를 들어 1,2,3,4 위가 있을 때 1)1보다 작거나 같은 그룹, 2)2보다 작거나 같음 3)3보다 작거나 같음 으로 나눠서 불순도를 계산

A일 떄, B일 때, C일 때, A or B일 때, A or C, B or C



Decision Tree의 분류 알고리즘

CART: Classification and Regression Trees



어떤 질문을 해야할까?

Gini impurity가 낮은 질문

information gain을 알 수 있음 



어떤 종류의 질문을 해야할까? 

그럼 어떤 질문을 언제 해야할까?



불순도: chance of being incorrect if you randomly assign a label to an example in the same set

gini impurity: 0~1 사이의 값 가짐. 높을 수록 샘플이 섞여있는 거, uncertainty 의미



information gain: find the best question to ask

info gain= starting impurity - average impurity



분할점: 불순도 낮아지는 특성 선택, 부모 노드의 불순도>자식노드 불순도

information gain이 0이상일 떄

scikit learn dafault는 지니계수 많이 씀(더 빠름)



가지치기 pruning?

트리 생성시 가지 개수 제한 가능(하이퍼 파라미터): 설정 가능



각 노드에서의 선택이 항상 전역적으로 최적의 선택인 최적의 트리는 아니다. 특성이 많아지면 컴퓨터도 풀기 어려워짐

그리디 알고리즘: 그 상태에서 최적을 찾는 것



트리 모델에서는 nominal을 ordinal로 encode해도 별 문제 안 생김



과적합을 줄이기 위해 가지치기



어느 노드에서 불순도가 엄마 노드보다 높더라도 가중합이 감소하면 분기한다.

https://colab.research.google.com/github/random-forests/tutorials/blob/master/decision_tree.ipynb#scrollTo=vrxZN08SgNiW



중요 특성들 그래프로 알아보기

```python
import matplotlib.pyplot as plt
from sklearn.pipeline import make_pipeline

#name_steps: 유사 딕셔너리 객체, 파이프라인 내 과정에 접근하게 해줌
pipe.name_steps #파이프라인에 들어간 함수들 출력해줌

model = pipe.named_steps['logisticregression']
enc = pipe.named_steps['onehotencoder']
encoded_columns = enc.transform(X_val).columns
coefficients = pd.Series(model_lr.coef_[0], encoded_columns)
plt.figure(figsize=(10,30))
coefficients.sort_values().plot.barh();
```

