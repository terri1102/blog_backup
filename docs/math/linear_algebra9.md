---
layout: default
title: "9. Independence, Basis, and Dimension"
date: 2021-10-11
comments: true
nav_order: 9
parent: Math
use_math: true
---



{:toc} 



# **이번 강의의 목표**

- Linear Independence
- Spanning a space
- Basis and dimension 개념 배우기



# 1. Independence

 간단한 예시를 통해서 선형 독립이라는 개념을 알아볼 것이다. A 행렬은 (mxn)의 크기를 갖는 행렬이라고 가정해보자(단, m < n). 그렇다면 Ax=0에는 영벡터가 아닌 해가 존재한다. 이때 방정식의 개수보다 미지수의 개수가 더 많다. (미지수 xi는 n개인데, A의 행(방정식)은 m개이기 때문에) 그렇다면 A의 영공간에 0이 아닌 해가 있다는 결론을 내릴 수 있다. 따라서 RREF(A)의 결과로 나온 행렬이 적어도 하나(사실상 n-m개)의 free variable을 가질 것이다. 



그렇다면 여러 벡터들이 서로 (선형)독립적이라는 것은 무슨 의미일까? 

> Vectors x1, x2,...,xn are independent if no combination gives zero vector except the zero combinations. (zero combination : 모든 ci가 0인 결합)

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



## Glossary





# Reference

[MIT 18.06 Linear Algebra, Spring 2005](https://www.youtube.com/watch?v=yjBerM5jWsc&list=PLE7DDD91010BC51F8&index=10&t=9s)

