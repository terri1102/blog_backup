---
layout: default
title: "[비지도학습] Clustering의 K-Means"
date: 2021-01-17
category: [Data Science]
parent: Machine Learning
nav_order: 3
excerpt: "K-Means clustering 소개"
tags: [clustering, k-means, elbow method ]
comments: true
---



`Machine Learning`은 데이터의 특징에 따라 예측하거나 분류하는 작업을 한다. `지도학습(Supervised Learning)`에는 분류, 회귀 알고리즘이 있고, `비지도 학습(Unsupervised Learning)`에는 클러스터링, 차원 축소, 연관 규칙 학습, 강화 학습 등이 있다.



**Clustering:** 비지도학습 중 하나로 주어진 데이터들이 얼마나 유사한지 판단하는 것.
**Clustering 종류**: Hierarchical(Agglomerative, Divisive)/ Point Assignment



---

# K- Means Clustering

**K-Means 군집화 알고리즘**:  K는 cluster의 수를 의미. Means은 각 클러스터의 중심과 데이터들의 평균 거리를 의미

**[1단계] 표본 공간에 k개의 중심을 무작위로 생성**

```python
import numpy as np

k=3              #군집의 개수
choice1 = np.random.choice(X[:,0], k)   #X: 훈련데이터  #데이터 셋에서 k개를 무작위 선택 후, 중심의 x축 좌표로 지정
choice2 = np.random.choice(X[:,0], k)          #데이터 셋에서 k개 무작위 선택 후 중심의 y좌표로 지정
list_choices = np.array(list(zip(choice1, choice2))) #zip: 같은 자리끼리 묶어서 array만들어줌 [33, 23], [30,2], [7, 89]
#print(list_choices)
```



**[2단계] 각 표본에 가까운 중심에 할당**

```python
#1. 거리 계산하는 함수
def Distance(a, b):
    return np.sqrt(np.sum(np.power(a-b), 2))) #유클라디안 거리

#2. 각 군집의 중심 새롭게 계산
old = np.zeros(list_choices.shape)  #중심의 좌표와 동일한 크기의 행렬 선언
clusters = np.zeros(len(X))         #cluster 레이블 저장 위한 행렬 선언, 훈련데이터와 같은 개수
flag = Distance(list_choices, old)  #반복문의 종료 기준이 될 flag 변수 정의. 중심좌표 간의 거리를 계산해 최종적으로 0이 될 때까지 계산 반복
print(old)
print(flag)
```



**[3단계] 각 군집의 중심을 새롭게 계산**

```python
from copy import deepcopy

dist = []
while flag !=0:
    for i in range(len(X)):
        for j in range(3):
            temp = Distance(X[i], list_choices[j])
            dist.append(temp)
        cluster = np.argmin(dist)
        clusters[j] = cluster
        dist = []
        
    old = deepcopy(list_choices)   #이전 중심과 새로운 중심 비교를 위해 복사
    
    for i in range(k):             #k=3이니 세 번 반복
        points = [X[j] for j in range(len(X) if clusters[j] == i) 
        #군집의 중심별로 데이터 모아서 리스트에 저장
        list_choices[i] = np.mean(points)   #리스트 points 값의 평균 계산->centroid의 x,y 값
                  
     flag = Distance(list_choices, old)   #거리차가 있다면 다시 반복

```



**[4단계] 시각화**

```python
import matplotlib.pyplot as plt
#1번 군집
plt.scatter(X[clusters == 0,0], X[clusters == 0,1], s =50, c= 'red', marker= 'o', edgecolor = 'black', label = 'A')
#2번 군집
plt.scatter(X[clusters == 1,0], X[clusters == 1,1], s =50, c= 'yellow', marker= 'o', edgecolor = 'black', label = 'B')
#3번 군집
plt.scatter(X[clusters == 2,0], X[clusters == 2,1], s =50, c= 'blue', marker= 'o', edgecolor = 'black', label = 'C')

#centroids
plt.scatter(list_choices[:,0], list_choices[:, 1], s = 250, marker = '*', c = 'black', edgecolor= 'black', label= 'centroids')

plt.legend()
plt.grid()
plt.show()

```

---



**Sklearn으로 간편하게 하기**

```python
from sklearn.cluster import KMeans 

kmeans = KMeans(n_clusters = 3)
kmeans.fit(df)
labels = kmeans.labels_
#print(labels)                       #0,1,2로 군집화된 값들 리스트로 나옴

new_series = pd.Series(labels)
df['clusters'] = new_series.values
df                                  #기존 df에 cluster column 생기고 그 안에 label 값들이 채워짐      
```

------



## K-Means 에서 K를 결정하는 방법(Eyeball, metrics)

### Elbow methods

```python
ss = [] #pca할 때 나왔던 ss임
K = range(1,15)
for k in K:
    km = KMeans(n_clusters = k)
    km = km.fit(points)
    ss.append(km.inertia_)   #ss를 리스트에 추가함. 작을수록 좋은 것
    
plt.plot(K, ss, 'bx-')
plt.show()
```

---



## Hierarchical Clustering

:algorithm that builds hierarchical clusters

1. each components are assigned by its own cluster
2. based on the distance, close components are bind into one cluster
3. repeat it until there is only one cluster



**K-means와 Hierarchical clustering 차이**

HCA은 빅 데이터를 잘 다루지 못한다. time complexity가 높아서

HCA의 clustering은 reproducible

K means은 주로 구형 클러스터와 잘 어울림



[^ ]: https://joernhees.de/blog/2015/08/26/scipy-hierarchical-clustering-and-dendrogram-tutorial/#Perform-the-Hierarchical-Clustering