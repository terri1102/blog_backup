---
layout: default
title: "4. Factorization into A = LU"
date: 2021-09-18
comments: true
nav_order: 4
parent: Math
use_math: true
---



{:toc} 



# **이번 강의의 목표**

* Elimination을 통한 행렬 분해
* Permutation matrix의 개념



# **1. A = LU**

이번 강의에서는 Elimination을 통해 행렬분해하는 법을 배울 것이다. 이때 A는 행의 교환없이 elimination으로 pivot을 구할 수 있는 2x2 정방행렬이라고 가정하고, 이제 A와 U를 연결해주는 L을 구해보자.


$$
A = \begin {bmatrix} 2 & 1 \\
8 & 7\end {bmatrix}
$$
일 때, U는 
$$
U = \begin {bmatrix} 2 & 1 \\
0 & 3\end {bmatrix}
$$
이다. 이를 만족시키는 E21은
$$
E_{21} A = U
$$

$$
\begin {bmatrix} 1 & 0 \\
4 & 1\end {bmatrix}\begin {bmatrix} 2 & 1 \\
8 & 7\end {bmatrix} = \begin {bmatrix} 2 & 1 \\
0 & 3\end {bmatrix}
$$


이고, 이를 A에 대한 식으로 쓰면


$$
A = LU
$$

$$
\begin {bmatrix} 2 & 1 \\
8 & 7\end {bmatrix} = \begin {bmatrix} 1 & 0 \\
-4 & 1\end {bmatrix}\begin {bmatrix} 2 & 1 \\
0 & 3\end {bmatrix}
$$


이다. L은 E21의 역행렬이고 Lower triangle을 의미한다. (U는 Upper triangle)

여기서 연산을 한 번 더하면 행렬로 피봇값만 분리해서 볼 수도 있다.


$$
\begin {bmatrix} 2 & 1 \\
8 & 7\end {bmatrix} = \begin {bmatrix} 1 & 0 \\
-4 & 1\end {bmatrix}\begin {bmatrix} 2 & 0 \\
0 & 3\end {bmatrix} \begin {bmatrix} 1 & \frac{1}{2} \\
0 & 1\end {bmatrix}
$$


이제 조금 더 복잡한 3x3 행렬인 행 교환이 필요없는 A를 생각해보자. A를 LU로 분해하기 위해서는 E32E31E21의 역행렬을 구해야 한다.


$$
E_{32}E_{31}E_{21}A=U
$$

$$
A=E_{21}^{-1}E_{31}^{-1}E_{32}^{-1}U
$$


이제 E32, E31(=I), E21을 계산 편의를 위해 아래의 값을 갖는 행렬로 가정해보자.


$$
E_{32}= \begin {bmatrix} 1 & 0 & 0 \\ 0& 1 & 0 \\ 0 & -5 & 1\end {bmatrix}, E_{31} = \begin {bmatrix} 1 & 0 & 0 \\ 0& 1 & 0 \\ 0 & 0 & 1\end {bmatrix}, E_{21}=\begin {bmatrix} 1 & 0 & 0 \\ -2& 1 & 0 \\ 0 & 0 & 1\end {bmatrix}
$$

$$
E_{32}E_{31}E_{21} = \begin {bmatrix} 1 & 0 & 0 \\ -2& 1 & 0 \\ 10 & -5& 1\end {bmatrix}=E
$$


여기에서 3행 1열의 10이 마음에 들지 않으니...(왜??) 이제 E의 역행렬을 구해서 
$$
A= E^{-1}U
$$
를 구할 것이다. E31은 단위 행렬이기에 생략하고 E21의 역행렬과 E 32의 역행렬을 구해서 이를 순서대로 곱하면 된다. 


$$
E_{21}^{-1}E_{32}^{-1}= E^{-1}=L
$$



$$
\begin {bmatrix} 1 & 0 & 0 \\ 2& 1 & 0 \\ 0 & 0 & 1\end {bmatrix}\begin {bmatrix} 1 & 0 & 0 \\ 0& 1 & 0 \\ 0 & 5 & 1\end {bmatrix} = \begin {bmatrix} 1 & 0 & 0 \\ 2& 1 & 0 \\ 0 & 5 & 1\end {bmatrix} = L
$$


이번에 나온 행렬 L에는 10이 없다. 
$$
E_{21}^{-1}
$$
과
$$
E^{-1}_{32}
$$
의 2와 5가 곱해지지 않았기 때문에 10이 안 만들어진 것이다. 이렇게 right order로 행렬 곱셈을 하면 각 원소들은 자리만 바꾼채 유지된다.

> In the right order, the multipliers just sit in the matrix L. I just keep a record of what these multipliers were and that gives me L. If no row exchanges, multipliers go directly into L



## Cost efficiency of Elimination

이제 다른 관점으로 생각해보자. Elimination 연산은 얼마나 비싼가? 즉 nxn 행렬 A에 연산 몇 번을 해야 U를 얻을 수 있을까?

예를 들어 n을 100이라고 하고 A행렬의 원소가 모두 0이 아닐 때 연산 횟수는 n에 비례하는가? (여기서 연산은 곱하기와 빼기) nxn 행렬일 때 연산 횟수는
$$
n^2+(n-1)^2+...+2^2+1^2
$$
이며, 최악의 경우 
$$
n^3
$$
이다. 참고로 determinant(행렬식)으로 구하면 비용은 n!이다. 
$$
n^2+(n-1)^2+...+2^2+1^2 
$$
는 미적분의 integral을 이용해서 생각해볼 수 있다. (선형대수는 이산적인 세계를 다루고 미적분은 연속적인 세계를 다루긴 하지만)
$$
\int n^2 dx = \frac{1}{3}n^3
$$
이기에
$$
n^2+(n-1)^2+...+2^2+1^2 \approx \frac{1}{3}n^3
$$
이다. 반면 A에 행하는 연산이 아닌 A = LU에서 식의 오른쪽에 수행하는 연산의 비용은 
$$
n^2
$$
 이다.

따라서 A를 LU로 나누는 것의 비용은 
$$
\frac{1}{3}n^3
$$
이고 E의 역행렬 연산(L을 구하는 것)의 비용은
$$
n^2
$$
이기에 E를 구하는 것보다 L을 구하는 연산이 더 효율적이다.

(오른쪽 연산을 뭐라고 써야하나...)



# 2. Permutation

이제 행렬의 소거를 하는 과정에서 row exchange가 필요한 행렬의 경우에 대해서 생각해보자. 피봇값이 0이면 반드시 행 교환을 해야 한다.

**치환행렬(permutation matrix):** 순서가 부여된 임의의 행렬을 의도된 다른 순서로 뒤섞는 연산 행렬. 즉 치환 행렬 P는 임의의 행렬 A에 대해서 (왼쪽에 곱해지는) PA의 연산을 통해서 A 행렬의 행 또는 열의 순서를 재배열하게 된다. 

3x3 단위 행렬의 행을 교환하는 방법은 몇 가지일까?
$$
\begin {bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end {bmatrix}
$$
의 행을 교환해서 만들 수 있는 행렬들은 행 한 번 교환시
$$
\begin {bmatrix} 0 & 1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end {bmatrix}, \begin {bmatrix} 0 & 0 & 1 \\ 0 & 1 & 0 \\ 1 & 0 & 0 \end {bmatrix}, \begin {bmatrix} 1 & 0 & 0 \\ 0 & 0 & 1 \\ 0 & 1 & 0\end {bmatrix}
$$
3가지, 두 번 교환시
$$
\begin {bmatrix} 0 & 1 & 0 \\ 0 & 0 & 1 \\ 1& 0 & 0 \end {bmatrix}, \begin {bmatrix} 0 & 0 & 1 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end {bmatrix}
$$
2가지이다. 즉 3x3행렬을 행을 교환해서 만들 수 있는 행렬의 개수는 6개이다. 4x4 행렬의 Permutation matrix의 조합은 4x3x2x1로, nxn 행렬의 치환행렬의 개수는 n!이다.

만약 위의 P행렬들을 서로 곱한다면 어떻게 될까? 이 행렬들을 서로 곱하거나 invert해도 P 행렬의 그룹 안에 속하게 된다. 



**치환 행렬의 속성**

1. 치환행렬의 역행렬은 치환행렬의 전치행렬과 같다.

$$
P^{-1} = P^T
$$







## Reference

[permutation matrix](https://ko.wikipedia.org/wiki/%EC%B9%98%ED%99%98%ED%96%89%EB%A0%AC)

https://twlab.tistory.com/13

https://www.youtube.com/watch?v=MsIvs_6vC38&list=PLE7DDD91010BC51F8&index=5
