---
layout: post
title: "2021년 1월 셋째주 TIL"
date: 2021-01-11
categories: [TIL]
excerpt: "2021.01.11-2021.01.17까지 TIL"
permalink: /TIL/2021-01-11-3rd-week-of-Jan/
tags: [ANOVA, levene, one-hot coding]
TIL: true
comments: true
typora-copy-images-to: ..\..\assets\img
---

## 2021.01.11(월)

---

자세한 코드나 오류 등은 Github TIL 레포에 저장하고 있고, 블로그 TIL엔 간단하게 오늘의 소감 정도만 적어야 겠다. 

##### 5F 회고법에 맞게 작성해보자

**Fact(사실)** : 오늘 스프린트 챌린지를 하면서 문제를 다 못 풀었다.

**Feeling(느낌)** :  개념을 헷갈려서 문제를 다 못 푼 것 같다. 

**Finding(교훈)** : ​​

1.ANOVA가 단순히 t-test의 샘플이 많을 때 하는 검정이라고 알고 있었는데, ANOVA의 한 종류인 일원배치분산분석은  세 개 이상의 집단으로 구성된 하나의 독립변수에 따라, 종속변수의 평균이 유의미한 차이가 있는지 검증할 때 쓴다는 것을 알게 되었다.

```python
from scipy import stats
fvalue, pvalue = stats.f_oneway(sample1, sample2, sample3, sample4) #깔끔하게 프린트 된다
print("fvalue:", fvalue, "pvalue:",pvalue)  
```

2.t-test 쓰기 위한 조건이 독립성, 정규성, 등분산성인데 등분산성 테스트(levene's test)는 오늘 처음 써 봤다.

```python
stats.levene(subset1,subset2,subset3,subset4) #statistic값과 pvalue 나옴
```



**Future action(행동)** : 질문 or 더 찾아볼거리

1. categorical feature은 chi2_ind로, continuous feature은 ANOVA로 pvalue를 구했는데 이 두 개를 섞어서 비교 가능? 
2. ANOVA 제대로 썼는지... 

**Feedback(피드백)** :  앞으로 통계공부할 때는 이 개념을 언제 쓸 수 있는지, 어떻게 설정하는지, 사용할 수 있는지 여부를 염두에 두고 공부하라는 말을 들었다.



git hub 블로그 너무너무 어렵다...마크다운 편집기에서 작성한 거랑 왜 이렇게 다르게 보이는지...심지어 숫자도 달라져!

---

## 2021.01.12(화)

---

**Fact**: 오늘은 선형대수(기초 행렬, 벡터)를 numpy로 구현하는 법을 배웠다.

**Feeling**: 통계학을 배울 때처럼 헷갈리는 개념은 별로 없었다.  ~~gem도 깔고 config도 수정했는데 jemoji를 넣는 길은 요원하다...~~일단 넣었는데 이제 크기가 이상해!

**Finding**: 다른 분이 질문해주신 벡터 차원 관련 질문에서 tensor라는 개념을 새로 알게되었다!

* **tensor:** 선형관계를 나타내는 다중선형대수학의 대상

vector는 항상 1 tensor의 형태로 나타나지만, 다양한 차원을 가질 수 있다. 예를 들어 좌표평면에서 x,y를 표현할 때 주로 쓰는 2x1 벡터는 2차원이다.


$$
\vec{V1}= [0,1,1,3] \quad 1차원
$$

$$
\vec{V2} = [[0],[1]] \quad 2차원
$$

cf)tf.Tensor 객체의 랭크는 그 차원의 수이지만 수학에서 쓰는 행렬의 랭크와 다른 의미이다. 랭크가 1인 Tensor의 Type은 벡터라고 https://www.tensorflow.org/guide/tensor?hl=ko에서도 나와있다.



* **MAE와 MSE**

**MSE**(mean squared error, 평균 제곱 오차): 잔차(오차)의 제곱에 대한 평균을 취한 값 


$$
MSE = E [ (X - \hat{X})^2] = \frac{1}{N}\Sigma(X-\hat{X})^2
$$


**MAE**(mean absolute error, 평균절대오차) : 모든 절대오차의 평균


$$
MAE = \frac{1}{N}\Sigma(|X-\hat{X}|)
$$



**사용례**

**평균 제곱 오차(MSE)**는 회귀에서 자주 사용되는 **손실 함수**입니다. 정확도 개념은 회귀에 적용되지 않습니다. 일반적인 **회귀 지표**는 **평균 절대 오차(MAE)**입니다. MSE는 손실함수로써 쓰이고  MAE는 회귀지표로써 사용된다. 

[^ ]:https://blog.naver.com/PostView.nhn?blogId=heygun&logNo=221516529668&parentCategoryNo=&categoryNo=56&viewDate=&isShowPopularPosts=true&from=search

**Future action** 선형대수 정리한 것 복습

**Feedback** 

---

## 2021.01.13(수)

---

**Fact:** 오늘은 선형대수에서 span, basis, rank로 벡터의 차원 대한 것과 투영에 대해 배웠다. 

**Feeling**: 음 혼란스럽다...수학적 개념은 대강 알겠는데 이걸 실제로 어떻게 쓰게 될지 모르겠다. 그리고 코딩 문제를 풀려고 저지사이트를 기웃거려 봤는데 처음이라 그런지 낯설고 채점도 자꾸 에러나고...가야할 길이 멀다는 생각이 든다.

**Finding:** https://twlab.tistory.com/24 벡터의 span, basis, rank, linear independence에 대해 잘 정리된 블로그다.

* 이 개념들의 사용되는 곳: 차원 축소

* 이 개념들을 사용하는 법:? 

```python
np.linalg.matrix_rank(array)
```

* 개념 이해:

내가 이해한 바로는 여러 벡터가 선형독립인지 종속인지 알기 위해 각 벡터들에 스칼라 곱을 하고 조합해서 0이 나올 수 있는지 확인한다. 
$$
[2,1],[3,2],[1,2]의 \; 벡터가 \;있을 때\quad C_1[2,1]+C_2[3,2]+C_3[1,2]=[0,0]인지 \;확인
$$
이때, C1, C2, C3 적어도 하나는 0이 아니어야 선형 종속이다. 모든 벡터에 곱해질 스칼라가 0이라면 선형독립이다. 이 뜻은 각 벡터가 독립이기에 0이 아닌 어떤 수를 곱하고 이를 조합해서 다른 벡터를 만들지 못한다는 의미다. 

 n개의 벡터가 선형독립이면 이들은 span to Rn이다. (n차원으로 span한다.)  이때 만약 선형종속인 벡터가 있어서 n보다 적은 차원이 된다면 이들은 Rn의 선형 부분공간에 속하게 된다. 선형독립이 아닌 벡터는 redundant하며 기존에 있는 벡터와 겹쳐서 스케일만 다를 뿐 같은 정보라고 볼 수 있다.

Basis(기저)는 벡터공간을 생성하는데 필요한 최소한의  벡터집합이다. 즉, 선형관계에 있지 않은 벡터들의 모음이며, R2의 basis는 서로 독립적인 두 벡터들의 (다양한) 조합이다. span의 역개념에 해당하며 basis는 n차원을 생성하는데 필요한 최소 벡터의 개수라고 생각하면 될 것 같다.  

Rank는 매트릭스의 열을 만들고 있는 벡터들로 만들 수 있는 span의 차원이다. 일부 벡터가 선형관계이면 매트릭스 자체의 차원과 달라진다. 가우스 소거를 통해 매트릭스의 선형종속인 벡터를 제거하고 나면 rank를 알 수 있다.


**Future Action**: 블로그 글로 잘 정리하자. 코딩 문제 하루에 조금씩만 풀어보자.

**Feedback**: 

:paw_prints:

---



# 21.01.14(목)

---

**Fact**: 오늘 배운 PCA와 scree plot 이해하느라고 TIL도 못 쓰고 주말에 쓰는 중...

**Feeling**: 진짜 하루가 갈수록 배운 것을 이해하고 소화하는데 더 오래 걸린다. 대략 8시쯤에 과제 끝내고 나면 그제서야 좀 이해가 가는듯

**Finding**: One hot encoding의 프로세스를 알게 되었다.

**Future Action**: PCA의 프로세스 제대로 정리하기 

**Feedback**: 아직 질문한 건 없는데 질문거리가 많다

근데 맨날 feedback 받는 거 아니니까 feedback은 food for thought 으로 변경합니다...

**Food for thought**: 

[ ] 원 핫 인코딩할 때 결측치 처리 방법: 결측치도 숫자 하나 배당되나? 결측치 처리하는 법에는 1) 결측치 있는 열 drop, 2) imputation, 3) imputation with extension(기존 데이터 유지하면서 null 값에만 imputation 처리함)

```python
from sklearn.preprocessing import Imputer

my_imputer = Imputer()
imputed_X_train = my_imputer.fit_transform(X_train)
imputed_X_test = my_imputer.transform(X_test)
print("Mean Absolute Error from Imputation:")
print(score_dataset(imputed_X_train, imputed_X_test, y_train, y_test))
```

결론은 imputation을 하면 원핫인코딩을 했을 때 큰 차이는 없다..?

[^ ]: https://www.kaggle.com/rogelioem/machine-learning-missing-data-one-hot-encoding

---



# 21.01.15(금)

---

**Fact**: 후 오늘도 어려웠다. K means 등 클러스터링 하는 방법을 배웠는데 hierachical clustering은 무슨 의미가 있는지 파악하지 못했다. agglomerateclustering 함수 대신 linkage를 썼는데 이게 맞는지도 모르겠다.

**Feeling**: ​😵

**Finding**: 1) 다른 동기분들 답안를 보고 그냥 K-means와 PCA 후 K-means를 비교하는 문제의 문제 의도(Accuracy 비교)를 알게되었다. 

2) 구글 코랩에 로컬 파일 업로드 하는 것과 raw 파일을 주소로 불러오면 차이가 날 수 있다. 오늘 파일은 엑셀로 열어보니 마지막 칼럼 head가 옆 셀을 침범해서 nan으로 채워진 column이 하나 더 생겼다. 앞으로 데이터를 수집하고 process 하기 전에 꼼꼼히 살피는 습관을 들여야 겠다. 

**Future Action**: Accuracy 더 알아보기, plot 그리는 법 정리하기

앞으로도 과제 제출 후에 다른 사람들이 제출한 답안 몇 개 읽어봐야 겠다. 생각보다 이 과정에서 배우는 게 많다. 난 re가 불편해서 잘 안 쓰는데 이걸 자주 쓰시는 분들도 계시고, 사람에 따라 코드 작성하는데 미묘한 차이가 나는 것이 재미있다.

**Food for thought**: 🍲PCA(Feature 개수 줄이기)와 K-means(sample 개수 줄이기)가 데이터를 가공하는데 어떤 역할을 하는지 음미해보자.

---


