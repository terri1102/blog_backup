---
layout: default
title: "12. Graphs, Networks, Incidence Matrices"
date: 2021-10-24
comments: true
nav_order: 12
parent: Math
use_math: true
---

<br>

{:toc} 



# **이번 강의의 목표**

- 배운 것들 응용
- Graphs & Networks
- Incidence Matrix
- Kirchhoff's Laws



# Graph

그래프는 이산적 데이터를 표현하는 데 아주 중요하다. 아래의 유향 그래프를 행렬로 표현해보자.

![graph1](https://github.com/terri1102/blog_backup/blob/master/assets/images/math/graph1.jpg?raw=true)
$$
Incidence \; Matrix \; A = \begin {bmatrix} -1 & 1 & 0 & 0 \\0 & -1 & 1 & 0 \\-1 & 0 & 1 & 0 \\-1 & 0 & 0 & 1 \\ 0& 0& -1 & 1 \end {bmatrix} = \begin {bmatrix} edge1 \\ edge2 \\ edge3 \\edge4 \\ edge5 \end {bmatrix}
$$


방향이 있는 그래프에서 -1은 엣지가 출발하는 점이고 1은 엣지가 도착하는 점이다. 이때 루프를 이루는 edge1,2,3을 살펴보자. 위 행렬의 행(엣지)들은 독립적인가? 



아니다. 1행+2행=3행 이기 때문이다.

즉 loop가 있는 그래프는 linearly dependent rows를 가진다고 할 수 있다.



이제 이 행렬의 subspace를 살펴보자. 그러기 위해 확인해야 할 것은 열들이다. 위 행렬에서 열들은 독립적인가? 만약 열들이 독립적이라면 영공간은 안에는 무엇이 있을까?

모든 열들이 독립적이라면 행렬 A의 영공간에는 영벡터만 있을 것이다. 영공간이 알려주는 것은 어떤 열들의 조합을 통해 영벡터를 얻을 수 있는 가이다. 만약 어떤 열들의 선형 결합으로도 0을 얻을 수 없다면 영공간 안에는 영벡터만 있다고 할 수 있다. 



그럼 이제 영공간의 기저를 찾아보자.


$$
Incidence \; Matrix \; A = \begin {bmatrix} -1 & 1 & 0 & 0 \\0 & -1 & 1 & 0 \\-1 & 0 & 1 & 0 \\-1 & 0 & 0 & 1 \\ 0& 0& -1 & 1 \end {bmatrix}  \begin {bmatrix} x1 \\ x2 \\ x3 \\x4  \end {bmatrix} =  \begin {bmatrix} x2-x1 \\ x3-x2 \\ x3-x1 \\x4-x1 \\ x4-x3  \end {bmatrix} =  \begin {bmatrix} 0 \\ 0 \\ 0\\0 \\0  \end {bmatrix}
$$


X = x1, x2, x3, x4, x5라고 할 때 potentials at nodes 는 x2-x1 등이다. 



여기서 나온 potentials이 뭔지 몰랐는데 찾아보니까 Kirchhoff's Law에 나오는 Node potential method에서 나오는 개념 같다. 


$$
X = C\begin {bmatrix} 1\\1\\1\\1\end {bmatrix}
$$


따라서 영공간의 기저는 (1,1,1,1)이고 dim N(A)=1이다.

Node potential method (also called nodal analysis)를 단계별로 살펴보자.

1. 임의의 노드 하나를 임의의 0 값으로 fix한다(Fix that potentials fof nodes) 이렇게 하면 미지수가 하나 감소한다. 

2. 그렇게 되면 마지막 열이 지워지고 나머지 열들은 독립적이라고 할 수 있다. 그러면 랭크는 3이 된다. 

   (Any 3 potentials are independent, good variables)



[The steps of the nodal analysis](https://www.tina.com/node-potential-method/)

1. Pick a reference node with 0 node potential and label each remaining node with *V1, V2* or *j1,* *j2*and so on.

2. Apply Kirchhoff’s current law at each node except the reference node. Use Ohm’s law to express unknown currents from node potentials and voltage source voltages when necessary. For all unknown currents, assume the same reference direction (e.g. pointing out of the node) for each application of Kirchhoff’s current law.

3. Solve the resulting node equations for the node voltages.

4. Determine any requested current or voltage in the circuit using the node voltages.



그럼 이제 다른 부분 공간들을 살펴보자.

### N(A^T)

$$
A^Ty=0
$$

일 때 
$$
dim \; N(A^T)
$$
은 무엇일까? m-r = 5-3 = 2이다.


$$
A^T = \begin {bmatrix} -1 & 0 & -1 & -1 & 0 \\ 1 & -1 & 0 &0&0\\0&1&1&0&-1 \\0 & 0&0&1 &1\end {bmatrix}  \begin {bmatrix} y1 \\ y2 \\ y3 \\y4\\y5  \end {bmatrix} =  \begin {bmatrix} 0 \\ 0 \\ 0 \\0  \end {bmatrix}
$$


![cycle1]


$$
A^Ty =0
$$
을 다시 살펴보면 


$$
\left\{ \begin {aligned} -y_1 -y_3 -y_4 &=0 \quad node1 \\
y_1 - y_2 &= 0 \quad node2(current \; in \:  current \: out)\\ 
y_2+y_3 - y_5 &= 0 \quad node3 \\
y_4+y_5 &= 0 \quad node4
\end{aligned}
\right.

$$


A^T를 소거법으로 정리하면 마지막 행은 어떻게 될까?

모두 0이다. rank가 3이니까(pivot 3개 가짐)

Basis for N(A^T)



### Row space of A(Column space of A^T)

dim을 구하려면 몇 개의 row가 독립적인지 살펴봐야 한다. 3개가 독립적이기 때문에 rank는 3이다.



loop가 없는 그래프 = Tree



dim N(A^T) = m-r 

여기서 rank는 n-1이니까(col 한 개가 종속적이어서) 이는 # loops = # edges - (#nodes - 1)



오일러의 공식? 강의에서는 formula라고 했는데 Euler characteristic 식이랑 더 비슷한 것 같다...
$$
\chi = V - E + F
$$


#nodes - #edges + #loops = 1

아래의 그래프를 보면 노드는 5개, 엣지는 7개, 루프는 3개이다. 따라서 5-7+3=1이니까 오일러 성질? 성립함.

![cycle2]



앞의 세 방정식을 하나로 묶으면
$$
A^TCAX = f
$$
basic equation of applied math (in equlibrium- 뉴턴의 법칙 성립x. 시간은 고려x)
$$
A^TCA
$$
는 항상 대칭.(balance equation)

X에서 시작해서 A 행렬 곱하고 옴의 법칙의 상수인 C를 곱하고 앞에 A^T를 곱하면 f나옴....더 찾아볼 것.



## Reference

https://www.youtube.com/watch?v=6-wh6yvk6uc&list=PLE7DDD91010BC51F8&index=13

