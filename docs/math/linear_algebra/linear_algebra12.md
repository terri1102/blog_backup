---
layout: default
title: "11. Matrix Spaces; Rank 1; Small World Graphs"
date: 2021-10-24
comments: true
nav_order: 11
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

![graph1](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/math/graph1.jpg?raw=true)
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
$$
A^Ty=0
$$
일 때 dim N(A^T)은 무엇일까? m-r = 5-3 = 2이다.


$$
A^T = \begin {bmatrix} -1 & 0 & -1 & -1 & 0 \\ 1 & -1 & 0 &0&0\\0&1&1&0&-1 \\0 & 0&0&1 &1\end {bmatrix}  \begin {bmatrix} y1 \\ y2 \\ y3 \\y4\\y5  \end {bmatrix} =  \begin {bmatrix} 0 \\ 0 \\ 0 \\0  \end {bmatrix}
$$


X = x1, x2, x3, x4 







## Reference

https://www.youtube.com/watch?v=6-wh6yvk6uc&list=PLE7DDD91010BC51F8&index=13

