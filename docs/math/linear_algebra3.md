---
layout: default
title: "2. Elimination with Matrices"
date: 2021-09-10
comments: true
nav_order: 2
parent: Math
use_math: true
---



{:toc} 



# **이번 강의의 목표**

* 행렬 곱셈을 바라보는 5가지의 관점
* A의 역행렬이 있는 경우 판별
* A의 역행렬을 구하는 방법: determinant, Gauss-Jordan idea



# **1. 행렬 곱셈을 바라보는 5가지 관점**

## 1. Regular way

<justify>

행렬 A와 B를 곱해서 C가 나온다고 할 때 C행렬의 원소 c34는


$$
\begin {bmatrix} \\
 \\
 --row3--\\
 
  \\
  \end{bmatrix}\begin {bmatrix} ---c\\
 ---o \\
  ---l\\
 ---u \\
  ---m\\
  ---n\\
  ---4\end{bmatrix}= \begin {bmatrix}----
  \\----\\
  --c_{34}-\\
  ----\end{bmatrix}
$$

$$
c_{34}=(ros3\;of\;A) \; \dot{}\; (column4\;of\;B)= a_{31}b_{14}+a_{32}b_{24}+...=\sum^n_{k=1}a_{3k}b_{k4}
$$


이렇게 나타낼 수 있다.

</justify>

## 2. Column way

<justify>

Columns of C are combinations of columns of A



</justify>



## 3. Row way

<justify>

Rows of C are combinations of rows of B



</justify>



## 4. Sum of (Columns of A) x (Rows of B)



## 5. Block Multiplication

큰 행렬의 경우 행렬을 블록 단위로 나눠서 곱할 수도 있다. 블록을 행렬의 요소로 취급해서 행렬곱셈하듯이 연산하면 된다. 이걸 보고 배치 연산이랑 비슷한 건가? 하는 생각이 들었다. 좀 더 알아봐야 할듯



---

# 2. Inverses

## 1. A는 역행렬이 존재하는가?

<justify>

### A의 역행렬이 존재하는 경우: invertible, non-singular

정방행렬일 때 역행렬을 가지는 A행렬의 경우
$$
A^{-1}A=I=AA^{-1}
$$
가 성립한다.



역행렬을 구하는 방법

1. Determinant 이용
2. <span style="background:#FFD9EC">가우스-조던의 행렬 소거법</span>

Gauss-Jordan의 아이디어 : Solve 2 equations at onece







### A가 역행렬이 존재하지 않는 경우: not invertible, singular case

* determinant가 0인 경우
* 두 열이 같은 선 상에 존재하는 경우 

$$
\begin {bmatrix} 1 & 3 \\ 2 &6 \end {bmatrix}
$$

두 열이 같은 선 상에 있으면 같은 차원에 있다는 의미인데...그럼 1차원 벡터는 역행렬을 가지지 못하나? 그래서 위의 행렬이 역행렬이 없는 건가? 역행렬이 벡터공간에서 어떻게 표현되는지 찾아봐야 할거 같다. 좀 더 기하학적인 이해가 필요할 거 같다. 



[증명]

When can I find a vector X with AX = O?
$$
A =\begin {bmatrix} 1 & 3 \\ 2 &6 \end {bmatrix}
$$


A의 역행렬이 존재한다고 가정하자.
$$
AX= O \;이면\; \\
A^{-1}(AX)= A^{-1}O \;이기\; 때문에 \\
X=O \;이다
$$
위의 식대로 X는 영행렬이 아닌 다른 값을 가질 수 없다. 하지만 X가 영행렬이 아닌 반례가 존재한다.


$$
\begin {bmatrix} 1 & 3 \\
2 & 6 \end {bmatrix}\begin {bmatrix} -3 \\ 1 \end {bmatrix}= \begin {bmatrix} 0 \\ 0 \end {bmatrix}
$$


따라서 귀류법에 의해서 가정이 깨지기 때문에 A의 역행렬은 존재하지 않는다.

</justify>











## **Glossary**



# **Reference**

[MIT 18.06 강의](https://www.youtube.com/watch?v=QVKj3LADCnA&list=PLE7DDD91010BC51F8&index=3)

[가우스 소거법의 기하학적 의미](https://angeloyeo.github.io/2019/09/09/Gauss_Jordan.html )
