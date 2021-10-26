---
layout: default
title: "11. Matrix Spaces; Rank 1; Small World Graphs"
date: 2021-10-24
comments: true
nav_order: 11
parent: Math
use_math: true
---

<br>

{:toc} 



# **이번 강의의 목표**

- 새로운 벡터 공간의 기저
- Rank가 1인 행렬
- Small world graphs



## 1. Vector Space

M = 모든 3x3 행렬

스칼라값을 곱하거나 행렬끼리 더하는 경우 3x3 행렬의 공간 안에 속해있음.

Basis for M = all 3x3's ->9개 있음. 즉 dimension은 9개


$$
\begin {bmatrix} 1& 0&0\\0&0&0\\0&0&0\end {bmatrix}, \begin {bmatrix} 0& 1&0\\0&0&0\\0&0&0\end {bmatrix}, \begin {bmatrix} 0& 0&1\\0&0&0\\0&0&0\end {bmatrix}, \ldots, \begin {bmatrix} 0& 0&0\\0&0&0\\0&0&1\end {bmatrix}
$$


그럼 부분 공간은 어떨까? 3x3 대칭 행렬의 부분 공간이나 3x3 상삼각행렬에 스칼라값을 곱하거나 행렬끼리 더하는 경우 같은 부분 공간에 속해 있음 (행렬끼리 곱하는 것 제외)

그렇다면 그 부분공간의 basis와 dimension은 뭘까? 위의 original basis 중에 몇 개나 부분 공간의 basis에 들어갈까? 

대칭행렬의 경우 위의 행렬들 중 3개가 basis에 들어간다. 그 외에도 3개 있어서 총 6개 있음. dimension은 6

3x3 상삼각행렬의 경우 역시 dimension은 6이다.

그럼 대칭행렬이고 상삼각행렬인 경우는 어떨까?
$$
S \cap U = symmetric \; and \; upper \; triangular = diagnoal \; matrix
$$
이 경우 대각행렬이라고 볼 수 있다. 그럼 대각행렬의 basis와 dimension은?
$$
dim(S \cap U) = 3
$$




## Reference

https://www.youtube.com/watch?v=2IdtqGM6KWU&list=PLE7DDD91010BC51F8&index=12&t=1s

