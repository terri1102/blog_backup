---
layout: default
title: "9. Independence, Basis, and Dimension"
date: 2021-10-11
comments: true
nav_order: 9
parent: Math
use_math: true
---

<br>

{:toc} 



# **이번 강의의 목표**

- Linear Independence
- Spanning a space
- Basis and dimension 개념 배우기



# 1. Independence

 간단한 예시를 통해서 선형 독립이라는 개념을 알아볼 것이다. A 행렬은 (mxn)의 크기를 갖는 행렬이라고 가정해보자(단, m < n). 그렇다면 Ax=0에는 영벡터가 아닌 해가 존재한다. 이때 방정식의 개수보다 미지수의 개수가 더 많다. (미지수 xi는 n개인데, A의 행(방정식)은 m개이기 때문에) 그렇다면 A의 영공간에 0이 아닌 해가 있다는 결론을 내릴 수 있다. 따라서 RREF(A)의 결과로 나온 행렬이 적어도 하나(사실상 n-m개)의 free variable을 가질 것이다. 



그렇다면 여러 벡터들이 서로 (선형)독립적이라는 것은 무슨 의미일까? 일단 추상적인 정의를 살펴보자.

> Abstract definition of Independence: Vectors x1, x2,...,xn are independent if no combination gives zero vector except the zero combinations. (zero combination : 모든 ci가 0인 결합)

$$
c_1x_1+c_2x_2 + \cdots+ c_nx_n \ne 0 
$$



벡터 x1, x2, ..., xn의 결합을 했을 때 영벡터를 가진다면 이는 종속적이다. 



<p align="center">

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/math/vectors.jpg?raw=true" alt="vectors" style="zoom:110%;" />

</p>



위의 그림에서 두 벡터의 선형결합
$$
V2 - 2V1 = 0
$$
으로 0을 만들 수 있기 때문에 두 벡터는 선형종속적이다. 

만약 V2가 원점일 때 두 벡터는 독립적일까? V1에 0을 곱해서 영벡터를 만들 수 있기 때문에 역시 두벡터는 종속적이다. 
$$
0 * V1 + c * V2 = 0
$$


그럼 이제 2차원 공간에서 벡터가 세 가지인 경우도 생각해보자. 


$$
A= \begin {bmatrix} 2 & 1 & 2.5 \\ 1 & 2 & -1 \end {bmatrix}
$$


이 벡터들은 서로 독립적일까 아니면 종속적일까? 2차원 공간에 벡터가 세 개가 존재하기 때문에 종속적이다. 이는 세 벡터의 선형결합으로 영벡터를 만들 수 있다는 의미이다.


$$
\begin {bmatrix} 2 & 1 & 2.5 \\ 1 & 2 & -1 \end {bmatrix} \begin {bmatrix} c_1 \\ c_2 \\ c_3\end {bmatrix} = \begin {bmatrix} 0 \\ 0\end {bmatrix}
$$


이제 구체적인 정의를 보면, 행렬 A의 열벡터들의 독립여부를 살펴보기 위해선 영공간을 봐야한다. 

> They are `independent` if nullspace of A is only the zero vector. They are `dependent` if Ac= 0 for some non zero c.



따라서 벡터들이 독립인 경우 rank는 n이고, N(A) = {0}이며, free variable을 가지지 않는다. 반면 벡터들이 종속인 경우 rank는 n보다 작고 free variable를 가진다.



# 2. Spanning a space

Spanning a space는 무슨 의미일까? 사실 우리는 이미 이 개념을 열공간에서 접한 적이 있다. 행렬의 열들에 대해 이들의 모든 결합을 구하면 열공간이 나오고, 행렬의 열들은 이 열공간을 span한다.

이를 정리하자면 벡터 v1, ..., vl이 어떤 공간을 span한다는 것은 그 공간은 벡터들의 모든 결합으로 구성되어있다는 것을 의미한다.

> Vectors V1,...,Vl spanning a space means the space consists of all combinations of those vectors.



이제 우리가 관심있는 것은 Set of vectors that span a space and is independent. 이는 우리가 적절한 개수의 벡터를 가진다는 것을 의미한다. Basis(기저) 개념을 이용해서 이를 정리해보자.



# 3. Basis and Dimension

> Basis for a space is a sequence of vectors V1, V2, ...,Vd

즉 기저는 공간(space) 안에 있는 여러 벡터들이 두 가지 속성을 가지는 것을 나타낸다.

1. 벡터들은 서로 독립적이다.
2. 벡터들은 공간(space)을 span한다

3차원 공간의 벡터들로 예를 들어보자. (Space is 
$$
\mathbb{R}^3
$$
) 

하나의 기저는 


$$
c1= \begin {bmatrix} 1 \\ 0 \\ 0 \\ \end {bmatrix}, c2= \begin {bmatrix} 0 \\ 1 \\ 0 \\ \end {bmatrix}, c3 = \begin {bmatrix} 0 \\ 0 \\ 1 \\ \end {bmatrix}
$$


이다. 위의 벡터들을 선형결합해서 영벡터를 만들 수 있을까? 이 세 벡터들은 선형독립이기 때문에 zero combination을 제외하고서는 불가능하다.

위의 벡터들을 3x3 단위행렬로 두고 생각해보면, 단위행렬의 영공간은 무엇인가? 단위행렬의 영공간은 오직 영벡터이다. 그렇기 때문에 이 행렬의 열들은 독립적이다. 



3차원 공간의 다른 기저를 살펴 보자.


$$
\begin {bmatrix} 1 \\ 1 \\2 \end {bmatrix}, \begin {bmatrix} 2 \\ 2 \\5 \end {bmatrix}
$$


이 두 벡터들은 서로 선형독립이지만 
$$
\mathbb{R}^3
$$
를 span하지는 않는다. 이는 
$$
\mathbb{R}^3
$$
의 어떤 벡터는 두 벡터의 선형결합에 포함되지 않는다는 의미이다. 두 개의 벡터는 3차원 공간의 `plane`을 span한다. 그들은 독립적이기 때문에 plane의 기저이다. 만약 (3,3,7) 벡터를 추가한다면 여전히 plane을 span하겠지만 독립적이지 않기 때문에 기저가 될 수 없다. 

그럼 여기에 기존의 벡터와 독립인 벡터를 한 개 더 추가한 경우를 살펴보자.


$$
\begin {bmatrix} 1 \\ 1 \\2 \end {bmatrix}, \begin {bmatrix} 2 \\ 2 \\5 \end {bmatrix}, \begin {bmatrix} 3 \\ 3 \\8 \end {bmatrix}
$$


위의 벡터들을 행렬로 만든 후에 RREF형태로 만들면 free variable을 가지게 될까? 

n개의 벡터가 기저인 조건: 

> n vectors give basis if the nxn matrix with those columns is invertible.



기저는 한 개가 아니다. 어떤 invertible한 3x3 행렬의 열을 골라도 열공간은 3차원이고, 독립적이며 
$$
\mathbb{R}^3
$$
의 기저이다.

그럼 여러 기저들끼리 서로 공유하는 성질은 무엇일까? 다양한 행렬들이 
$$
\mathbb{R}^3
$$
의 기저가 될 수 있지만 벡터의 개수가 같아야 한다. 즉 n차원의 기저가 되려면 n개의 벡터를 가져야 한다. 

> Every basis for the space has the same number of vectors. 

그럼 n은 뭐라고 부를까? Dimension이다. 위의 문장을 다르게 쓰면 모든 space의 기저는 같은 개수의 벡터(space의 차원수: n)를 가진다.



여태까지 배운 것들을 정리해보자.

1 Independence : that looks at combination not being zero.

2 Spanning : that looks at all the combinations.

3 Basis : that's the one that combines independence and spanning.

4 Dimension of a space : the number of vectors in any basis. 



새로운 예제로 이해해보자. A 행렬의 공간(space)는 C(A)이고 A는 아래와 같다.


$$
A = \begin {bmatrix} 1 & 2 & 3 & 1\\ 1 & 1 & 2 & 1 \\ 1 & 2 & 3 & 1\end {bmatrix}
$$


이때 A의 벡터들은 A의 열공간을(C(A)) span하는가? 그렇다. 열공간의 정의상 A의 벡터들은 열공간을 span한다. 하지만 벡터들은 열공간의 기저를 이루는가? 즉 벡터들끼리 독립적인가? 아니다.  Ax= 0인 A의 영공간을 살펴보자. 


$$
N(A) = \begin {bmatrix} 1 \\ 1 \\ -1 \\0 \end {bmatrix}
$$


영공간에 0이 아닌 해가 있기 때문에... 위의 벡터는 A의 영공간에 속한다. 그렇다면 A의 열공간의 기저는 무엇일까? 많은 답이 있겠지만, 가장 자연스러운 답은 column1, column2이다. 그래서 A행렬의 rank는 2이다. 이를 다시 말하면 
$$
rank(A) = number \; of \;pivot \;columns = dimension \; of \; the \; C(A) = 2
$$


이다. 이때 `행렬 A`의 Rank와 `행렬 A의 부분공간인 열공간`의 dimension을 말하는 것임에 주의하자. 

그럼 column1과 column2 말고 A의 열공간의 다른 기저를 찾아볼 수 있을까? column1과 column3도 기저이고, column2와 column3도 기저이고... 많은 기저가 있을 것이다. 

그럼 아래의 벡터들은 어떨까?


$$
\begin {bmatrix} 2 \\ 2 \\ 2 \end {bmatrix}, \begin {bmatrix} 7 \\ 5 \\ 7 \end {bmatrix}
$$


만약 우리가 Dimension of the C(A)를 알고, 독립적인 벡터들을 Dimension of the C(A)만큼 고른다면 자동적으로 기저가 된다. 

영공간의 경우 
$$
Dim \; C(A) = r
$$
이기에 영공간에도 벡터가 Dim C(A) 개 있고 독립적이면 기저가 된다.


$$
A = \begin {bmatrix} 1 & 2 & 3 & 1 \\ 1 & 1 & 2& 1 \\ 1 & 2 & 3&1 \end {bmatrix}, N(A) = \begin {bmatrix} -1\\-1\\1\\0\end {bmatrix}, \begin {bmatrix} -1\\0\\0\\1\end {bmatrix}
$$


위의 두 개의 벡터는 영공간을 span하는가? 즉 영공간은 두 벡터의 모든 결합으로 구성되어있는가? 그렇다. 이를 통해 영공간의 차원은 n - r이라는 것을 알 수 있다. 

> dimension of the null space is the number of free variables = n - r

###### n: 행렬의 열의 개수

###### r: rank



# Reference

[MIT 18.06 Linear Algebra, Spring 2005](https://www.youtube.com/watch?v=yjBerM5jWsc&list=PLE7DDD91010BC51F8&index=10&t=9s)

