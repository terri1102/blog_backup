---
layout: post
title: "TIL"
date: 2021-01-11
category: [TIL]
excerpt: "2021.01.11-2021.01.17까지 TIL"
tags: [ANOVA, levene, one-hot coding]
comments: true
---

Covariance와 Correlation의 관계


$$
Cov(X,Y) = E((X-\mu_x)(Y-\mu_y))
$$




| Correlation   | 경우의 수 | (X-mu_X) | (Y-mu_Y) | Cov(X,Y) |
| ------------- | --------- | -------- | -------- | -------- |
| X, Y positive | X>mu_X    | +        | +        | +        |
| positive      | X<mu_X    | -        | -        | +        |
| negative      | X>mu_X    | +        | -        | -        |
| negative      | X<mu_x    | -        | +        | -        |



하지만 X와 Y의 단위는 각각 다르기 때문에 공분산의 정도를 보여주지는 못 함(얼마나 covariant한지 알 수 없음)

이를 비교하기 위해서 normalize 과정을 거침
$$
Corr(X,Y) = \frac{Cov(X,Y)}{\sigma_X\sigma_Y} = \rho
$$
Cov(X,Y)는 절대 Var(X)*Var(Y)보다 클 수 없다
$$
Cov(X,Y) \leq Var(X)*Var(Y)
$$
따라서
$$
-1 \leq  \rho \leq 1
$$

$$
\rho =1 :완벽한 양의 상관관계(한 직선상에 있음) \rho = 0:아무 관련도 없음 \rho=-1 완벽한 음의 상관관계(한 직선상)
$$

$$
\rho >0 : 양의 상관관계
$$



---

Projection Vector 증명

Projection vector(사영 벡터)



선형독립, Span과 basis , rank의 관계



벡터가 독립이라는 의미

vectors are independent if 
$$
c_1X_1+c_2X_2+...+c_nX_n \ne0, \quad except \; for \; all \; c_i=0
$$
임의의 스칼라 값을 곱해서 벡터끼리 연산을 했을 때 결과 값이 0이 나온다면 종속적이다.

독립일 때: 벡터들을 행렬 A의 column으로 넣고 행렬 A의 null space를 확인

만약 이 행렬 A의 null space가 오직 zero vector만 존재할 경우, 이 벡터들은 독립이다.

이때 행렬 A의 rank는 column의 수와 같다.

따라서 free variable이 존재하지 않는다



종속일 때: 벡터들을 행렬 A의 column으로 넣고 행렬 A의 null space를 확인

행렬 A의 Null space(Ax=0)가 zero vector 이외의 다른 값이 존재할 때 이 벡터들은 종속이다.



Span: 보통 span a space 라고 표현한다. span은 벡터들의 조합으로 형성할 수 있는 공간

벡터 v1,v2..vn이 어떤 공간(space)을 span한다는 의미 

벡터 v1, v2, v3,...vn들의 가능한 모든 선형 조합으로 공간을 형성하는 것을 의미한다. 이때 형성되는 공간은 조합되는 벡터에 따라 Rn이 될 수도 subspace가 될 수도 있다.

결국 span이라는 것은 벡터들의 모든 가능한 선형 결합에 대한 결과벡터들을 하나의 공간에 몰아 넣은 것을 의미. 



기저벡터들은 독립이고, 공간을 span한다. 

독립인 벡터들은 공간의 기저로서 공간을 span한다.

만약 v1, v2가 독립인 벡터라고 할 때 R3를 span한다고 할 수 없다. 3차원 공간에서 어떤 평면만을 span한다.



벡터들이 N차원의 기저가 되는 지 아는 법은 벡터들을 (column vector로 만들고 이를)하나의 행렬로 만들고 가우스 소거법을 이용해 echelon form 행렬을 만든다. 그런 뒤 이 행렬이 free variable을 갖는지, 혹은 모든 각 column vector가 pivot을 갖는지 확인하면 된다. 즉, 행렬의 rank를 가지고 판별하게 된다. 



우리가 만약 N차원의 공간 Rn에 대해 n개의 벡터를 가지고 있을 때 이들이 기저 벡터가 되기 위해서는 nxn 행렬의 역행렬이 존재(invertible)해야 한다. 즉 3차원 공간의 R3의 기저는 3개의 3차원 크기의 벡터가 존재해야 하며, 3x3 크기의 행렬의 역행렬이 존재해야 한다.



차원(Dimension): 주어진 공간(space)들에 대한 모든 기저들은 같은 수의 벡터를 가진다. 여기서 벡터의 수가 바로 그 공간의 차원(dimension)을 의미한다.





어떤 행렬 A의 Rank는 행렬 A의 column space C(A)의 차원(Dimension)이다. (=number of pivor columns) 

https://twlab.tistory.com/24

