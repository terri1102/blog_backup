---
layout: default
title: "T academy 모델과 검증 & 앙상블"
date: 2021-06-15
parent: ETC
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
column_list = #결측치 있는 컬럼들
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

**스케일링을 할 때 데이터를 쪼갠 후에!! 해야함**. train, valid 나누기 전에 하면 validation score가 높게 나옴

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

### 1. 로지스틱 회귀 분석

### 2. 서포트 벡터 머신(분류)

주어진 데이터를 바탕으로 하여 클래스 사이의 간격(Margin)을 최대화하는 데이터 포인트(support vector)

를 찾아내고, 그 서포트 벡터에 대해 수직인 경계를 통해 데이터를 분류하는 알고리즘



#### 경험적 위험 최소화 vs 구조적 위험 최소화

* 경험적 위험 최소화(Empirical Risk Minimization, ERM):

  훈련 데이터에 대해 손실을 최소화

  학습 알고리즘의 목표

  신경망, 결정트리, 선형 회귀 등

* 구조적 위험 최소화(Structural Risk Minimization, SRM):

  관찰하지 않은(Unseen) 데이터에 대해서도 위험을 최소화

  오차 최소화를 일반화 시키는 것



#### Cost: soft or hard

Soft Margin은 유연한 경계면을 만들어내고, Hard Margin은 분명하게 나누는 경계면을 만들어냅니다. Soft Margin은 경계면을 조금씩 넘어가는 데이터들(비용)을 감수하면서 가장 최선의 경계면을 찾는다.



#### 저차원을 고차원으로 Kernel Trick

다음과 같이 저차원에서는 선형 분리가 되지 않을 수 있지만, 고차원에서는 선형 분리가 가능할 수 있습니다. 이러한 원리를 바탕으로 선형 분리가 불가능한 저차원 데이터를 선형 분리가 가능한 어떤 고차원으로 보내 선형 분리를 할 수 있습니다. 여기에서 Kernel 함수라 불리는 매핑 함수를 사용하는데, 실제로 고차원에서 연산한 것과 같은 결과를 내준다 하여 Kernel Trick이라 부릅니다.

대표적인 kernel 함수: Linear, Poly, RBF(방사기저함수), Hyper-Tangent



## XGBoost

* Boosting Tree 알고리즘
* Gradient Boosting Model 대비 빠른 수행 시간
* 과적합 규제 기능
* 가지치기 기능
* 자체 교차 검증 기능
* 결측치 자체 처리
* Early stopping 기능
* 균형 트리 분할 방식(대칭 트리 형성)



GPU로 돌리기

```python
xgb = XGBClassifier(tree_method='gpu_hist')
```





## LightGBM

* XGBoost의 장점 +
* XGBoost보다 가볍고 빠른 모델, 하지만 더 나은 학습 성능
* 리프 중심 트리 분할 방식(비대칭 트리 형성)
* 적은 데이터에서 과적합 우려

GPU로 돌리기

```python
lgb = LGBMClassifier(tree_method='gpu_hist')
```



**오토엠엘:** NII, hyperopt, kerastuner, pycaret(너무 자동화가 되어있음)

하이퍼 파라미터를 알아서 튜닝해줌



## 5강 k-Fold 교차 검증

Hold out 방법으로 충분한 검증이 되는가? 한 번만 쪼개는데?

이 것을 K개로 쪼개서 검증을 하겠다는 것이 K-Fold Cross Validation.

보통 훈련 시간이 너무 오래 걸리고, validation set이 10%미만으로 너무 적기 때문에 K는 10을 초과해서 하지는 않는다. 보통 5개로 쪼개서 하는 경우가 많다. 5번 검증을 해서 검증 스코어가 5번 나온 후 이를 평균낸다.



train과 validation set이 계속 바뀌기 때문에 전처리 과정을 이 안에 넣어야 한다.

```python
def preprocess(x_train, x_valid, x_test):
    tmp_x_train = x_train.copy()
    tmp_x_valid = x_valid.copy()
    tmp_x_test = x_test.copy()
    
    tmp_x_train = tmp_x_train.reset_index(drop=True)
    tmp_x_valid = tmp_x_valid.reset_index(drop=True)
    tmp_x_test = tmp_x_test.reset_index(drop=True)
    
    #이 부분!
    for c in has_na_columns: #결측치 있는 컬럼(위에 코드에선 column_list임)
        tmp_x_train.loc[tmp_x_train[c] == '?', c] = tmp_x_train[c].mode()[0]
        tmp_x_valid.loc[tmp_x_valid[c] == '?', c] = tmp_x_valid[c].mode()[0]
        tmp_x_test.loc[tmp_x_test[c] == '?', c] = tmp_x_test[c].mode()[0]
        
    tmp_x_train['log_capital_loss'] = tmp_x_train['capital_loss'].map(lamda x: np.log(x) if x!= 0 else 0)
    tmp_x_valid['log_capital_loss'] = tmp_x_valid['capital_loss'].map(lamda x: np.log(x) if x!= 0 else 0)
    tmp_x_test['log_capital_loss'] = tmp_x_test['capital_loss'].map(lamda x: np.log(x) if x!= 0 else 0)
    
    tmp_x_train['log_capital_gain'] = tmp_x_train['capital_gain'].map(lamda x: np.log(x) if x!= 0 else 0)
    tmp_x_valid['log_capital_gain'] = tmp_x_valid['capital_gain'].map(lamda x: np.log(x) if x!= 0 else 0)
    tmp_x_test['log_capital_gain'] = tmp_x_test['capital_gain'].map(lamda x: np.log(x) if x!= 0 else 0)
     
    tmp_x_train = tmp_x_train.drop(columns=['capital_loss', 'capital_gain'])
    tmp_x_valid = tmp_x_valid.drop(columns=['capital_loss', 'capital_gain'])
    tmp_x_test = tmp_x_test.drop(columns=['capital_loss', 'capital_gain'])
    
    scaler = StandardScaler()
    tmp_x_train[num_columns] = scaler.fit_transform(tmp_x_train[num_columns])
    tmp_x_valid[num_columns] = scaler.transform(tmp_x_valid[num_columns])
    tmp_x_test[num_columns] = scaler.transform(tmp_x_test[num_columns])
    
    tmp_all = pd.concat([tmp_x_train, tmp_x_valid, tmp_x_test])
    
    ohe = OneHotEncoder(sparse=False)
    ohe.fit(tmp_all[cat_columns]) #onehotencoder는 fit은 전체, trnaform은 각각
    
    ohe_columns = list()
    for lst in ohe.categories_:
        ohe_columns += lst.tolist()
    
    tmp_train_cat = pd.DataFrame(ohe.transform(tmp_x_train[cat_columns]), columns = ohe.columns)
    tmp_valid_cat = pd.DataFrame(ohe.transform(tmp_x_valid[cat_columns]), columns = ohe.columns)
    tmp_test_cat = pd.DataFrame(ohe.transform(tmp_x_test[cat_columns]), columns = ohe.columns)
    
    tmp_x_train = tmp_x_train.drop(columns=cat_columns)
    tmp_x_valid = tmp_x_valid.drop(columns=cat_columns)
    tmp_x_test = tmp_x_test.drop(columns=cat_columns)
    
    #np.array로 반환
    return tmp_x_train.values,tmp_x_valid.values, tmp_x_test.values
```

```python
#xgboost에 f1 score가 없으니 직접 구현(fit할 때 넣어야 해서 sklearn꺼 못 씀)
def xgb_f1(y, t, threshold=0.5):
    t = t.get_label()
    y_bin = (y > threshold).astype(int)
    return 'f1', f1_score(t, y_bin, average='micro')
```

```python
from sklearn.model_selection import KFold, StratifiedKFold
n_splits = 5
skf = StratifiedKFold(n_splits=n_splits, shuffle=True, random_state=2020)
```

```python
val_scores = list()
oof_pred = np.zeros((test.shape[0],))

for i, (trn_idx, val_idx) in enumerate(skf.split(train, label)): #라벨 비율에 맞게 쪼개줌-근데 인덱스만 줌!!!!!!!!
    x_train, y_train = train.iloc[trn_idx, :], label[trn_idx]
    x_valid, y_valid = train.iloc[val_idx, :], label[val_idx]
    
    #전처리
    x_train, x_valid, x_test = preprocess(x_train, x_valid, test)
    
    #모델 정의
    clf = XGBClassifier(tree_method='gpu_hist')
    
    #모델 학습
    clf.fit(x_train, y_train,
           eval_set = [[x_valid, y_valid]], #xgb나 lightgbm은 이런 식으로 eval set 넣는 거 가능. sklearn 모델들은 불가
           eval_metric = xgb_f1, #lightgbm은 이 부분 바꿔야함
           early_stopping_rounds = 100, #트리 개수는 많이 만들고 early stopping으로 멈춤
           verbose = 100,
           )
    
    #훈련, 검증 데이터 f1 score 확인
    trn_f1_score = f1_score(y_train, clf.predict(x_train), average='micro')
    val_f1_score = f1_score(y_valid, clf.predict(x_valid), average='micro')
    print('{} Fold, train f1_score : {:.4f}4, validation f1_score : {:.4f}\n'.format(i, trn_f1_score, val_f1_score))
    
    val_scores.append(val_f1_score)
    
# 교차 검증 f1 score 평균 계산
print('Cross Validation Score : {:.4f}'.format(np.mean(val_scores)))
```







## 6강 Out-of-Fold(OOF) 앙상블

OOF: K Fold에서 값을 섞는 것.

![OOF1]

K Fold 검증을 하면서 모델을 하나 만들고 추론(predict_proba)을 동시에 함. 이렇게 5 fold로 모델이 5개가 나오면 이 예측값을 평균((proba1+proba2+proba3+proba4+proba5)/5)을 내서 앙상블을 한다. 

케글에서는 리더보드 스코어 대로 가중치를 두어서 섞기도 한다.;;;

시드는 고정하고 해야한다. 

하드보팅보다는 소프트보팅이 더 좋다.
```python
#위의 코드와 거의 동일
val_scores = list()
oof_pred = np.zeros((test.shape[0],)) #(6512,) 테스트 샘플 개수만큼 차원을 갖는 0으로 구성된 벡터를 만듦. 결과를 담아둘 통을 만듦

for i, (trn_idx, val_idx) in enumerate(skf.split(train, label)): #라벨 비율에 맞게 쪼개줌-근데 인덱스만 줌!!!!!!!!
    x_train, y_train = train.iloc[trn_idx, :], label[trn_idx]
    x_valid, y_valid = train.iloc[val_idx, :], label[val_idx]

    #전처리
    x_train, x_valid, x_test = preprocess(x_train, x_valid, test)

    #모델 정의
    clf = XGBClassifier(tree_method='gpu_hist')

    #모델 학습
    clf.fit(x_train, y_train,
           eval_set = [[x_valid, y_valid]],
           eval_metric = xgb_f1,
           early_stopping_rounds = 100, #트리 개수는 많이 만들고 early stopping으로 멈춤
           verbose = 100,
           )

    #훈련, 검증 데이터 log loss 확인
    trn_f1_score = f1_score(y_train, clf.predict(x_train), average='micro')
    val_f1_score = f1_score(y_valid, clf.predict(x_valid), average='micro')
    print('{} Fold, train f1_score : {:.4f}4, validation f1_score : {:.4f}\n'.format(i, trn_f1_score, val_f1_score))

    val_scores.append(val_f1_score)

    oof_pred += clf.predict_proba(x_test)[:,1]/n_split #[:,1]인 이유: 첫째 열 무시, 확률값만 가져옴

#교차 검증 F1 score 평균 계산
print('Cross Validation Score : {:.4f}'.format(np.mean(val_scores)))
```

```python
(oof_pred > 0.5).astype(int)
```



### 하이퍼 파라미터 튜닝을 할 때

```python
#위의 코드와 거의 동일
val_scores = list()
oof_pred = np.zeros((test.shape[0],)) #(6512,) 테스트 샘플 개수만큼 차원을 갖는 0으로 구성된 벡터를 만듦. 결과를 담아둘 통을 만듦


#함수를 만들어서 튜닝함
def train(args):

    for i, (trn_idx, val_idx) in enumerate(skf.split(train, label)): #라벨 비율에 맞게 쪼개줌-근데 인덱스만 줌!!!!!!!!
        x_train, y_train = train.iloc[trn_idx, :], label[trn_idx]
        x_valid, y_valid = train.iloc[val_idx, :], label[val_idx]

        #전처리
        x_train, x_valid, x_test = preprocess(x_train, x_valid, test)

        #모델 정의
        #여기 바뀌는 것 확인!!!!!
        clf = XGBClassifier(max_depth  =args.max_depth,
                            subsample = args.subsample,
                            colsample_bytree = args.colsample_bytree,
                            reg_lambda = args.reg_lambda
                            reg_alpha = args.reg_alpha,
            tree_method='gpu_hist')

        #모델 학습
        clf.fit(x_train, y_train,
               eval_set = [[x_valid, y_valid]],
               eval_metric = xgb_f1,
               early_stopping_rounds = 100, #트리 개수는 많이 만들고 early stopping으로 멈춤
               verbose = 100,
               )

        #훈련, 검증 데이터 log loss 확인
        trn_f1_score = f1_score(y_train, clf.predict(x_train), average='micro')
        val_f1_score = f1_score(y_valid, clf.predict(x_valid), average='micro')
        print('{} Fold, train f1_score : {:.4f}4, validation f1_score : {:.4f}\n'.format(i, trn_f1_score, val_f1_score))

        val_scores.append(val_f1_score)

        oof_pred += clf.predict_proba(x_test)[:,1]/n_split #[:,1]인 이유: 첫째 열 무시, 확률값만 가져옴

#교차 검증 F1 score 평균 계산
print('Cross Validation Score : {:.4f}'.format(np.mean(val_scores)))
```





## 7강 Stacking 앙상블

2 stage 앙상블인 Stacking 앙상블은 수십 개의 1 stage 모델의 결과를 모아 2 stage 모델로 학습 후 결과를 내는 앙상블 방식이다.

효율이 안 좋음..아주 조금 오름. 과적합 우려도 있음. 현업에선 가성비를 챙기기 때문에 잘 안 쓰는 편. 성능 낮아질 수도 있다. 쥐어짜는 방법ㅋㅋㅋ

![stacking1]

validation predict 값들을 다 모음-5 fold일 때 5개의 prediction set이 나오면 이를 concat함. 모델을 아주 많이 모아서 새 데이터셋을 만들기도 한다. 

![staking2]

새로운 데이터셋을 이용해서 train한다.



### stacking 앙상블 과정

1) 1 stage 결과 모으기

stacking 앙상블을 진행할 1 stage 모델의 결과(train, test)를 모은다.

```python

val_scores = list()

new_x_train_list = [np.zeros((train.shape[0], 1)) for _ in range(4)] #(train.shape[0], 1) 벡터형태. 모델 4개
new_x_test_list = [np.zeros((test.shape[0], 1)) for _ in range(4)]

for i, (trn_idx, val_idx) in enumerate(skf.split(train, label)): #라벨 비율에 맞게 쪼개줌-근데 인덱스만 줌!!!!!!!!
    x_train, y_train = train.iloc[trn_idx, :], label[trn_idx]
    x_valid, y_valid = train.iloc[val_idx, :], label[val_idx]

    #전처리
    x_train, x_valid, x_test = preprocess(x_train, x_valid, test)

    #모델 정의
    clfs = [LogisticRegression,
           RandomForestClassifier,
           XGBClassifier(tree_method='gpu_hist'),
           LGBMClassifier(tree_method='gpu_list')]

    for model_idx, clf in enumerate(clfs):
        clf.fit(x_trian, y_train)
        
        new_x_train_list[model_idx][val_idx, :] = clf.predict_proba(x_valid)[:,1].reshape(-1,1)# #벡터->행렬(아래 블럭에서의 연산을 위해서)
        #reshape(-1,1)의 의미: -1은 컴퓨터가 알아서 해라.
        #하지만 이미지나 시계열 데이터는 차원 바꿀 때 더 조심해야함. 우리가 생각하지 못하는 차원이 있을 수 있기 때문에
        
        new_x_test_list[model_idx][:] += clf.predict_proba(x_test)[:,1].reshape(-1,1)/n_splits
    
```

```python
new_train = pd.DataFrame(np.concatenate(new_x_train_list, axis=1), columns=None)
new_label = label #아무 것도 안 한 라벨
new_test = pd.DataFrame(np.concatenate(new_x_test_list, axis=1), columns=None)

new_train.shape, new_label.shape, new_test.shape
```

### 2) 2 Stage Meta Model 학습

```python
val_scores = list()
oof_pred = np.zeros((test.shape[0],))


for i, (trn_idx, val_idx) in enumerate(skf.split(new_train, new_label)): 
    x_train, y_train = new_train.iloc[trn_idx, :], new_label[trn_idx]
    x_valid, y_valid = new_train.iloc[val_idx, :], new_label[val_idx]

    #전처리
    scaler = StandardScaler()
    x_train = scaler.fit_transform(x_train)
    x_valid = scaler.transform(x_valid),
    x_test = scaler.transform(new_test)

    #모델 정의 : 뭘 쓰든 상관없음
    clfs = XGBClassifier(tree_method='gpu_hist')
	
    
    #모델 학습
    clf.fit(x_train, y_train,
           eval_set = [[x_valid, y_valid]], #xgb나 lightgbm은 이런 식으로 eval set 넣는 거 가능. sklearn 모델들은 불가
           eval_metric = xgb_f1, #lightgbm은 이 부분 바꿔야함
           early_stopping_rounds = 100, #트리 개수는 많이 만들고 early stopping으로 멈춤
           verbose = 100,
           )
    
    #훈련, 검증 데이터 f1 score 확인
    trn_f1_score = f1_score(y_train, clf.predict(x_train), average='micro')
    val_f1_score = f1_score(y_valid, clf.predict(x_valid), average='micro')
    print('{} Fold, train f1_score : {:.4f}4, validation f1_score : {:.4f}\n'.format(i, trn_f1_score, val_f1_score))
    
    val_scores.append(val_f1_score)
    
	oof_pred += clf.predict_proba(x_test)[:,1]/n_splits
#교차 검증 f1 score 평균 계산
print('Cross Validation Score : {:.4f}'.format(np.mean(val_scores)))    
```



## 6. 결과 만들기

```python
import os
os.listdir(**)
```

```python
submit = pd.read_csv('sample_submission.csv')
```

```python
submit.loc[:, 'prediction'] = (oof_pred > 0.5).astype(int)
```

```python
submit.to_csv('submission.csv',index=False)
```

