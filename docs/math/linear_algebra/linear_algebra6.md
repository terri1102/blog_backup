---
layout: default
title: "6. Column Space and Nullspace"
date: 2021-09-27
comments: true
nav_order: 6
parent: Math
use_math: true
---



{:toc} 



# **이번 강의의 목표**

* Vector space와 subspace 복습

* Column space와 Null space의 개념 알기



# 1. Vector space and Subspace

## Vector space

지난 시간의 복습으로 Vector space의 요건을 다시 기억해보자. 벡터 공간은 V+W와 cV가 벡터 공간에 존재해야 하고, cV+dW(선형 결합)도 공간에 존재해야 한다. 즉, 벡터 공간(또는 선형 공간)은 원소를 서로 더하거나 주어진 배수로 늘이거나 줄일 수 있는 공간이다. 



여기서 벡터 공간 안에서 벡터들은 8가지 성질을 만족한다

1. A+B = B+ A : 교환(commutative) 법칙
2. (A+B) + C = A + (B+C) : 결합(associative) 법칙
3. A + 0 = A : 항등원
4. A + (-A) = 0 : 역원
5. c(A+B) = cA + cB : 분배 법칙
6. (c+k)A = cA + kA : 분배 법칙
7. c(kA) = (ck)A
8. 1A = A



## Subspace

<p align="center">

<img src="https://github.com/terri1102/blog_backup/blob/master/assets/images/math/subspace.jpg?raw=true" alt="subspace" style="zoom:100%;" />

</p>



P와 L이 
$$
\mathbb{R}^{3}
$$
의 subspace일 때 이 둘의 union도 subspace를 가질까? 
$$
P \cup L
$$
= all vectors in P or L or both

아니다. 벡터 공간인지 보기 위해 P+L, cP, vL, cP+vL 등 선형결합의 경우를 생각해보면...P와 L을 더한 것은 보통 P와 L을 벗어나기 때문에 subspace라고 할 수 없다.



그럼 
$$
P \cap L
$$
은 subspace를 가질까?

P와 L의 교집합은 0벡터이기 때문에 부분 공간을 가진다. 



이를 정리하면 부분공간 S와 T에 대해 S와 T의 교집합도 부분공간이다. 

요건 1) V, W가 S와 T 모두에 속하면 V+W도 S와 T에 속한다

요건 2) cV나 cW도 S와 T에 속한다



# 2. Column space

이제 열공간으로 넘어가보자. 


$$
A = \begin {bmatrix} 1 & 1& 2 \\ 2 & 1 & 3 \\ 3 & 1 & 4 \\ 4 & 1 & 5 \end {bmatrix}
$$


행렬 A의 열공간은 R4의 부분 공간이다. 그리고 A의 열공간은 C(A)라고 쓰며, 이는 모든 열들의 모든 선형 결합의 집합이다. 이 C(A)는 모든 4차원 공간을 채우는가? 아니다.

앞서 배운 내용과 연결해서 왜 아닌지 설명해보겠다. 

Ax=b는 모든 b에 대해서 해를 갖는가? 아니다. 4개의 방정식과 3개의 미지수를 가지기 때문에 해가 없는 경우도 있다. 
$$
Ax = \begin {bmatrix} 1 & 1& 2 \\ 2 & 1 & 3 \\ 3 & 1 & 4 \\ 4 & 1 & 5 \end {bmatrix} \begin {bmatrix} x_1 \\ x_2 \\ x_3 \end {bmatrix} = \begin {bmatrix} b1 \\ b2 \\ b3 \\ b4 \end {bmatrix}
$$
다시 말하면, A는 4차원 전체를 채우지 않기 때문에 b 벡터는 A와 x의 선형결합이 아닐 수도 있다. 하지만 몇 가지 경우 방정식의 해를 구할 수 있다. 그렇다면 어떤 벡터 b일 때 방정식을 풀 수 있을까? 이게 오늘 강의의 제일 중요한 질문이다. 일단 b 벡터가 영벡터이면 x도 영벡터가 된다. 그 외에 
$$
x = \begin {bmatrix} 1 \\ 0 \\ 0 \end {bmatrix} \; b= \begin {bmatrix} 1 \\ 2 \\ 3\\4 \end {bmatrix}, x = \begin {bmatrix} 0 \\ 1 \\ 0 \end {bmatrix} \; b = \begin {bmatrix} 1 \\ 1\\ 1\\ 1 \end {bmatrix}
$$
가 있다. b를 잘 보면 A의 열벡터인 것을 알 수 있다. 즉 Ax=b는 b가 A의 column space일 때 풀 수 있다! (We can solve Ax=b exactly when b is in C(A)) 왜냐하면 열공간은 정의상 모든 선형 결합(Ax)을 포함하기 때문이다. 따라서 열공간은 언제 방정식을 풀 수 있는 지 알려준다. 



그럼 A의 모든 열은 독립적인가(independent)? 이를 알아보기 위해서 열을 없애도 같은 열 공간을 가지는지를 보면 된다. 
$$
A = \begin {bmatrix} 1 & 1& 2 \\ 2 & 1 & 3 \\ 3 & 1 & 4 \\ 4 & 1 & 5 \end {bmatrix}
$$


의 3열을 없애도 열공간은 동일하다. 따라서 A의 열공간은 2차원의 R^4의 부분 공간이다.(2-dim subspace of R^4)



# 3. Nullspace

$$
Ax = \begin {bmatrix} 1 & 1& 2 \\ 2 & 1 & 3 \\ 3 & 1 & 4 \\ 4 & 1 & 5 \end {bmatrix} \begin {bmatrix} x_1 \\ x_2 \\ x_3 \end {bmatrix} = \begin {bmatrix} 0 \\ 0 \\ 0 \\ 0 \end {bmatrix}
$$

A의 영공간은 Ax=0을 만족시키는 모든 해이다. 

일단 어떤 A이든 간에 x는 영벡터를 포함한다. 그 외에도 더 생각해보면 A의 1열 + 2열 = 3열이기 때문에 (1,1,-1)도 해가 될 수 있다. 이를 일반화하면 
$$
c\begin {bmatrix} 1 \\ 1 \\ -1 \end {bmatrix}
$$
으로 A의 영공간은 R3의 (부분 공간인) 직선이다. 



그럼 Null space는 벡터 공간인가? Ax=0이 항상 subspace를 갖는지 확인해본다. 1) 만약 AV= 0 이고 AW=0이면 A(V+W)=0이다. 따라서 V와 W가 영공간에 있으면 V+W도 영공간에 있다. 2) 만약 AV= 0 이면 A(12V)=0이다. (스칼라값은 앞으로 뺄 수 있으므로 12(AV)로 바꾸면 AV=0이다) 이 두 조건을 만족하기 때문에 벡터 공간이다. 



벡터 공간의 의미에 대해 더 알아보기 위해 b를 영벡터가 아닌 다른 벡터라고 가정해보자
$$
Ax= \begin {bmatrix} 1 & 1 & 2 \\ 2 & 1 & 3 \\ 3 & 1 & 4 \\ 4 & 1 & 5 \end {bmatrix}\begin {bmatrix}x_1 \\ x_2 \\ x_3 \end {bmatrix} = \begin {bmatrix} 1 \\ 2 \\ 3\\ 4 \end {bmatrix}
$$
x의 해가 될 수 있는 (1,0,0) 등은 subspace를 form하는가? 아니면 vector space를 form하는가? x의 해에는 영벡터가 포함 안 되기 때문에 origin을 지나지 않게 되고 x는 subspace가 아니다. 

이렇게 열공간과 영공간 두 가지의 부분 공간에 대해서 배워봤다.



# Reference

[MIT 18.06 Linear Algebra, Spring 2005](https://www.youtube.com/watch?v=8o5Cmfpeo6g&list=PLE7DDD91010BC51F8&index=7&t=2s)

https://rfriend.tistory.com/173

