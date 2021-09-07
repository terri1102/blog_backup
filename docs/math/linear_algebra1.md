---
layout: default
title: "1. The Geometry of Linear Equations"
date: 2021-09-06
comments: true
nav_order: 1
parent: Math
use_math: true
---





[Linear algebra 수업](https://www.youtube.com/watch?v=J7DzL2_Na80&list=LL&index=6&t=125s)을 듣고 정리하는 디렉토리

교재: Introduction to Linear Algebra



## n개의 linear equation과 n개의 unknown를 바라보는 3가지 관점

1. Row picture
2. Column picture*
3. Matrix form



이번 수업에서는 선형방정식과 행렬식을 바라보는 3가지의 관점에 대해서 배웠다. 먼저 2개의 미지수와 2개의 선형 방정식의 예시를 보면서 행으로 접근하는 것과 열로 접근하는 것에 대해서 생각해보자.


$$
2x -y  =0 \\
-x + 2y = 3
$$
이를 행렬로 표현해보면 
$$
\begin {bmatrix} 2 & -1 \\
				-1 &  2\end {bmatrix} \begin {bmatrix} x \\ y\end {bmatrix} = \begin {bmatrix} 0 \\ 3 \end {bmatrix}
$$
이다. 여기서 앞의 행렬을 A, 벡터를 x, 결과는 b로 표현할 수 있다. Ax = b 이렇게 Matrix form으로 보기 전에 Row picture과 Column picture의 차이에 대해 알아보자.



### 1. Row picture의 관점

![rowpicture1](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/math/rowpicture1.jpg?raw=true)



Row picture로 두 개의 선형방정식을 푸는 것은 두 선을 그려서 선이 만나는 점을 해로 구하면 된다.



### 2. Column picture의 관점

![columnpicture2](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/math/columnpicture2.jpg?raw=true)

linear combination of the columns

이때 x,y의 다른 결합을 찾을 수 있을까? x,y의 조합은 전체 스페이스를 채운다.



이렇게 2차원일 때는 두 가지 관점 모두 쉬워 보인다. 하지만 차원이 늘어갈수록 row picture를 통해서 해를 구하는 것은 어려워진다.





2. Column picture

보통은 elimination(소거법)으로 계산한다. 
$$
x\begin {bmatrix} 2 \\ -1 \\ 0 \end {bmatrix}+ y \begin {bmatrix} -1\\ 2 \\ -3 \end {bmatrix}+z\begin {bmatrix} 0 \\ -1 \\4 \end {bmatrix} = \begin {bmatrix} 1 \\ 1 \\-3 \end {bmatrix}
$$

> "Can I solve Ax = b for every b?" "Do the linear combinations of the columns fill 3-d space?"

여기서 A에 대한 답은 yes이다. 따라서 A는 non-singular matrix이고 invertible matrix이다.



만약 3개의 column이 같은 plane에 있다면 b를 만들 수 없다. 이 경우 세 개의 column의 결합으로 새로운 값을 만들어내지 못하기 때문에.

따라서 A는 singular case, not invertible하다.



 8th dimensional plane inside 9th dimensional space 등 앞으로 이런 경우를 더 배우게 될 것.



3. Matrix form

$$
Ax=b
$$

행렬과 벡터는 어떻게 곱할까? 2가지의 방법이 있다.

1. Column 
2. Row - dot product의 아이디어

앞으로 Ax의 의미는 Combination of the columns of A로 받아들이기



### Glossary

* coefficient matrix(계수 행렬) : 행렬 방정식 Ax = b에서 A를 행렬 방정식의 계수 행렬이라고 한다. [출처: 위키피디아](https://ko.wikipedia.org/wiki/%EC%B2%A8%EA%B0%80_%ED%96%89%EB%A0%AC)

* invertible matrix(가역 행렬) : non-singular matrix(비특이행렬), regular matrix(정칙 행렬): 그와 곱한 결과가 단위 행렬인 행렬을 갖는 행렬  [출처: 위키피디아](https://ko.wikipedia.org/wiki/%EA%B0%80%EC%97%AD%ED%96%89%EB%A0%AC) 수업에서는 방정식 Ax=b의 해가 존재하며, b의 값과 무관하게 항상 유일하다는 의미로 쓰인듯.



소감: 오늘 처음으로 강의 들었는데 막연하게 알고있던 선형대수에 대한 concrete한 관점을 제시하는 강의였던 것 같다. 강의 듣고 책 보니까 진도를 2.1까지 나간듯.

