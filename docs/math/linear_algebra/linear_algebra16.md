---
layout: default
title: "16. Projection Matrices and Least Squares"
date: 2021-11-18
comments: true
nav_order: 16
parent: Math
use_math: true
---

<br>

{:toc} 



# **이번 강의의 목표**

- Projection matrices 설명
- Least Squares and best straight line



# 1. Projection matrix


$$
P = A(A^TA)^{-1}A^T
$$


* If b in column space 

$$
Pb = b
$$

* If b is perpendicular to column space

$$
Pb = 0
$$



위의 경우들을 하나하나 살펴보자. 

먼저 Pb =0에서 b가 column space에 직교한다는 것은 어떤 의미일까?

A의 전치행렬의 null space에 있다는 것을 의미한다. 

이를 증명하기 위해 P의 식에 b를 곱하면
$$
Pb = A(A^TA)^{-1}A^Tb
$$
이 되는데 
$$
A^Tb =0
$$
이기 때문에 Pb = 0이 된다.

다음으로 Pb = b인 경우를 살펴보자. Column space에 있다는 말의 의미는 column들의 결합(combinations of columns)이라는 것을 의미한다. (Ax로 표현할 수 있는 결합들)

따라서 
$$
Pb = A(A^TA)^{-1}A^TAx = \cancel{AA^{-1}} \cancel{(A^T)^{-1}A^T}Ax=Ax = b
$$
이기 때문에 Pb = b이다.

![columnspace]

P는 Pb이고 e는(I-P)b이다. (e도 projection임) P가 projection이면 I-P도 projection이고 P가 직교하면 I-P와도 직교한다. P square가 P이면 (I-P) square도 (I-P)이다. 



그럼 이제 가장 잘 fit하는 line y = C+Dt를 찾아보자. (전체 error를 가장 적게 만드는 선)

그럼 overall error는 어떻게 구해야 할까?

(1,1),(2,2),(3,2)를 지나는 직선을 구하기 위한 방정식들

C+D = 1

C+2D = 2

C+3D = 2
$$
Ax=b  \\  \begin{bmatrix} 1 & 1 \\ 1 & 2\\1&3 \end{bmatrix} \begin{bmatrix}C\\D \end{bmatrix} = \begin{bmatrix} 1 \\ 2 \\3 \end{bmatrix}
$$
그럼 이때 best possible은 무엇일까?

각 방정식별 error를 구한다음에 이를 모두 구해서 overall error를 구할 것이다. 그리고 이를 최소화하려고 할 것이다.


$$
\norm{Ax-b}^2 = \norm{e}^2
$$


이 그림에서 3개의 error는 어디에 있을까? vertical distance이다. 

전체 error는(Sum of Squares)
$$
\norm{Ax-b}^2 = \norm{e}^2 = e_1^2+e_2^2+e_3^2
$$
로 쓸 수 있으며 이를 minimize하는 것이 목표이다. 

이를 위해서 linear regression 방법을 사용해서 선을 구할 것이다. 



그럼 best fitting line으로 projection한 점들(p1,p2,p3)은 뭘까? closest point in the column space (combination)

p1, p2, p3를 이용하면 이제 방정식을 풀 수 있게 된다. 

이제 x와 p를 찾아보자.
$$
\hat{x} = \begin{bmatrix} \hat{C} \\ \hat{D} \end{bmatrix}, p
$$
x를 구하기 위한 방정식은
$$
A^TA\hat{x} = A^Tb
$$
 이다. (통계에서 fitting하는 선을 찾을 때 많이 씀)

식으로 나타내보면(normal equations)
$$
\begin{bmatrix} 1&1&1\\1&2&3 \end{bmatrix} \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 2 \\ 1 & 3 & 2 \end{bmatrix} = \begin{bmatrix}3 & 6 & 5 \\ 6 & 14 & 11\end{bmatrix}
$$
$$
3C + 6D = 5
\\
6C + 14D = 11
$$



이제 C와 D를 구할 수 있음

이를 Minimize할 에러를 구하는 식으로 구해보면


$$
\norm{Ax-b}^2 = \norm{e}^2 = e_1^2+e_2^2+e_3^2 \\
(C+D-1)^2 + (C+2D-2)^2 + (C+3D-2)^2
$$


이를 편미분하면 위의 식


$$
3C + 6D = 5 : error \; with \; respect \; to\; C\; being\; zero
\\
6C + 14D = 11 : error \;with\; respect\; to\; D\; being\; zero
$$
이 나옴

이를 풀면 해는 D = 1/2, C = 2/3

따라서 best line은 
$$
\frac{2}{3} + \frac{1}{2}t
$$
이다. 

이 선을 이용해서 p1, p2, p3와 e1, e2, e3를 구할 수 있다. 이를 이용해서 b를 구할 수 있다.


$$
b = p + e
$$

$$
\begin {bmatrix}1 \\ 2 \\ 2 \end{bmatrix} = \begin{bmatrix} 7/6 \\ 5/3 \\ 13/6 \end{bmatrix} + \begin{bmatrix}-1/6 \\ 2/6 \\ -1/6 \end{bmatrix}
$$


이때 p와 e는 직교한다...




## Reference

https://www.youtube.com/watch?v=YzZUIYRCE38&list=PLE7DDD91010BC51F8&index=15

