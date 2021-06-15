---
layout: default
title: "T academy 모델과 검증 & 앙상블"
date: 2021-06-15
parent: Article Reviews
grand_parent: Reviews
nav_order: 3
comments: true
---





[[캐글 x 데이타분석캠프 3] 모델과 검증 & 앙상블](https://tacademy.skplanet.com/live/player/onlineLectureDetail.action?seq=192)을 듣고, 빅분기 시험 전에 머신러닝 복습 할 겸 간단하게 들어본 것을 정리해보았다.



## 1강 인트로

머신러닝? 패턴을 찾는 것

보통 데이터를 쓸만하게 가공하는 전처리는 경진 대회에서 다루지 않는 부분

문제 설정 -> 데이터 수집 , 데이터 전처리 -> **EDA**, 인사이트 얻기, feature engineering,모델 개발, 모델 평가

공부팁: 다른 사람들의 kaggle 커널을 많이 읽어보기. 데이터와 사람이 많은 곳으로 가기



## 2강 데이터 확인, 결측치 처리, Log 변한

**라벨값 인코딩**

```python
label = label.map(lambda x: 1 if x == ">50K" else 0)
```

**컬럼 삭제**

```python
del train['id'] #del은 컬럼 한 개만 삭제
del test['id']
```



### 데이터 확인

```python
df.info()
```

object 범주형



**결측치가 '?' 이런식으로 들어가 있을 때!** nan값이 아니니까 EDA로 찾아내야한다.



```python
df.describe()
```

주로 확인하는 것: 이상치 max, min, mean, median

자세하게 볼 때는 그래프로 EDA 해봐야 함



* Tip: train을 복사한 train_temp를 만들어서 원본 보존하면서 전처리하기



### 결측치 

수치형 iterative imputer(MICE)

일부가 결측치이면 최빈값으로 하기도

범주형 변수: 군집으로 묶어서 넣기도 함

ex) 타이타닉에서 이름에 mr, mrs 붙어있는 것으로 나이 유추



범주형 변수를 결측치를 최빈값으로 채우기

```python
column_list
for c in column_list:
    train.loc[train[c] == '?', c] = train[c].mode()[0] #0 안붙이면 최빈 변수 이름
    test.loc[test[c] == '?', c] = test[c].mode()[0]
```



### 변수의 분포가 양 끝으로 쏠려있을 때

모델이 학습을 잘 못하니까 log 처리 후 기존 컬럼 삭제

```python
tmp_train['log_capital_gain'] = train['capital_gain'].map(lambda x: np.log(x) if x != 0 else 0)
```



## 3강 데이터 쪼개기, 스케일링, 인코딩

데이터 쪼개기 train, valid, test set

스케일링을 할 때 데이터를 쪼갠 후에!! 해야함. train, valid 나누기 전에 하면 validation score가 높게 나옴

인코딩은 쪼개기 전에 미리 해도 해도 된다.

```python
from sklearn.model_selection import train_test_split
tmp_train, tmp_valid, y_train, y_valid = train_test_split(tmp_train, lavel, test_size=0.3, random_state=2020, shuffle=True, stratify=label)
```



인덱스 초기화: 나중에 concat할 때 인덱스가 순서대로 있어야 제대로 붙음

```python
tmp_train = tmp_train.rest_index(drop=True) #시계열 데이터여서 인덱스 필요하면 false로
```



### 스케일링

카테고리 컬럼과 수치형 컬럼 메타 데이터 만들기

```python
cat_columns = [c for c,t in zip(tmp_train.dtypes.index, tmp_train.dtypes) if tmp_train.dtypes == 'O'] #대문자 O 면 범주형 칼럼임(object)
num_columns - [c for c in tmp_train.dtypes.index if c not in cat_columns]
```



StandardScaler: 표준편차 1, 평균 0으로 변환

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
tmp_train[num_columns] = scaler.fit_transform(tmp_train[num_colums])
tmp_valid[num_colums] = scaler.transform(tmp_valid[num_colums])
tmp_test[num_colums] = scaler.transform(tmp_test[num_colums])
```

변환 후에는 describe로 확인해보기. validation은 평균, 표준편차가 좀 다른 값 나올 수도 있음



### 인코딩

범주형 변수를 수치형 변수로 원핫인코딩

선형독립을 만들어주기 위해서(두 백터를 내적했을 때 0)

OneHotEncoding: 백터를 선형 독립으로 만들어주는 인코딩. 정석적 인코딩.하지만 차원의 저주에 걸릴 수 있음

LabelEncoding: 

트리 기반 모델이면 둘 중 어떤 걸 써도 상관없음



```python
from sklearn.preprocessing import OneHotEncoding

tmp_all = pd.concat([tmp_train, tmp_valid, tmp_test])
ohe = OneHotEncoder(spare=False)
ohe.fit(tmp_all[cat_columns]) #특성 뽑을 때는 train, valid 합쳐서
```

```python
#ohe.categories_ : 원핫인코딩된 특성 클래스 이름들 나옴 
#원핫인코딩된 칼럼에 이름 달아주기
ohe_columns = list()
for lst in ohe.categories_:
    ohe_colums += lst.tolist()
```

```python
#변환
new_train_cat = pd.DataFrame(ohe.transform(tmp_train[cat_colums]))
new_valid_cat =  pd.DataFrame(ohe.transform(tmp_valid[cat_colums]))
new_test_cat = pd.DataFrame(ohe.transform(tmp_test[cat_colums]))
```



정리

```python
tmp_train = pd.concat([tmp_train, new_train_cat], axis=1)
tmp_valid = pd.concat([tmp_valid, new_valid_cat], axis=1)
tmp_test = pd.concat([tmp_test, new_test_cat], axis=1)

#기존 범주형 변수 제거
tmp_train = tmp_train.drop(colums=cat_columns)
tmp_valid = tmp_valid.drop(columns=cat_columns)
tmp_test = tmp_test.drop(columns=cat_columns)
```





## 4강 Scikit-Learn 기반 분류 모델



## 5강 k-Fold 교차 검증



## 6강 Out-of-Fold(OOF) 앙상블



## 7강 Stacking 앙상블