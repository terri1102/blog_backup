---
layout: default
title: "10. The Four Fundamental Subspaces"
date: 2021-10-16
comments: true
nav_order: 10
parent: Math
use_math: true
---

<br>

{:toc} 



# **이번 강의의 목표**

- 열공간과 영공간 외에 2개의 subspace 더 배우기



# 1. 4 Subspaces

## 4 Subspaces 

* Column space : C(A)
* Null space : N(A)
* Row space : R(A) 라고 하지 않고, 여태까지 그래왔던 것처럼 열기준으로 생각 하기 위해 A Transpose의 열공간이라고 생각할 것이다.  C(A^T)
* Null space of A Transpose : N(A^T). The left null space of A



그럼 mxn 행렬인 A행렬의 부분 공간의 차원을 알아보자. 

N(A)는 n개의 요소를 가진 벡터이다. 따라서 
$$
\mathbb{R}^n
$$
에 속한다. C(A)의 경우 
$$
\mathbb{R}^m
$$
에 속하게 된다.


$$
C(A^T) \; in \;  \mathbb{R}^n
$$

$$
N(A^T) \; in \; \mathbb{R}^m
$$



### 4 Subspaces

이들을 이해하기 위해 그들의 basis, dimension을 알아보자.

basis: pivot columns

dimension : rank r


$$
\begin {bmatrix} 1 & 2 & 3 \\ 1 & 2 & 3 \\ 2 & 5 & 8 \end {bmatrix}
$$
dimension은 2.

세 열벡터들은 종속적이다.



|           | C(A)          | N(A)                       | C(A^T) | N(A^T) |
| --------- | ------------- | -------------------------- | ------ | ------ |
| basis     | pivot columns | special solutions          |        |        |
| dimension | r(rank)       | n - r(# of free variables) | r      | m-r    |


$$
\begin {bmatrix} 1 & 2 & 3 &1\\1 & 1 &2& 1\\ 1 & 2 & 3 &1 \end {bmatrix} \rightarrow \begin {bmatrix} 1 & 2 & 3 & 1 \\ 0 & 1 & 1 & 0 \\ 0 & 0 & 0 & 0 \end {bmatrix}\rightarrow \begin {bmatrix} 1 & 0 & 1 & 1 \\ 0 & 1 & 1 & 0 \\ 0 & 0 & 0 & 0 \end {bmatrix}= \begin {bmatrix} I & F \\ zero & vectors \end {bmatrix} = R
$$


이렇게 R로 reduce한 후의 열공간은 A의 열공간과 달라질 수 있다.

row operation을 했을 때 row space는 보존된다. 하지만 열공간은 보존하지 않기 때문에 달라진다.
$$
C(R)\neq C(A)
$$


Basis for row space is first r rows of R



A가 R의 벡터들의 조합이라는 사실을 어떻게 알까? row reduction을 반대로 시행하면 된다. 



이제 Null space of A transpose를 알아보자. 


$$
A^Ty=0
$$

$$
\begin {bmatrix}& & & & \\ & & & & \\&&&& \end {bmatrix} \begin {bmatrix}  \\ \\  \\ \\ \end {bmatrix} = \begin {bmatrix}  0\\ 0\\ 0\\  0\\ \end {bmatrix}
$$
의 꼴이라고 할 때


$$
y^TA = 0^T
$$

$$
\begin {bmatrix} &  y^T &\end {bmatrix} \begin {bmatrix} &&\\ &A&\\&& \\ \end {bmatrix} = \begin {bmatrix} & 0& \end {bmatrix}
$$


이를 가우스조던 소거법을 ..


$$
rref \begin {bmatrix} A_{m \times n} I_{m \times n} \end {bmatrix} \rightarrow \begin {bmatrix} R_{m \times n}  \end {bmatrix}
$$




# Reference

[MIT 18.06 Linear Algebra, Spring 2005](https://www.youtube.com/watch?v=yjBerM5jWsc&list=PLE7DDD91010BC51F8&index=10&t=9s)

