---
layout: default
title: "5. Transposes, Permutations, Spaces R^n"
date: 2021-09-26
comments: true
nav_order: 5
parent: Math
use_math: true
---



{:toc} 



# **이번 강의의 목표**

* Permutation과 Transpose 마무리
* Subspace 개념



# 1. Permutations

지난 시간에 이어서 Permutation에 대해 복습하자면, Permutation matrix는 행 교환을 실행하는 행렬이다.

P 행렬은 `Identity matrix with reordered rows`이며 여기에 단위행렬도 포함된다. P 행렬의 개수는 nxn행렬인 A행렬에 대해 n!개이다.



1) 행 교환이 필요 없는 경우 : A=LU 여기서 P는 단위행렬

Matlab 연산은 

1. pivot이 0인지 체크
2. pivot이 충분히 큰지 체크-> 대수적으로 필요없지만 너무 작은 값이면 numerical accuracy에 영향있음

이렇게 두 가지 조건을 체크해서 행 교환을 시행한다.



2) 행 교환이 필요한 경우 : PA = LU (모든 invertible한 A행렬에 대해 이렇게 쓸 수 있음)



그 외


$$
P ^ {-1} = P^T \\
P^T P=I
$$


# 2. Transpose

전치행렬의 일반식은
$$
(A^T)_{ij}=A_{ji}
$$
이다.



A가 대칭행렬(symmetric matrix)인 경우 
$$
A^T=A
$$
이다. 



 성질 1)
$$
R^TR
$$
 는 항상 대칭이다.


$$
R = \begin {bmatrix} 1 & 2 & 4 \\
3 & 3& 1\end {bmatrix}
$$


증명 1) 직접 곱해본다
$$
\begin {bmatrix} 1 & 3 \\ 2 & 3 \\ 4 &1 \end {bmatrix} \begin {bmatrix} 1 & 2 & 4 \\
3 & 3& 1\end {bmatrix} = \begin {bmatrix} 10 &11& 7 \\11\\8 \end {bmatrix}
$$


증명 2) 전치해본다


$$
(R^TR)^T = R^T R ^{TT}= R^TR
$$
계산하지 않고도 항상 대칭행렬이 나오는 것 증명



# 3. Vector Space & Subspace



벡터 공간에 대해 알아보기 위해 R2 를 예시로 살펴보겠다.

R2: `all 2-dim real vectors`= `x-y plane`



벡터공간의 조건 : 벡터공간은 벡터들의 선형결합에 대해 닫혀있어야 한다. 즉 선형결합을 한다고 해서 다른 차원으로 가거나 하면 안 됨.

Vector space has to be closed under multiplication and addition of vectors: linear combination
