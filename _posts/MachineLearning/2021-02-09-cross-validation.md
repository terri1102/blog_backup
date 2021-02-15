---
layout: post
title: "[Machine Learning] Cross Validation"
date: 2021-02-09
category: [Machine Learning]
MachineLearning : true 
excerpt: "교차검증을 하는 이유와 방법 이해"
tags: [CV, RandomSearchCV, GridSearchCV]
comments: true
---





# Cross Validation

여러 다른 머신러닝 모델을 비교하고 성능을 평가할 수 있게 해주고 하이퍼 파리미터 튜닝도 도와준다.



## 1. 교차검증 모델 만들기

:실전에서 모델이 얼마나 잘 작동할지 평가할 수 있다.

```python
from sklearn import datasets
from sklearn import metrics
from sklearn.model_selection import KFold, cross_val_score
from sklearn.pipeline import make_pipeline
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StatndardScaler
from category_encoders import OrdinalEncoder
from sklearn.impute import SimpleImputer
from sklearn.feature_selection import SelectKBest, f_classif
```

```python
digits = datasets.load_digits()

features = digits.data  #특성 행렬

target = digits.target  #타겟 벡터

scaler = StandardScaler() 

logit = LogisticRegression()  #모델 인스턴스(객체)

pipe = make_pipeline(OrdinalEncoder(),
                     SimpleImputer(),
                     scaler,
                     SelectKBest(f_classif, k = 20)
                     logit) 

kf = KFold(n_splits = 10, shuffle=True, random_state = 2)  #내 컴퓨터를 생각한다면 최대 5로 하자...

cv_results = cross_val_score(pipe,         #파이프라인
                            features,      #특성 행렬
                            target,        #타겟 벡터
                            cv=kf          #교차 검증
                            scoring = 'f1' #평가 지표
                            n_jobs=-1)     #모든 CPU 코어 사용
cv_results.mean()      #그냥 cv_results하면 fold가 10개니까 값이 10개 나온다
```



**K-Fold CV의 장점**

Hold-out 기법의 단점인

1) 모델 성능은 테스트 세트로 나뉜 일부 샘플에 의해 결정되는 것

2) 전체 가용 데이터를 사용하여 모델을 훈련하고 테스트하지 못하는 것(hold-out은 샘플 데이터가 적을 때 충분히 훈련을 못 시킨다)

을 극복하게 해준다.



**K-Fold CV를 수행시 고려할 점**

1) KFCV는 각 샘플이 다른 샘플과 독립적으로 생성되었다고 가정한다. (데이터는 독립동일분포: IID) 데이터가 IID라면 폴드를 나누기 전에 샘플을 섞어야 한다. (shuffle=True) 

2) K-Fold CV를 사용해 classifier를 평가할 때 각 타긱 클래스의 샘플이 같은 비율로 폴드에 담기는 것이 좋다. (KFold의 클래스를 StratifiedKFold로 바꾸면 된다.)

3) 검증 세트나 교차검증을 사용시 훈련세트에서 데이터를 전처리하고 이 변환을 훈련 세트, 테스트 세트에 모두 적용하기(훈련 데이터엔 fit_transform, 테스트 데이터엔 transform)



LeaveOneOutCV: n_splits = n인 K-Fold라고 할 수 있다.



**ShuffleSplit**

반복횟수에 상관없이 훈련 폴드와 테스트 폴드 크기를 임의로 지정할 수 있다. 반복마다 복원추출하기 때문에 샘플이 여러번 테스트 폴드에 포함될 수 있다.

```python
from sklearn.model_selection import ShuffleSplit

ss = ShuffleSplit(n_split=10, train_size = 0.5, test_size=0.2, random_state = 2)

cv_results = cross_val_score(pipe, features, target, cv=ss, scoring = 'accuracy', n_jobs=-1)
```

RepeatedKFold: 교차검증을 반복하여 실행

```python
from sklearn.model_selection import RepeatedKFold

rfk = RepeatedKFold(n_splits = 10, n_repeats = 5, random_state = 2) #50개의 교차검증 개수 생김

cv_results = cross_val_score(pipe, features, target, cv=rfk, scoring='accuracy',n_jobs=-1)


```



## 2. 기본 회귀 모델

**DummyRegressor 사용한 기본 모델**: 우리가 만들 모델과 비교할만한 baseline을 제공해준다

```python
from sklearn.dummy import DummyRegressor

dummy = DummyRegressor(strategy='mean')
#clf = DummyRegressor(strategy='constant', constant = 20)  #모든 샘플 20으로 예상하는 모델

dummy.fit(X_train, y_train)

dummy.score(X_test, y_test) #r2 점수
```



## 3. 기본 분류 모델 

**DummyClassifier**

```python
from sklearn.datasets import load_iris
from sklearn.dummy import DummyClassifier
from sklearn.model_selection import train_test_split

iris = load_iris()
X_train, y_train = iris.data, iris.target #iris.data는 feature_names 없이 타겟이 분리된 값이다.

X_train, X_test, y_train, y_test = train_test_split(X_train, y_train, random_state = 0)

#기본 분류 모델
dummy = DummyClassifier(strategy='uniform', random_state = 0) #uniform: 클래스 비중이 균등하도록 랜덤하게 예측(1과 0을 50% 비중으로 예측)

#'most_frequent' : 가장 많은 값으로 예측
dummy.fit(X_train, y_train)

dummy.score(X_test, y_test)

```

**진짜 만들고 싶은 모델 만들어서 Dummy 모델과 비교**

```python
clf = RandomForestClassifier()

clf.fit(X_train, y_train)

clf.score(X_test, y_test)
```



## 4. 이진 분류기의 성능 평가

**cross_val_score**

```python
#cross_val_score : 정확도(accuracy), 정밀도(precision), 재현율(recall), f1 가능
from sklearn.metrics import cross_val_score

model = RandomForestClassifier()

cross_val_score(model, X, y, scoring= 'accuracy') #cv 매개변수를 지정하지 않으면 회귀일 때는 KFold, 분류일 때는 StratifiedFold 분할기 사용됨
```

```python
#실제 타겟 값과 예측 값을 안다면 직접 계산 가능
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

accuracy_score(y_test,y_pred) #위에 cross_val_score랑 들어가는 값이 다름
```

**cross_validate**: scoring의 매개변수에 여러 평가 지표 추가 가능

```python
from sklearn.model_selection import cross_validate

cross_validate(model, X, y, scoring=['accuracy','f1'])
```



## 5. 이진 분류기 임곗값 평가

ROC(Receiving Operating Characteristic) curve는 이진분류기의 품질을 평가하는 데 널리 쓰이는 방법이다.

ROC 곡선은 확률 임곗값마다 진짜 양성과 거짓 양성 개수를 비교한다. 랜덤하게 예측하는 분류기는 대각선으로 나타난다.

```python
import matplotlib.pyplot as plt
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import roc_curve, roc_auc_score
from sklearn.model_selection import train_test_selection

X_train, X_test, y_train, y_test = train_test_split(X_train, y_train, test_size = 0.2, random_state =2)

logit = LogisticRegression() #모델 객체 생성
logit.fit(X_train, y_train)  #모델 학습

y_proba = logit.predict_proba(y_test)[:.1]

false_positive_rate, true_positive_rate, threshold = roc_curve(y_test, y_probabilities)

#ROC 곡선
plt.title('ROC curve')
plt.plot(false_positive_rate, true_positive_rate)
plt.plot([0,1], ls = '--')
plt.plot([0,0],[1,0], c='.7'), plt.plot([1,1], c='.7')
plt.ylabel('True Positive Rate')
plt.xlabel('False Positive Rate')
plt.show()
```

```python
#예측 확률 계산
logit.predict_proba(X_test)[0:1] #첫 번째 샘플에 대한 예측 확률
#>>array([[0.777, 0.223]])

#class 확인
logit.classes_
#>>array([0,1])   #즉 0일 확률이 0.777
```

임곗값을 조절해서 모델을 편향되게 만들어야 할 때: 거짓 양성이 큰 비용을 치르게 한다면 확률 임곗값을 높여야 한다. 일부 양성 샘플을 예측하지 못하더라도 예측한 샘플은 양성이라고 강하게 확신할 수 있기 때문                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             

우리가 수집한 타겟 데이터 컬럼

1) train the machine learning algorithm : 머신러닝 알고리즘을 학습시킨다.  estimate the parameters모델의 파라미터를 측청한다

2) test the machine learning algorithm :머신러닝 알고리즘을 테스트한다. 머신러닝 모델이 얼마나 잘 작동하는지 평가



학습 데이터와 테스트 데이터를 4 블럭으로 나눠서 3 블럭은 학습 데이터로 쓰고, 1 블럭은 테스트를 하는데 쓴다.(4-Fold-Cross Validation)  테스트에 쓸 블럭을 바꿔가며 얼마나 잘 예측했는지 측정하여 다른 모델과 비교한다. Leave-one-out-Cross Validation은 모든 각각의 샘플을 블럭이라고 가정하고, 한 샘플을 테스트에 쓴다. 보통은 10 블럭으로 나누는 10-Fold Cross validation을 많이 사용한다. 



하이퍼 파라미터가 있는 모델의 경우 10-Fold-Cross Validation을 파라미터를 튜닝하는 데 사용한다.

파라미터 하나가 바뀌면 다른 모델로 취급

오늘 배운 거는 파라미터 튜닝하는 것인데 나중에 score로 아예 다른 모델(다른 알고리즘 쓰는 모델, 랜덤포레스트, 로지스틱...등)도 비교 가능하다.



랜덤서치cv를 사용할 때 refit을 써야하는지?
refit을 true로 하면 바로 검증+훈련 세트로 훈련시킴

랜덤서치를 써보고 좀 근처 값으로 그리드 서치를 해도 되는데 꼭 올라가는 건 아니다.
verbose 1 높이면 cv하는 상황이 조금 더 구체적으로 나온다. 코드 진행 상황이 보임

특징이 중요하면 데이터 클리닝의 영향을 많이 받음. 안 중요하면 별 영향 없다
일단 간단하게 라도 처리하고 모델 돌려보고, feature engineering 하기
컴퓨터를 괴로히는 것이 더 나을 수 있다..

랜덤 스테이트의 하이퍼 파라미터ㅋㅋ
대회니까 테스트 데이터에 맞출려고 넣어서 돌릴 수는 있음. 하지만 현실적이진 않음

타겟의 메이저 클래스가 많으면 f1 score 영향 많이 주게 됨. 이 때 클래스 웨이트를 좀 바꿔 볼 수도 있음

회귀 문제에서 주로 타겟 인코딩 사용
타겟 인코딩은 훈련 데이터에만 씀. 이걸 스무딩해서 테스트 셋에 인코딩을 할 때는 훈련데이터에 fit한 걸로 transform함

k fold, holdout 어떤 게 좋음?
일단 cv 쓰고, 데이터가 정말 많으면 holdout 써도 됨
데이터가 너무 많으면 cv하는데 시간 엄청 걸림
데이터가 적으면 cv
기준은 본인 컴퓨터 사양에 따라;;;

recall 줄고 precision 늘리는 방향으로 수정 가능?
가능. 특성 전처리 등 가능. 테스트 셋은 마지막에 한번 측정.

클래스 웨이트 : 모델 자체에 타겟 클래스 비중이 안 맞을 때. 비율이 적은 클래스에 모델에서 가중치
f beta score : beta가 크면 recall의 영향을 많이 받게 스코어가 나옴. 리콜이 안 좋으면 스코어가 낮게 나옴. 모델은 다 만들어진 상태에서 스코어만 좀 다르게 보는 것. 

보통 테스트 데이터 스코어가 검증 데이터 스코어보다 엄청 낮으면 문제
테스트 데이터의 f1 score를 보고 계속 돌아가서 고치면 결국 검증 데이터처럼 쓰게 되면
테스트 셋에 과적합되어서 테스트 테이터의 신뢰가 무너지는 것

검증 셋에서 높은 f1 score가 나오는 걸로 내기. 테스트 셋 f1 score 높아도 private leader board에서 높지 않을 수 있음.

특성별로 encoder 다른 거 써도 된다. 
--그럼 binary는 원핫 써볼까?

실무에서 데이터 언제 업데이트?
하루에 한번 ? 실시간? 일주일에 한번 등 훈련함

특성들끼리 상관관계 분석을 했는데...?타겟변수랑 크게 유의한 변수가 없는 것 같아서
그냥 다 썼다. selectKBest도 상관관계 분석, 히트맵보다 kbest. ->이것도 randomsearch에 넣고 쓰기

히트맵 상관관계 corrcoef는 하나의 특성과 하나의 특성의 관계임. 따라서 여러 특성 간의 관계를 보여주지 못함. 

파라미터 중 효율성을 위한 것이 있음. 컴퓨터 시간 줄이려고 특성을 줄이기도 함.

같이 써도 되는 데이터인가 고민..근데 그 데이터 자체의 이해가 있어야 함.



선형모델에서는 다중공선성 조심. 트리모델에는 그런 고민이 좀더 적어짐.



### 검증 곡선 

하이퍼파라미터 값의 영향을 시각화

```python
import matplotlib.pyplot as plt
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import validation_curve

param_range = np.arange(1,250, 10)
train_scores, test_scores = validation_curve(RandomForestClassifier(),
                                            X_train,
                                            y_train,
                                             #조사할 하이퍼 파라미터
                                            param_name = "n_estimators",
                                             #하이퍼파라미터 값의 범위
                                             param_range=param_range,
                                             cv=3,
                                             scoring ='accuracy',
                                             n_jobs = -1)

train_mean = np.mean(train_scores, axis=1)
train_std = np.std(train_scores, axis =1)

test_mean = np.mean(test_scores, axis =1)
test_std = np.std(test_scores, axis =1)
plt.plot(param_range, train_mean, label="Training score", color='black')
plt.plot(param_range, test_mean, label="cross_validation score", color = 'dimgrey')


plt.fill_between(param_range, train_mean - train_std, train_mean + train_std, color = 'gray')
plt.fill_between(param_range, test_mean-test_std, test_mean + test_std, color='gainsboro')

#그래프 출력
plt.title("validation curve w/ RF")
plt.xlabel('number of trees')
plt.ylabel('accuracy score')
plt.tight_layout()
plt.lagend(loc='best')
plt.show()
```



# 모델 선택

## 1. GridSearchCV

GreidSearchCV는 교차검증을 사용하여 모델을 선택하는 브루트포스한 방법이다. 사용자가 하이퍼 파라미터로 테스트 해보고 싶은 값들을 입력하면 GridSearchCV는 모든 값의 조합에 대해 모델을 훈련한다.

```python
hyperparameters = dict(C=C, penalty=penalty)

#그리드 서치 객체를 만든다
gridsearch = GridSearchCV(model, hyperparameters, cv = 5, verbose =1, n_jobs=-1)

#그리드 서치 수행
best_model = gridsearch.fit(X_train, y_train)
```



**최선의 하이퍼파라미터 출력**

```python
best_model.best_estimator_.get_params()['penalty'] #가장 좋은 패널티
```

**타깃 벡터를 예측**

```python
best_model.predict(X_train)
```









## 모델 선택 후 성능 평가

중복 교차검증(nested cross-validation)을 사용하여 편향된 평가를 피한다.



**Reference**

파이썬을 활용한 머신러닝 쿡북 Chapter 11 모델 평가, 12 모델 선택

https://robjhyndman.com/hyndsight/crossvalidation/