---
layout: post
title: "[Machine Learning] 모델 선택"
date: 2021-02-10
category: [Machine Learning]
MachineLearning : true 
excerpt: "모델 평가와 모델 선택에서 사용하는 CV를 구분하기2"
tags: [CV, GridSearchCV, RandomSearchCV]
comments: true
---



# Cross Validation

개념이 헷갈렸는데...gridsearchcv나 randomsearchcv 같은 것은 하이퍼파라미터 튜닝 등 **모델 선택**에서 쓰이고, cross_val_score는 함수(매서드) 자체에 모델이 attribute로 들어가는 중첩 검증으로 **모델 평가**이다.

LogisticRegressionCV처럼 일부 모델은 그 모델에서 사용하는 하이퍼 파라미터를 알아서 찾아주는 CV도 있다. 



# 모델 선택

## 1. GridSearchCV(완전 탐색)

GreidSearchCV는 교차검증을 사용하여 모델을 선택하는 브루트포스한 방법이다. 사용자가 하이퍼 파라미터로 테스트 해보고 싶은 값들을 입력하면 GridSearchCV는 모든 값의 조합에 대해 모델을 훈련한다.

*브루트 포스(brute force): 키 전수 조사, 무차별 대입



```python
from sklearn.model_selection import GridSearchCV
from sklearn.impute import SimpleImputer
from category_encoders import OrdinalEncoder
from sklearn.pipeline import make_pipeline
from sklearn.ensemble import RandomForestClassifier


pipe1 = make_pipeline(
    OrdinalEncoder(), 
    SimpleImputer(), 
    RandomForestClassifier(random_state=2, class_weight={0:1,1:3}) #'balanced'로 해도 됨
)

dists = {
    'randomforestclassifier__n_estimators': [431, 432], 
    'randomforestclassifier__max_depth': [8, 9, None], 
    'randomforestclassifier__max_features': [0.6, 0.7, 0.8]
    
}

#그리드 서치 객체를 만든다
clf = GridSearchCV(
    pipe1, 
    param_grid=dists, 
    #n_iter=50, #gridsearchcv에서는 정해준 모든 값 대입해서 안 쓰는 파라미터
    cv=3, 
    scoring='f1',  
    verbose=1,
    n_jobs=-1
)


#그리드 서치 수행
best_model = clf.fit(X_train, y_train)
```



교차검증에서 제외한 폴드는 테스트 세트와 같은 역할을 하기 때문에 어떤 전처리도 해서는 안 된다. 따라서 교차검증 전에 전처리를 하면 안 되고, SearchCV안에 전처리를 넣어야 한다.  (위에서는 pipe1 안에 들어 있음)



**최선의 하이퍼파라미터 출력**

```python
print(best_model.best_params_)
print(best_model.best_score_)
```



**각 하이퍼 파라미터의 조합으로 만들어진 모델들 출력**

```python
pd.DataFrame(best_model.cv_results_).sort_values(by='rank_test_score').T
```



**타깃 벡터를 예측**

```python
y_pred = best_model.predict(X_test)
```



**모델의 중요 특성들 그래프로 보기**

```python
rf_pipe = pipe1.named_steps['randomforestclassifier']
importances_pipe = pd.Series(rf_pipe.feature_importances_, X_train.columns)

plt.figure(figsize=(10, 10))
plt.title(f'Top features')
importances_pipe.sort_values().plot.barh();
```



## 2. RandomizedSearchCV

```python
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import randint, uniform
import numpy as np
from sklearn.ensemble import RandomForestClassifier

pipe = make_pipeline(
    OrdinalEncoder(), 
    SimpleImputer(), 
    RandomForestClassifier(random_state=2)
)

#탐색할 하이퍼 파라미터의 범위 지정
dists = {
    #'simpleimputer__strategy': ['mean', 'median'],         # 최대한 시간 단축시키려 #달음...
    'randomforestclassifier__n_estimators': randint(300, 500), 
    'randomforestclassifier__max_depth': [8, 10, None], 
    'randomforestclassifier__max_features': uniform(0, 1), # max_features
    'randomforestclassifier__min_samples_split' : randint(1, 50), # 노드 분할에 사용할 최소한의 샘플 데이터 수
    'randomforestclassifier__min_samples_leaf' : randint(1, 100)   
}

#랜덤서치 객체를 만든다
clf = RandomizedSearchCV(
    pipe, 
    param_distributions=dists, 
    n_iter=50, 
    cv=5, 
    scoring= 'f1',  
    verbose=1,
    refit=True,
    n_jobs=-1,
    random_state =2
)

clf.fit(X_train, y_train);
```



**최선의 하이퍼파라미터 출력**

```python
print(best_model.best_params_)
print(best_model.best_score_)
#특정 하이퍼 파라미터만 보고 싶을 때
#print(best_model.best_estimator_.get_params()['randomforestClassifier_max_features'])
```



**각 하이퍼 파라미터의 조합으로 만들어진 모델들 출력**

```python
pd.DataFrame(best_model.cv_results_).sort_values(by='rank_test_score').T
```



**타깃 벡터를 예측**

```python
#이제 best_model 객체를 사용 가능
y_pred = best_model.predict(X_train) #훈련이 얼마나 잘 됐나 보려고..?
```



**모델의 중요 특성들 그래프로 보기**

```python
rf_pipe = pipe1.named_steps['randomforestclassifier']
importances_pipe = pd.Series(rf_pipe.feature_importances_, X_train.columns)

plt.figure(figsize=(10, 10))
plt.title(f'Top features')
importances_pipe.sort_values().plot.barh();
```



## 모델 선택 후 성능 평가

중복 교차검증(nested cross-validation)을 사용하여 편향된 평가를 피한다.

```python
from sklearn.model_selection  import cross_val_score

scores = cross_val_score(pipe, X_train, y_train, cv=5, 
                         scoring='f1')
```



**Reference**

파이썬을 활용한 머신러닝 쿡북 Chapter 11 모델 평가, 12 모델 선택

https://robjhyndman.com/hyndsight/crossvalidation/