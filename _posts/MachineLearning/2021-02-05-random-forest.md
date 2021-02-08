---
layout: post
title: "[Machine Learning]랜덤 포레스트 모형"
date: 2021-02-04
category: [Machine Learning]
MachineLearning : true 
excerpt: "랜덤 포레스트 모델과 배깅(bagging)"
tags: [random forest, oob, bagging, bootstrap]
comments: true
---



# Random Forest

랜덤포레스트 모델은 앙상블 방법 중 하나로 기본모델(weak base learner)를 만들어 그 모델의 예측결과를 다수결이나 평균을 내어 예측하는 방법이다. 데이터가 선형이든 비선형이든 분류 문제에 사용가능하다.



순서

1. bootstrapped dataset을 만든다
2. bootstrapped dataset을 이용해서 랜덤하게 선택한 특성(복원추출)들로 구성한 subset으로 decision tree를 만든다. 
3. 처음으로 돌아가서 새로운 bootstrap dataset을 만들고(복원추출) 다시 그 subset으로 decision tree를 만든다.
4. 여러 트리를 만든 후에 트리들끼리 투표를 해서 더 많은 표를 받은 쪽으로 결정한다.

이렇게 100회 이상 시행한 다양한 트리로 인해서 랜덤포레스트모형은 결정트리모형보다 더 효과적이다 



이렇게 만든 랜덤포레스트 모형이 정확한지 확인하는 법

5. 한 번도 bootstrapped dataset에 들어가지 못한 샘플들을 out-of-bag dataset으로 만든다.
6. out-of-bag dataset의 샘플들을 위에서 만든 모든 트리들에 넣어서 돌려서 트리들이 정확한 label로 분류하는지 보고, 마찬가지로 트리들끼리 투표해서 더 많은 쪽으로 label한다. 
7. 랜덤포레스트 모형이 out-of-bag dataset의 모든 샘플을 제대로 분류했는지 그 비율로 정확도를 나타낸다. 
8. 제대로 분류하지 못한 비율: out-of-bag error



몇 개의 특성을 써야 할까?

1) 랜덤 포레스트 모형 만들기

2) 랜덤 포레스트 모형의 정확도 평가

3) 1번으로 돌아가서 특성의 개수를 바꿔서 다시 시행

4) 보통 모든 특성의 개수의 제곱근의 개수로 시행하고 그 상하로 바꿔가며 테스트해서 가장 최적의 개수를 찾는다



**Bagging**: **B**ootstrapping(무작위 복원추출) the data plus using the **ag**gregate to make a decision

배깅: 앙상블 학습법의 메타 알고리즘. 배깅은 분산을 줄이고 과적합을 피하도록 해준다. 결정 트리 학습법이나 랜덤 포레스트에 주로 적용된다. 

[^ ]:StatQuest: Random Forests Part 1- Building, Using and Evaluating



트리모델을 쓸 때는 원핫인코딩보다 ordinal encoding을 사용하는 것이 더 좋다

중요한 범주형 특성을 원핫인코딩을 하게 되면 상위 노드에서 채택될 확률이 떨어지게 된다.

ordinal encoding을 명목변수에 하면 순서가 생기지만 트리모델에서는 상관이 없기 때문에 해도 된다



### 결측치 2 종류

1. Missing data in the original dataset used to create the random forest

범주형 데이터: 최빈도값로 채워 넣음

수치형 데이터:  중위수(median)으로 채워 넣음

->평가: 결측치 있는 샘플과 가장 비슷한 샘플을 찾는 과정?

범주형 데이터:

샘플들을 트리에 돌려봄->같은 리프 노드로 분류되는 샘플이 비슷한 샘플임 



수치형 데이터:

proximity matrix(샘플들로 cross table 만든 것)를 만들어서 살펴볼 수도 있음 

트리를 돌려봐서 같은 노드에 분류되면 cross하는 곳에 1을 적음

모든 트리를 돌린 후 얻은 proximity matrix

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/img/proximity_matrix1.png?raw=true" alt="proximity matrix1" style="zoom:67%;" />

이를 트리의 개수로 나눠서 비율로 변환함

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/img/proximity_matrix2.png?raw=true" alt="proximity matrix2" style="zoom:67%;" />

이들의 가중평균을 구함 
$$
weighted\;average = value \;of\;a\; sample_1 *\frac{weighted\;average\;of\;a\;column}{sum \;of\;proximities}+ value \;of\;a\; sample_2 *\frac{weighted\;average\;of\;a\;column}{sum \;of\;proximities}+...+value \;of\;a\; sample_n *\frac{weighted\;average\;of\;a\;column}{sum \;of\;proximities}
$$


샘플 4가 결측치 일 때 샘플1의 가중평균은
$$
weighted \;average \;of\; sample1 = 샘플1의 \;값 *\frac{0.1}{0.1+0.1+0.8}
+샘플2의\;값*\frac{0.1}{0.1+0.1+0.8}+샘플3의 \;값 *\frac{0.8}{0.1+0.1+0.8}
$$


proximity matrix를 distance matrix로 변환 후 히트맵을 그릴 수 있음

MDS를 그릴 수도 있음






 2. Missing data in a new sample that you want to categorize

범주형 데이터의 결측치 처리

1)레이블이 0과 1만 있다고 할 때, 결측치가 있는 test 데이터의 샘플을 각각 레이블 0과 1을 단 두 개의 데이터셋으로 만든다

2)레이블이 0일 때 결측치를 범주1로 달지 범주2로 달지 위의 방법으로 결정, 레이블이 1일 때 결측치를 범주 1로 할지 범주 2로 할 지 결정(설명 편의를 위해 결측치 있는 특성의 범주는 2가지)

레이블이 0일 때 범주1, 레이블이 1일 때 범주 2를 달게 되었다고 하자

3)위에서 쓴 방법으로 트리들이 범주1, 레이블0으로 투표한 수와 범주2, 레이블1로 투표한 수를 비교해서 결측치가 있는샘플을 분류한다



Sklearn

1) train 데이터를 훈련/검증 세트로 분리

```python
from sklearn.model_selection import train_test_split
train, val = train_test_split(train, train_size = 0.8, test_size = 0.2, stratify= train['target'], random_state =1) 
#stratify는 classification 문제를 불 때 중요한 옵션값이다. 지정하지 않으면 특정 label이 train이나 val에 쏠려서 분배될 수 있어 모델 성능 차이가 많이 나게 된다.
```

