---
layout: post
title: "[Machine Learning]Decision Trees"
date: 2021-02-04
category: [Machine Learning]
MachineLearning : true 
excerpt: "L"
tags: [lo]
comments: true
---





# Random Forest

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



**Bagging**: **B**ootstrapping the data plus using the **ag**gregate to make a decision

배깅: 앙상블 학습법의 메타 알고리즘. 배깅은 분산을 줄이고 과적합을 피하도록 해준다. 결정 트리 학습법이나 랜덤 포레스트에 주로 적용된다. 

[^ ]:StatQuest: Random Forests Part 1- Building, Using and Evaluating



### 결측치 2 종류

1. Missing data in the original dataset used to create the random forest

범주형 데이터: 최빈도값로 채워 넣음

수치형 데이터:  중위수(median)으로 채워 넣음

->평가: 결측치 있는 샘플과 가장 비슷한 샘플을 찾는 과정?

샘플들을 트리에 돌려봄->같은 리프 노드로 분류되는 샘플이 비슷한 샘플임 

proximity matrix(샘플들로 cross table 만든 것)를 만들어서 살펴볼 수도 있음 

트리를 돌려봐서 같은 노드에 분류되면 cross하는 곳에 1을 적음

모든 트리를 돌린 후 얻은 proximity matrix





2. Missing data in a new sample that you want to categorize



