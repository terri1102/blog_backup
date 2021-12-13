---
layout: default
title: "14. Orthogonal Vectors and Subspaces"
date: 2021-11-12
comments: true
nav_order: 14
parent: Math
use_math: true
---

<br>

{:toc} 



# **이번 강의의 목표**

- 벡터들이 직교한다는 것의 의미, subspace가 직교한다는 의미, basis가 직교한다는 의미 알기
- Null space와 Row space가 직교하는 것 이해
- 

$$
N(A^TA) = N(A)
$$

이해



# 1. Orthogonal Vectors

Orthogonal(=perpendicular)하다는 의미: 벡터들끼리 각도가 90도라는 것

직각 삼각형으로 만들어서 보자.

두 벡터가 직교하는지 알고 싶으면 
$$
X^TY=0
$$
을 만족시키는지 확인하면 된다. 즉 X^T와 Y의 dot product이 0인지 확인하면 된다.

또한 피타고라스 정리에 따라서
$$
\norm{x}^2+\norm{y}^2 = \norm{x+y}^2
$$
를 만족시키는지 확인해도 된다. (직각삼각형에서만 성립)

그렇다면 위의 두 식은 어떻게 연결지을 수 있을까?

예시를 들어서 살펴보자.

 x, y를 아래의 벡터라고 두고 length square를 구해보자.


$$
x=\begin {bmatrix} 1\\2\\3 \end {bmatrix}, y= \begin {bmatrix} 2 \\ -1\\0 \end{bmatrix}, x+y =\begin {bmatrix} 3\\1\\3 \end {bmatrix}
$$


cf) 참고로 강의에서 length square 라고 하면
$$
\norm{x}^2
$$
이걸 의미함


$$
\norm{x}^2 = 14, \quad \norm{y}^2 = 5, \quad \norm{x+y}^2 = 19
$$
이기 때문에 
$$
\norm{x}^2+\norm{y}^2=\norm{x+y}^2
$$
이 성립하고, 이를 
$$
\norm{x}^2 == x^Tx
$$




## Reference

https://www.youtube.com/watch?v=YzZUIYRCE38&list=PLE7DDD91010BC51F8&index=15

