---
layout: default
title: "7. Solving Ax = 0: Pivot Variables, Special Solutions"
date: 2021-09-28
comments: true
nav_order: 7
parent: Math
use_math: true
---



{:toc} 



# **이번 강의의 목표**

* Computing the null space
* pivot variables & free variables
* special solution - rref(A) = R



# 1. Computing Nullspace

Ax=0에서 영공간을 구하는 방법에 대해 알아볼 것이다.


$$
A = \begin {bmatrix} 1 & 2 & 2 & 2 \\ 2 & 4 & 6& 8 \\ 3 & 6 & 8 & 10 \end {bmatrix}
$$


일 때, 일단 elimination을 실행한다. Elimination 후에도 해는 바뀌지 않는다.


$$
\begin {bmatrix} 1 & 2 & 2 & 2 \\ 0& 0 & 2& 4 \\ 0 & 0 & 2 & 4 \end {bmatrix}
$$


첫 번째 pivot 1 이후 다음 pivot을 보니 0이다. Row exchange를 할 만한 행을 찾아본다. 없기 때문에 다음 열로 넘어가면 2행 3열의 2가 두 번째 pivot이 된다. 이를 이용해서 세 번째 행에 elimination을 실행한다.


$$
\begin {bmatrix} 1 & 2 & 2 & 2 \\ 0& 0 & 2& 4 \\ 0 & 0 & 0 & 0 \end {bmatrix} = U
$$


이제 행렬이 echelon form(계단 모양)이 되었다. 그리고 숫자들이 대각선 위에 있으니까 upper triangle의 U라고 부르자. 이 행렬의 pivot 값은 U(1,1)인 1과 U(2,3)인 2이고, 첫 번째 열과 세 번째 열이 pivot columns가 되며, 나머지 열은 free columns가 된다. 이때 pivot 값의 개수가 2개이기 때문에 A의 rank는 2이다. (r = 2이고, n-r=2(free variable)이다.)



이렇게 소거로 구한 U를 이용해 Ux= 0을 구해보자. 이때 free columns인 2열과 4열에 곱해질 x2, x4는 아무 값이나 가능하다. 하지만 계산의 편의를 위해 보통 1이나 0을 쓰는데, 이번에는 x2=1, x4=0으로 두고 나머지 x1, x3을 구해보자.


$$
\begin {bmatrix} 1 & 2 & 2 & 2 \\ 0& 0 & 2& 4 \\ 0 & 0 & 0 & 0 \end {bmatrix} \begin {bmatrix} x_1 \\ 1 \\ x_3 \\ 0\end {bmatrix} = 0
$$

$$
x_1 + 2 + 2x_3 = 0 \\
2x_3 = 0
$$
이기에


$$
X = c \begin {bmatrix} -2 \\ 1 \\ 0 \\0 \end {bmatrix}
$$


이다. x2=0, x4=1로 두고 풀면 


$$
X = c \begin {bmatrix} 2 \\ 0 \\ -2 \\1 \end {bmatrix}
$$
이다. 그럼 Ax=0의 전체 해는 어떻게 될까? 영공간은 위에서 구한 special solution의 모든 결합을 포함하기 때문에 
$$
X = c \begin {bmatrix} -2 \\ 1 \\ 0 \\ 0 \end {bmatrix} + d \begin {bmatrix} 2 \\ 0 \\ -2\\1 \end {bmatrix}
$$
이다. 



# 2. Reduced row echelon form

이번엔 위의 방법보다 연산을 한 번 더해서 행렬의 형태를 rref로 만들어서 해를 구해보겠다. Rref는 pivot 값 위 아래에 0이 있으며 pivot값은 1인 행렬을 의미한다. 위에서 구한 U행렬을 가져오자. 참고로 마지막 행이 0인 것은 3행이 1행과 2행의 결합이었다는 것을 보여준다.


$$
\begin {bmatrix} 1 & 2 & 2 & 2 \\ 0& 0 & 2& 4 \\ 0 & 0 & 0 & 0 \end {bmatrix} = U
$$
U행렬 중 1행에서 2행을 빼서 pivot 위의 값을 0으로 만든다.


$$
\begin {bmatrix} 1 & 2 & 0 & -2 \\ 0& 0 & 2& 4 \\ 0 & 0 & 0 & 0 \end {bmatrix}
$$


이제 2행에 1/2를 곱해서 pivot을 1로 만들어준다.
$$
\begin {bmatrix} 1 & 2 & 0 & -2 \\ 0& 0 & 1& 2 \\ 0 & 0 & 0 & 0 \end {bmatrix} = R
$$


이렇게 구한 rref(A) = R를 통해 알 수 있는 정보는

- pivot rows : 1, 2
- pivot columns : 1, 3
- Identity matrix in pivot rows and pivot columns : 1열, 3열만 보면 단위 행렬임
- 마지막 행이 모두 0인 것 : 두 행의 선형결합이었음

이제 이렇게 구한 R를 이용해서 RX = 0 을 풀면 된다.


$$
x_1 + 2x_2-2x_4 = 0 \\
x_3 + 2x_4 = 0
$$
을 풀면 된다. AX=0이나 UX=0이나 RX=0의 해가 같다는 것을 (아직 익숙하지는 않지만) 잘 이해해야 한다. 



이를 back substitution하면 free part는 부호가 반대로 나타날 것이다. 
$$
X = c \begin {bmatrix} -2 \\ 1 \\ 0 \\ 0 \end {bmatrix} + d \begin {bmatrix} 2 \\ 0 \\ -2 \\ 1 \end {bmatrix}
$$
왜 이렇게 나타날까? rref form으로 행렬을 다시 써보면


$$
R = \begin {bmatrix} I & F \\ 0 & 0\end {bmatrix}
$$


RX= 0을 풀기 위해  RN=0인 null space 행렬을 구할 것이다. 왜냐하면 null space 행렬의 열이 special solution이기 때문이다. 


$$
N = \begin {bmatrix} -F \\ I \end {bmatrix}
$$


Matlab teaching code command 중에 N equal이 있는데 이는 null basis를 구해준다. R을 이용해서 pivot variable과 free variable을 골라서 pivot variable은 복사하고 free variable의 0과 1값을 유지한다.
$$
\begin {bmatrix} I & F \end {bmatrix} \begin {bmatrix} x_{pivot} \\ x_{free} \end {bmatrix} = 0
$$
이기 때문에 x_pivot은 -F x_free이다. special choice로 x_free를 단위행렬로 고르면 x_pivot은 -F이다. --> 수정 필요



이제 다른 예시를 보자


$$
A = \begin {bmatrix} 1 & 2 & 3 \\ 2 & 4 & 6 \\ 2 & 6 & 8 \\ 2 & 8 & 10 \end {bmatrix}
$$


이 행렬의 pivot column의 개수는 몇 개이고 어떤 열이 pivot column일까? 일단 3열은 1열 + 2열이니 pivot 열이 아니므로 pivot 열은 2개이다. 3열은 free column으로 elimination 후에 0이 되어 드러날 것이다. 이제 rref(A)를 구해보자.


$$
\begin {bmatrix} 1 & 2 & 3 \\ 0 & 0 & 0 \\ 0 & 2 & 2 \\ 0 & 4 & 4 \end {bmatrix}
$$


두 번째 행의 pivot이 0이니 아래 행을 봐서 row exchange가 가능한지 보자. 가능하다!


$$
\begin {bmatrix} 1 & 2 & 3 \\  0 & 2 & 2 \\ 0 & 4 & 4 \\ 0 & 0 & 0 \end {bmatrix}
$$


이제 3행에서 2행 x 2한 것을 뺀다.


$$
\begin {bmatrix} 1 & 2 & 3 \\  0 & 2 & 2 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end {bmatrix} = U
$$


U 행렬의 rank는 2이고 마지막 열은 free column이다. 



그럼 Nullspace와 special solution을 구해보자. 위에서 구한 U의 free variable인 x3을 1로 두고 풀어보면 (0을 넣으면 X는 모두 0이 되기 때문에 1로 한다)


$$
1 x_1 + 2x_2 + 3x_3 = 0 \\
2x_2 + 2x_3 = 0
$$
이기 때문에 x3=1이면 x2=-1, x1=-1이다. 


$$
X= \begin {bmatrix} -1 \\ -1 \\ 1\end {bmatrix}
$$
이 답이 되는지 빠르게 확인해보면 A의 1열 - A의 2열 + A의 3열 = 0벡터가 맞기 때문에 해가 맞다. 전체 nullspace는 


$$
X = c \begin {bmatrix} -1 \\ -1 \\ 1 \end {bmatrix}
$$
이고 
$$
\begin {bmatrix} -1 \\ -1 \\ 1 \end {bmatrix}
$$
는 basis(기저)가 된다.



rref로 계속 진행해보자. 


$$
U = \begin {bmatrix} 1 & 2 & 3  \\ 0 & 2 & 2 \\ 0 & 0& 0 \\ 0& 0& 0\end {bmatrix}
$$

$$
R = \begin {bmatrix} 1 & 0 & 1 \\ 0 & 1 & 1 \\ 0 & 0& 0 \\ 0& 0& 0\end {bmatrix}
$$




## Glossary

special solution : 한국어로 뭘까..?

particular solution: 특수해



# Reference

[MIT 18.06 Linear Algebra, Spring 2005](https://www.youtube.com/watch?v=VqP2tREMvt0&list=PLE7DDD91010BC51F8&index=8)

https://twlab.tistory.com/22
