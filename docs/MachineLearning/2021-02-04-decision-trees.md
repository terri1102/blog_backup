---
layout: post
title: "[Machine Learning] 의사결정트리 모델"
date: 2021-02-04
category: [Machine Learning]
MachineLearning : true 
excerpt: "의사결정트리 모델"
tags: [불순도, impurity, 지니지수, gini index,information gain]
comments: true
---



# Decision Trees(의사결정 트리)

[TOC]


## 의사결정트리 모델

의사결정 트리는 데이터를 학습하여 데이터 내에 존재하는 규칙을 찾아내고 이 규칙을 나무구조로 모형화해 분류와 예측을 수행하는 분석방법이다. 특성을 기반으로 샘플을 분류해 나가며, 질문이나 말단의 정답을 노드(마디, node)라 하고 노드를 연결하는 선을 엣지라고 한다. 

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fe/CART_tree_titanic_survivors_KOR.png/1024px-CART_tree_titanic_survivors_KOR.png" alt="결정트리" style="zoom: 50%;" />

[^ ]: 위키피디아: 결정트리



## 결정트리의 특징

numeric과 categorical variable에 모두 적용 가능하다.

yes/no data(답이 두 개)에 적용 가능하다. 즉, 한 노드에서 뻗어나는 가지는 항상 2개이며, 여러 답이 있을 경우 A와 ~A로 나눠서 사용 가능하다.

분류와 회귀문제 모두 적용 가능하며, 종속변수가 이산형일 땐 분류트리, 연속형일 땐 회귀 트리로 구분한다.

결정트리모델은 선형모델과 달리 비선형, 비단조(non-monotonic), 특성상호작용(feature interactions) 특징을 가지고 있는 데이터 분석에 용의하다.

트리 모델에서는 nominal variable을 ordinal로 encode해도 별다른 문제가 안 생긴다. 변수의 클래스 간 서열이 있든 없든 상관없이 분기가 되는지 안 되는지가 중요하기 때문이다.



## 가지의 분리 기준

### 불순도(impurity)
불순도란 랜덤하게 샘플을 분류할 때 틀릴 확률을 나타내는 지표이다.  불순도는 0과 1 사이의 값을 가지며, 높을 수록 샘플이 섞여있는 것을 의미한다. 불순도의 종류에는 대표적으로 지니 지수와 엔트로피 지수가 있다. 

[^ ]:statquest: 'Chance of being incorrect if you randomly assign a label to an example in the same set.'

참고로 불순도의 지니지수는 gini index고 경제학에서 소득 불평등을 나타내는 지표는 지니 계수(gini coefficient)다.

Scikit learn의 DecisionTreeClassifier의 dafault는 지니지수로 연산이 더 빠르기 때문에 그렇다고 한다. 엔트로피 지수도 많이 이용되는 듯하다.

$$
지니불순도 : {\displaystyle {I}_{G}(p)=\sum _{i=1}^{J}p_{i}(1-p_{i})=1-\sum _{i=1}^{J}{p_{i}}^{2}}
$$

$$
엔트로피 : {\displaystyle \mathrm {H} (T)=\operatorname {I} _{E}\left(p_{1},p_{2},...,p_{J}\right)=-\sum _{i=1}^{J}{p_{i}\log _{2}p_{i}}}$
$$



### Information gain(정보 획득)

특정한 특성을 사용해 분할했을 때 엔트로피의 감소량을 뜻한다. 

IG(T,a) = H(T) − H(T|a) 

분할전 노드 불순도 - 분할 후 자식노드'들'의 불순도

info gain= starting impurity - average impurity



### 분기 기준 

트리의 분기 기준은 information gain이 0이상일 때, 즉 불순도 낮아질 때이다. 즉, 부모 노드의 불순도>자식노드 불순도이며, 그렇게 만들어주는 특성 들 중 불순도의 감소가 최대인 것, 정보획득이 가장 큰 특성을 선택한다. 어느 노드에서 불순도가 부모 노드보다 높더라도 두 노드에서의 가중합이 감소하면 분기한다.



###  어떤 질문을 해야할까?

불순도 감소가 최대인 질문, 정보 획득이 큰 질문 



### 어떤 질문을 root node에 해야 할까?

뿌리마디(root node)에는 샘플을 가장 균일하게 나눠주는 특성이 선택되며, 올바른 분류를 위해선 상위 노드에서 하위 노드로 갈수록 집단 내에선 동질성이, 집단 간에는 이질성이 커져야 한다.



## 하이퍼 파라미터

하이퍼 파라미터를 튜닝해야 하는 이유는 트리 모델의 각 노드에서의 선택이 항상 전역적으로 최적의 선택인 최적의 트리는 아니기 때문이다.  결정트리 모델에는 특정 상태에서 최적을 찾는 그리디 알고리즘이 사용되며,  특성이 많아지면 컴퓨터도 전체적인 최적을 찾기 어려워지 때문에 하이퍼 파라미터를 튜닝해줘야 한다.



**가지치기 pruning?**

트리 생성시 가지 개수 제한 가능(하이퍼 파라미터): max-depth 설정 가능

과적합을 줄이기 위해 가지의 깊이를 제한한다.



## 의사결정 트리의 알고리즘

어떤 종류의 질문을 해야할까? 그 질문을 언제 해야할까? 를 결정하는 것



**1. 분류트리**



```python
#DecisionTreeClassifier
pipe = make_pipeline(
    OneHotEncoder(use_cat_names=True), 
    SimpleImputer(), 
    DecisionTreeClassifier(min_samples_leaf=10, random_state=2)
)    #노드에 최소한 존재해야 하는 샘플 수 정해줌->클수록 트리 깊이 얕아짐

pipe.fit(X_train, y_train)
print('훈련 정확도', pipe.score(X_train, y_train))
print('검증 정확도', pipe.score(X_val, y_val))
```

```python
dt_model = DecisionTreeClassifier(max_depth=6, random_state=2)
       #트리 깊이 제한

dt_model.fit(X_train, y_train)
print('훈련 정확도', pipe.score(X_train, y_train))
print('검증 정확도', pipe.score(X_val, y_val))
```



**2. 회귀트리**

1) 특성에 따라서 샘플을 오름차순으로 정렬한다.

2) 모든 인접한 샘플들의 평균을 구한다.

3) 샘플들 간 평균을 기준으로 불순도를 계산한다. (즉 평균이 구분 기준이 되는 셈)

4) 평균들의 불순도를 비교해서 가장 낮은 불순도를 갖는 평균이 cutoff 포인트가 된다.

```python
#DecisionTreeRegressior
```



**3. ranked data/여러 nominal data**

경우의 수를 나눠서 분류한다. 예를 들어 1,2,3,4 위가 있을 때 1보다 작거나 같은 그룹,  2보다 작거나 같은 그룹, 3보다 작거나 같은 그룹으로 나눠서 불순도를 계산한다. A일 떄, B일 때, C일 때, A or B일 때, A or C, B or C



## 의사결정 트리의 분류 지표와 알고리즘

CART: Classification and Regression Trees





## 트리 그리기

```python
import graphviz 
dot_data = tree.export_graphviz(clf, out_file=None) 
graph = graphviz.Source(dot_data) 
graph.render("iris")
```

<img src="https://scikit-learn.org/stable/_images/iris.png" alt="graphviz" style="zoom: 50%;" />

```python
#타겟의 분포를 보여주는 산점도
dot_data = tree.export_graphviz(clf, out_file=None, 
                     feature_names=iris.feature_names,  
                     class_names=iris.target_names,  
                     filled=True, rounded=True,  
                     special_characters=True)  
graph = graphviz.Source(dot_data)  
graph 
```



### 중요 특성들 그래프로 알아보기

중요 특성으로 나열할 때 같은 특성이 여러 번 질문으로 쓰인 경우 이를 모두 합친 값을 기준으로 나열한다. 즉, 샘플들을 분류하는데 중요한 특성들은 여러번 질문으로 쓰일 수 있으며, 이렇게 여러 번 쓰인 질문은 주요 특성으로 랭크된다. 

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



##### Reference

https://scikit-learn.org/stable/modules/tree.html

https://colab.research.google.com/github/random-forests/tutorials/blob/master/decision_tree.ipynb#scrollTo=vrxZN08SgNiW