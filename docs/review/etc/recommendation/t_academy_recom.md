---
layout: default
title: 추천시스템의 이해
date: 2021-04-17
grand_parent: Reviews
parent: ETC
comments: true
nav_order: 12
---



### Table of Contents

0. 추천시스템 이해

- 추천시스템 개요
- 기업에서의 추천시스템
- 과거의 추천시스템

1. 컨텐츠 기반 추천
   - 컨텐츠 기반 모델 이론
   - TF-IDF 모델
   - 유사도 함수
2. 협업 필터링
   - KNN
   - SGD
   - ALS
3. 평가 함수
   - Accuracy, RMSE, F1 score
   - MAP
   - NDCG
4. 분석 실습



<br>

# 추천시스템의 개요

---

**추천시스템:** 추천시스템은 사용자(user)에게 상품(item)을 제안하는 소프트웨어 도구이자 기술. 이러한 제안은 어떤 상품을 구매할지, 어떤 음악을 들을지 또는 어떤 온라인 뉴스를 읽을지와 같은 다양한 의사결정과 연관이 있음.

**목표:** 어떤 사용자에게 어떤 상품을 어떻게 추천할 것인지에 대한 이해

###### Recommender Systems Handbook

<br>

**파레토와 롱테일의 법칙** <br>

파레토의 법칙: 상위 20%가 80%의 가치를 창출한다. <br>

롱테일의 법칙: 하위 80%의 가치가 상위 20%의 가치가 더 크다. -> 추천시스템



<br>

**추천시스템의 역사**

![추천시스템역사]



## 연관분석(Association Analysis)

* **정의** : 룰(rule) 기반 모델로서 상품과 상품 사이에 어떤 연관이 있는지 찾아내는 알고리즘. 이러한 연관은 2가지 형태로 존재한다.

* **연관**: 

  * 1) 얼마나 같이 구매되는가?

  * 2)  A 아이템을 구매하는 사람이 B 아이템을 구매하는가?

이렇게 두 가지 형태. 장바구니 분석이라고 하기도 함.

<br>

**평가지표**

<br>

* Support(지지도) : A 구매할 확률

$$
For \; the \; rule \; A \rightarrow B, \; support(A) = P(A)
$$

* confidence(신뢰도) : A를 구매했을 떄 B까지 구매할 확률

$$
confidence(A \rightarrow B) = \frac{P(A,B)}{P(A)}
$$



* lift(향상도) : 두 사건이 동시에 얼마나 발생하는지 비율, 독립성 측정

$$
lift(A \rightarrow B) = \frac{P(A,B)}{P(A)*P(B)}
$$



### Apriori 알고리즘

A priori 원리는 아이템셋의 증가를 줄이기 위한 방법. 기본적인 아이디어는 "빈번한 아이템셋은 하위 아이템셋 또한 빈번할 것이다"는 것. 즉 "빈번하지 않은 아이템셋은 하위 아이템셋 또한 빈번하지 않다"는 것을 이용해서 아이템셋의 증가를 줄이는 방법

<br>

아이디어

```sql
{2, 3}의 지지도 > {0,2,3}, {1,2,3}의 지지도
-P(item 2, item 3) > P(item 0, item 2, item 3)
-P(item 2, item 3) > P(item 1, item 2, item 3)

```

<br>

**Apriori 알고리즘의 과정**

1. k개의 item을 가지고 `단일항목집단` 생성(one-item frequent set)

- 단일항목집단: 아이템 한 개만 구매하는 경우

2. 단일항목집단에서 최소 지지도(support) 이상의 항목만 선택
3. 2번에서 선택된 항목만을 대상으로 `2개 항목집단` 생성
4. 2개 항목집단에서 최소 지지도 혹은 신뢰도 이상의 항목만 선택
5. 위의 과정을 k개의 k-item frequent set을 생성할 때까지 반복

<br>

**데이터(Implicit Feedback)**

Implicit Feedback: 구매는 했지만 실제로 만족했는지는 알 수 없음

| 거래번호 | 상품목록           |
| -------- | ------------------ |
| 0        | 우유, 기저귀, 쥬스 |
| 1 | 양상추, 기저귀, 맥주|
|2| 우유, 양상추, 기저귀, 맥주 |
|3| 양상추, 맥주|

이를 매트릭스로 변형하면 sparse matrix(희소행렬)이 나옴

| 거래번호 | 우유 | 양상추 | 기저귀 | 쥬스 | 맥주 |
| -------- | ---- | ------ | ------ | ---- | ---- |
| 0        | 1    | 0      | 1      | 1    | 0    |
|1|0|1|1|0|1|
|2|1|1|1|0|1|
|3|0|1|0|0|1|



1. 단일항목집단 생성: 우유, 양상추, 기저귀, 맥주, 쥬스
2. 지지도 계산: 우유(2/4), 양상추(3/4)...쥬스(1/4)
3. 최소 지지도를 0.5로 할 때 쥬스(1/4)는 제거 -> {우유, 양상추, 기저귀, 맥주}
4. 3에서 선택된 항목만을 대상으로 2개 항목 집단 생성 : {우유, 양상추}, {우유, 기저귀}, {우유, 맥주}, {양상추, 기저귀}, {양상추, 맥주}, {기저귀, 맥주}
5. 집단 별 지지도 구한 후 최소 지지도 이상의 항목만 선택: ~~{우유, 양상추} : 0.25~~, {우유, 기저귀} : 0.5, ~~{우유, 맥주} : 0.25~~, {양상추, 기저귀} : 0.5, {양상추, 맥주} : 0.75, {기저귀, 맥주} : 0.5

6. 위의 과정을 k개의 k-item frequent set을 생성할 때까지 반복 : {양상추, 기저귀, 맥주}

* 지지도 말고 다른 척도로도 사용 가능하다

<br>

**Apriori 알고리즘 장단점**

장점: 원리가 간단하여 사용자가 쉽게 이해 가능. 유한 연관성을 갖는 구매패턴을 찾아줌

단점: 데이터가 클 경우 (item이 많은 경우)에 속도가 느리고 연산량이 많음. 실제 사용시에 많은 연관상품들이 나타나는 단점



---

# FP-Growth

FP Growth는 Apriori의 속도측면의 단점을 개선한 알고리즘.  Apriori와 비슷한 성능을 내지만 FP Tree라는 구조를 사용해서 속도가 빠르다. 하지만 `동일하게 발생하는 아이템셋`을 찾는데는 좋지만 `아이템간의 연관성`을 찾는 것은 어렵다.

<br>

**FP-Growth 알고리즘의 과정**

1. 모든 거래를 확인하여 각 아이템마다의 지지도(support)를 계산하고 최소 지지도이상의 아이템만 선택
2. 모든 거래에서 빈도가 높은 아이템 순서대로 순서를 정렬
3. 부모 노드를 중심으로 거래를 자식노드로 추가해주면서 tree를 생성
4. 새로운 아이템이 나올 경우에는 부모노드부터 시작하고, 그렇지 않으면 기존의노드에서 확장
5. 위의 과정을 모든 거래에 대해 반복하여 FP TREE를 만들고 최소 지지도 이상의 패턴만을 추출



<br>

**예시**



1, 2 번까지는 Apriori와 동일 

3. 쥬스 삭제되고 빈도가 높은 {양상추, 기저귀, 맥주} -> {우유} 순으로 정렬

| 거래번호 | 상품목록                   |
| -------- | -------------------------- |
| 0        | 기저귀, 우유, ~~쥬스~~     |
| 1        | 양상추, 기저귀, 맥주       |
| 2        | 양상추, 기저귀, 맥주, 우유 |
| 3        | 양상추, 맥주               |

3. 부모 노드를 중심으로 거래를 자식노드로 추가해주면서 tree를 생성 

```python
거래번호 0
    [기저귀:1] ―― [우유:1]
    /
   /
[Root]
```

4. 새로운 아이템이 나올 경우에는 부모노드부터 시작하고, 그렇지 않으면 기존의 노드에서 확장

    거래번호 1
        [기저귀:1] ―― [우유:1]
        /
       /
    [Root]
       \
        \
         [양상추:1] ―― [기저귀:1] ―― [맥주:1]
    
    거래번호 2   
       [기저귀:1] ―― [우유:1]
        /
       /
    [Root]
       \
        \
         [양상추:2] ―― [기저귀:2] ―― [맥주:2] ―― [우유:1] 
```python
거래번호 3   
   [기저귀:1] ―― [우유:1]
    /
   /
[Root]
   \
    \
     [양상추:3] ―― [기저귀:2] ―― [맥주:2] ―― [우유:1] 
        \
         \
           [맥주:1]
```

| 아이템 | 지지도 | Conditional Pattern bases |
| ------ | ------ | ------------------------- |
| 기저귀 | 0.75   | {양상추} : 2 |
|양상추|0.75|{}|
|맥주|0.75|{양상추, 기저귀} : 2 {양상추} : 1|
|우유|0.5|{양상추, 기저귀, 맥주} : 2 {기저귀} : 1|




5. 지지도가 낮은 순서부터 시작하여 조건부 패턴을 생성 (우유)

<br>



**FP-Growth 알고리즘 장단점**

장점: Apriori 알고리즘보다 빠르고 2번의 탐색만 필요로 함

 		후보 Itemsets을 생성할 필요없이 진행 가능

단점: 대용량의 데이터셋에서 메모리를 효율적으로 사용하지 않음

​			Apriori 알고리즘에 비해서 설계하기 어려움

​			지지도의 계산이 FP-Tree가 만들어지고 나서야 가능함



<br>

<br>

# 2강 컨텐츠 기반 모델(유사도 함수, TF-IDF)

---

**컨텐츠 기반 추천시스템** : 사용자가 이전에 구매한 상품 중에서 좋아하는 상품들과 유사한 상품들을 추천하는 방법

<br>

**과정** : 아이템 -> 벡터 -> 벡터들 간 유사도 계산 -> 벡터1부터 N까지 자신과 유사한 벡터를 추출

<br>

**Represented Items** : Items을 벡터 형태로 표현, 도메인에 따라 다른 방법이 적용

* 자연어 관련 벡터 표현 방법: BERT, Word2Vec, TF-IDF, Countervectorizer

* 이미지: CNN, VGG...

<br>

### 유사도 함수

**1. 유클리디안 유사도 :** 문서간의 유사도를 계산

* 유클리디안 유사도 = 1/(유클리디안 거리+1e-05)

$$
\norm{p-q} = \sqrt{(p-q)\sdot (p-q)} = \sqrt{\norm{p}^2 + \norm{q}^2 - 2p\sdot q}
$$



```python
from sklearn.metrics.pairwise import euclidean_distances

euclidean_distances(countvect_df, countvect_df + 1e-5)
```

<br>

**장단점**

* 장점: 계산하기 쉬움. 
* 단점: p와 q의 분포가 다르거나 범위가 다른 경우에 상관성을 놓침

<br>

**2. 코사인 유사도** : 문서 간의 유사도를 계산. 많이 쓰임
$$
similarity = cos(\theta) = \frac{A \sdot B}{\norm{A}\norm{B}} = \frac{\sum_{i=1}^n A_iB_i}{\sqrt{\sum_{i=1}^{n}A_i^2}\sum_{i=1}^{n}B_i^2}
$$
예시

|      | 과일이 | 길고 | 노란 | 먹고 | 바나나 | 사과 | 싶은 | 저는 | 좋아요 |
| ----- | ------ | ---- | ---- | ---- | ------ | ---- | ---- | ---- | ------ |
| 문서1 | 0      | 0    | 0    | 1    | 0      | 1    | 1    | 0    | 0      |
|문서2|0|0|0|1|1|0|1|0|0|

문서 1과 문서2의 유사도
$$
\frac{1+1}{\sqrt{1^2+1^2+1^2} \sdot \sqrt{1^2+1^2+1^2}}=0.6667
$$

```python
from sklearn.metrics.pairwise import cosine_similarity

cosine_similarity(countvect_df, countvect_df)
```





<br>

**장단점**

* 장점: 벡터의 크기가 중요하지 않은 경우에 거리를 측정하기 위한 메트릭으로 사용.(문서 내에서 단어의 빈도수 - 문서들의 길이가 고르지 않더라도 문서내에서 나온 횟수의 비율을 확인하기 때문에 상관없음)
* 단점: 벡터의 크기가 중요한 경우에 대해서 잘 작동하지 않음 <-> 유클리디안



<br>

**3. 피어슨 유사도** : 문서간의 유사도 계산. 보통 상관계수 구할 때
$$
\gamma_{XY} = \frac{\sum^n_i(X_i-\bar{X})(Y_i-\bar{Y})}{\sqrt{\sum_i^n}(X_i-\bar{X})^2 \sqrt{\sum^n_i(Y_i-\bar{Y})^2}}
$$


예시 : 코사인유사도와 동일한 문서1,2
$$
\frac{1+1}{\sqrt{6(\frac{1}{3})^2+3(\frac{2}{3})^2} \sdot \sqrt{6(\frac{1}{3})^2+3(\frac{2}{3})^2}}=0.5
$$

```python
import pandas as pd
pearson_sim = countvect_df.T.corr()
np.array(pearon_sim)
```



**4. 자카드 유사도**: 문서 간 유사도 계산. 집합들끼리 얼마나 겹치는지 계산


$$
J(A,B) = \frac{\abs{A \cap B}}{\abs{A \cup B }} = \frac{\abs{A \cap B}}{\abs{A}+\abs{B}-\abs{A\cap B}}
$$


```python
from sklearn.metrics.pairwise import pairwise_distances
jac_sim = 1 - pairwise_distances(np.array(countvect_df),metric='jaccard')
jac_sim
```



그 외: Divergence, Dice, Sorenson 유사도 등 있음. 고객 집단별 다른 유사도 함수 



---

## 자연어 데이터를 벡터로 표현하는 방법



### **1. TF-IDF**

: TF-IDF는 특정 문서 내에 특정 단어가 얼마나 자주 등장하는지를 의미하는 단어 빈도(TF)와 전체 문서에서 특정 단어가 얼마나 자주 등장하는지를 의미하는 역문서 빈도(IDF)를 통해서 "다른 문서에서는 등장하지 않지만 특정 문서에서만 자주 등장하는 단어"를 찾아서 문서 내 단어의 가중치를 계산하는 방법.

* 문서의 핵심어 추출, 문서들 사이의 유사도 계산, 검색 결과의 중요도 정하는 작업에 활용

* 개념
  * TF(d, t) : 특성 문서 d에서의 특정 단어 t의 등장 횟수
  * DF(t): 특정 단어 t가 등정한 문서의 수
  * IDF(d,t): DF(t)에 반비례하는 수. log(n/1+df(t))

$$
TF(d,t) * IDF(d,t) = TF-IDF(d,t)
$$

<br>

**예시**

<br>

| 문서 | 내용           |
| ---- | -------------- |
| 0    | 먹고 싶은 사과 |
|1| 먹고 싶은 바나나|
|2| 길고 노란 바나나 바나나|
|3| 저는 과일이 좋아요|

IDF 값 계산

|      | 과일이 | 길고 | 노란 | 먹고 | 바나나 | 사과 | 싶은 | 저는 | 좋아요 |
| ---- | ------ | ---- | ---- | ---- | ------ | ---- | ---- | ---- | ------ |
| 총합 | 1      | 1    | 1    | 2    | 2      | 1    | 2    | 1    | 1      |

| 단어   | IDF(역문서빈도)     |
| ------ | ------------------- |
| 과일이 | ln(4/(1+1)) = 0.693 |
|길고| ln(4/(1+1)) = 0.693|
| ... | ... |

최종 TF*IDF값 매트릭스

|       | 과일이 | 길고 | 노란 | 먹고   | 바나나 | 사과     | 싶은   | 저는 | 좋아요 |
| ----- | ------ | ---- | ---- | ------ | ------ | -------- | ------ | ---- | ------ |
| 문서1 | 0      | 0    | 0    | 0.2876 | 0      | 0.693147 | 0.2876 | 0    | 0      |
|...|...|...|...|...|...|...|...|...|...|



**장단점:** 

* 장점: 
  * 많은 문서에 등장하는 단어는 중요한 단어가 아니기에 패널티를 줄 수 있음
  * 직관적인 해석이 가능함
* 단점: 
  * 대규모 말뭉치를 다룰 때 메모리상의 문제 
    - 높은 차원을 가짐. 매우 sparse한 형태의 데이터 -> word2vec으로 해결
  * oov 문제

<br>

**vectorizer**

Countvectorizer : 각 텍스트에서 단어 출현 횟수를 카운팅한 벡터

TfidfVectorizer : 전체 문서에서 자주 나오는 의미없는 단어에 패널티 줌 (라이브러리마다 계산식 조금씩 다를 수 있음)

HashingVectorizer: counvectorizer에서 해시 함수를 통해 속도를 높임

<br>

### 전처리: 형태소 처리, 불용어 처리

해주는 것이 좋음

<br>

### **2.Word2Vec(CBOW, Skip-gram)**

**통계기반 방법의 단점**

<br>

* 대규모 말뭉치를 다룰 때 메모리상의 문제
  * 높은차원을 가짐. 매우 sparse한 데이터 형태
  * 한번에 학습 데이터 전체를 진행함: 병렬처리 어렵. 큰 작업 처리 어렵
  * 학습을 통해서 개선하기가 어려움.

<br>

**추론기반의 방법(Word2Vec)**

추론: 주변 단어(맥락)이 주어졌을 때 빈 공간에 무슨 단어(중심단어)가 들어가는지를 추측하는 작업. 학습을 통해 성능 개선 가능

* CBOW 모델
* Skip-gram 모델

<br>

**Word2Vec** : 단어 간 유사도를 반영하여 단어를 벡터로 바꿔주는 임베딩 방법론. 원-핫 벡터 형태의 sparse matrix이 가지는 단점을 해소하고자 저차원의 공간에 벡터로 매핑하는 것이 특징이다. Word2Vect은 비슷한 위치에 등장하는 단어들은 비슷한 의미를 가진다는 가정을 통해서 학습을 진행한다. 저차원에 학습된 단어의 의미를 분산하여 표현하기에 단어 간 유사도를 계산할 수 있다.

> 추천시스템에서는 단어를 구매 상품으로 바꿔서 구매한 패턴에 Word2Vect을 적용해서 비슷한 상품을 찾을 수 있다.



**알고리즘 1: CBOW**

주변에 있는 단어들을 가지고, 중간에 있는 단어를 예측하는 방법



`You say goodbye and I say hello`

-> you [        ] goodbye and I say hello.



주변 단어: 주변에 있는 단어(you, goodbye)



```python
#`You [      ] goodbye and I say hello`
#you = [1,0,0,0,0,0,0]
#goodbye = [0,0,1,0,0,0,0]

import numpy as np
#1. 원핫벡터로 인풋값 정의
input1 = np.array([[1,0,0,0,0,0,0]])
input2 = np.array([[0,0,1,0,0,0,0]])

#(입력 x 차원의 크기) - 차원의 크기는 사용자가 선정 -> sparse 문제 해결
w_in = np.random.randn(7,3) #초기의 weight는 랜덤한 값으로 정해짐
#w_in: 단어의 임베딩 값이 됨


#은닉층
h_1 = np.matmul(input1, w_in) #은닉층의 값 input (1,7) * w_in (7,3)
h_2 = np.matmul(input2, w_in)

h = (h_1+h_2)/2
print(h)
#[[-0.2142566  -0.46789184  0.86361624]]  -> shape (1,3)

#3. hidden state의 값을 w_out과 곱해서 score를 추출
w_out = np.random.randn(3,7)
score = np.matmul(h, w_out)
print(np.round(score, 4))

#4. score에 softmax를 취해서 각 단어가 나올 확률을 계산
def softmax(x):
    exp_x = np.exp(x) #밑이 자연상수 e인 지수함수로 변환 e^x
    sum_exp_x = np.sum(exp_x)
    y = exp_x/sum_exp_x
    return y

pred = softmax(score)
print(np.round(pred, 4))

#5. 정답과 cross entropy loss를 계산
def cross_entropy_error(y,t):
    '''
    y: prediction
    t: target
    '''
    delta = 1e-7
    
    #y.shape[0]으로 나눠주는 이유는 배치 사이즈 반영
    return -np.sum(t*np.log(y+delta))/y.shape[0]
cross_entropy_error(pred, [0,1,0,0,0,0,0])
#loss: 2.10

#6. 5에서 계산한 Loss를 가지고 Backpropagation 과정을 통해 weight 업데이트
ans = [0,1,0,0,0,0,0]
ds = np.round(pred - ans, 4)
print(ds)

## 6-1) w_out
dw_out = np.outer(h,ds) #dw_out의 weight 업데이트
print(np.round(dw_out,4))

## 6-2) w_in
## w_in_new = w_in - learning_rate * dw_in
learning_rate = 1
w_in_new = w_in - learning_rate * dw_1
w_in_new = w_in_new - learning_rate * dw_2
print(np.round(w_in_new, 4))
```

위의 과정을 다른 문맥에 대해서도 수행 <br>

입력: You, goodbye -> 출력: say <br>

입력: say, and -> 출력: goodbye



<br>

**알고리즘 2: Skip-Gram**

중간에 있는 단어로 주변 단어들을 예측하는 방법. 성능이 더 좋아서 많이 쓰임

`You say goodbye and I say hello`

-> [           ] say [           ] and I say hello.



중심 단어: 중간에 있는 단어(say)



*윈도우 크기: 주변을 몇 칸까지 볼 지에 대한 크기

```python
#1. One Hot vector형태의 입력값을 받는다
#-[say] -> [you] 예측
#-[say] -> [goodbye] 예측
#윈도우크기:1
#만약 윈도우 크기 2이면 [say]를 통해 you, goodbye, and를 예측


#2. One Hot Vector 형태의 입력값을 W_in과 곱
# 입력값은 원-핫 백터형태를 가짐
input1 = np.array([[0,1,0,0,0,0,0]]) #say

output1 = np.array([[1,0,0,0,0,0,0]]) #you
output2 = np.array([[0,0,1,0,0,0,0]]) #goodbye

#(입력 x 차원의 크기) - 차원의 크기는 사용자가 선정
##초기의 weight는 랜덤하게 결정됨
w_in = np.random.randn(7,3)

h = np.matmul(input1, w_in) #은닉층의 값
print(h)
#[[1.73,0.25,-1.17]]


#3. hidden state의 값을 w_out과 곱해서 score를 추출
w_out = np.random.randn(3,7)
score = np.matmul(h, w_out)
print(np.round(score, 4))

#4. score에 softmax를 취해서 각 단어가 나올 확률을 계산
def softmax(x):
    exp_x = np.exp(x) #밑이 자연상수 e인 지수함수로 변환 e^x
    sum_exp_x = np.sum(exp_x)
    y = exp_x/sum_exp_x
    return y

pred = softmax(score)
print(np.round(pred, 4))


#5. 정답과 cross entropy loss를 계산
def cross_entropy_error(y,t):
    '''
    y: prediction
    t: target
    '''
    delta = 1e-7
    
    #y.shape[0]으로 나눠주는 이유는 배치 사이즈 반영
    return -np.sum(t*np.log(y+delta))/y.shape[0]
cross_entropy_error(pred, [0,1,0,0,0,0,0])


#6. 5에서 계산한 Loss를 가지고 Backpropagation 과정을 통해 weight 업데이트
#########
##- 두 개의 answer에 대해서 오차를 더함
#output1, output2가 answer임
ds1 = np.round(pred - output1, 4)
ds2 = np.round(pred - output2, 4)
ds = ds1 + ds2
print(ds)

## 6-1) w_out
dw_out = np.outer(h,ds) #dw_out의 weight 업데이트
print(np.round(dw_out,4))


## 6-2) w_in
da = np.dot(ds,w_out.T)
print(np.round(da,4))

dw_in = np.outer(np.array([[0,1,0,0,0,0,0]]), da)
print(np.round(dw_in, 4))


##w_out 업데이트
learning_rate = 1
w_out_new = w_out - learning_rate * dw_out
print(np.round(w_out_new, 4))

#7. 위의 과정을 다른 문맥에 대해서도 수행

```

Skip-gram의 통한 성능이 더 좋은 이유: 더 어려운 테스크를 수행함



[캐글 실습]



<br>

### 컨텐츠 기반 모델의 장단점

* 장점: 
  * 협업필터링은 다른 사용자들의 평점이 필요한 반면에 자신만의 평점을 가지고 추천시스템을 만들 수 있음
  * item의 feature를 통해서 추천을 하기 때문에 추천이 된 이유를 설명하기 용이
  * 사용자가 평점을 매기지 않은 새로운 item이 들어올 경우에도 추천 가능(cold start 가능)

* 단점:
  * item의 feature을 추출해야 하고 이를 통해서 추천하기 때문에 제대로 feature를 추출하지 못하면 정확도가 낮음. 그렇기에 Domain Knowledge가 분석시에 필요할 수도 있음
  * 기존의 item과 유사한 item 위주로만 추천하기에 새로운 장르의 item을 추천하기 어려움
  * 새로운 사용자에 대해서 충분한 평점이 쌓이기 전까지는 추천하기 어려움
