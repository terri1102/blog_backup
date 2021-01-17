---
layout: post
title: "[선형대수]Eigen vector와 Eigen values를 이용한 차원축소"
date: 2021-01-14
category: [TIL]
excerpt: "고유값, 고유벡터의 개념과 벡터의 정사영"
tags: [Eigen vector, Eigen values, projection]
comments: true
---



# Eigen vectors and Eigen values
---

**Eigen vector(고유 벡터)**: 선형변환이 일어날 때 이전 span에 남아 있는 벡터 (다른 벡터들은 이전 span에서 knock off됨)

**Eigen value(고유값)**: 그 벡터에 곱해진 값(얼마나 늘어나고 줄어드는지를 결정)

즉, 선형변환(Linear Transformation)을 했을 때 그 변환의 축이 되어 방향이 바뀌지 않는 벡터를 `Eigen vector`라고 하고,  변환의 크기를 스칼라 값으로 나타낸 것이 `Eigen value`이다.



**선형변환의 정의**: Lines remain lines(no curvy lines) & origin remains fixed

(or 조건): T(변환)를 함수라고 생갔했을 떄, 벡터 a,b를 더한 값을 T에 넣는 것이나 벡터 a, 벡터 b를 각각 선형변환하고 이를 더한 값이나 같다. 
$$
T(\vec{a}+\vec{b})=T(\vec{a})+T(\vec{b})
$$

$$
T(c\vec{a})=cT(\vec{a})
$$

 

**Eigen vector, Eigen values 찾기**

우리가 찾고자 하는 v벡터는 행렬 A의 Eigen vector이다. 즉,  행렬 A와 곱했을 때, 방향은 변하지 않고 스칼라값만 바뀌는 벡터이다.


$$
A\vec{v}=\lambda\vec{v}
$$
[^ ]: (A * v: Matrix vector multiplication,  lambda * v = scalar multiplication)

위의 식에서 형태을 맞추기 위해 lambda를 lambda*identity matrix로 바꿔준다.
$$
A\vec{v}-(\lambda I)\vec{v}=\vec{0}
$$

$$
(A-\lambda I)\vec{v}=\vec{0}
$$

이때 v벡터가 0행렬이 아닌 값을 찾는데, v벡터와 곱해서 0행렬이 나온다는 것은 차원 축소가 일어난다는 것을 의미한다. 

**즉, det(A-lambda*I) =0이다.** (행렬식이 0이면 역행렬이 존재하지 않고, 차원축소가 일어난다.)



정리하면!! 행렬 A의 eigen vector를 구하고 싶을 때,
$$
A =[(^3_0) (^1_2)]
$$
일 때, 여기서 lambda * I 를 뺀 것의 행렬식을 구해서 그것이 0인 값을 찾으면 된다.


$$
det([{^{3-\lambda}_0} {^1_{2-\lambda}}])=0
$$

$$
(3-\lambda)(2-\lambda) = 0
$$

즉 lambda는 2 또는 3이 나온다. 

이 lambda 값을 대입해서 벡터 v를 구하면 된다.



cf)하나의 아이겐 값이어도 여러 아이겐 벡터 가질 수 있다.



**Unit vector와 basis vector 차이**: unit vector는 길이가 1인 벡터.  basis vector는 기저에 속하는 여러 벡터 중 하나의 벡터.

**Unit vectors**
$$
\hat{i} = \begin{bmatrix} 1 \\ 0 \end{bmatrix} \quad \hat{j}= \begin{bmatrix} 0 \\ 1 \end{bmatrix} 
$$


**Change of Eigen basis**
$$
A =[{^3 _0} {^1 _2}]
$$
의 lambda값(eigen value)을 구할 때 basis vector를 이용할 수 있다. 여기서의 basis vector은 (우연히도) 아이겐 벡터를 합친 행렬이라고 가정한다.
$$
Change\; of \; basis \;matrix = [{^1 _0}{^{-1}\ _1}]
$$
일 때, 벡터 A의 앞에는 basis vector의 역행렬을, 뒤에는 basis vector를 곱한다.
$$
[{^1 _0}{^{-1}\ _1}]^{-1}*[{^3 _0} {^1 _2}]*[{^1 _0}{^{-1}\ _1}] = [{^3 _0}{^0 _2}]
$$
이렇게 하면 대각선 행렬의 형태로 eigen value가 나온다. 즉 lambda는 3 또는 2.



```python
np.linalg.eig(array)
#대각선 형태로 eigen value와 eigen vector가 나온다.
```



**행렬 변환 예시**


$$
\begin{bmatrix} a &b \\ c & d  \end{bmatrix}
$$
가 있을 때 
$$
\begin{bmatrix} a\\c\end{bmatrix}
$$
의 basis vector가 도달하는 곳은
$$
\begin{bmatrix} 2 \\ 1 \end{bmatrix}
$$

$$
\begin{bmatrix} b\\d\end{bmatrix}
$$

의 basis vector가 도달하는 곳은 
$$
\begin{bmatrix} 3\\-2\end{bmatrix}
$$
라고 하자.



이 때 Transformation
$$
\begin{bmatrix} a &b \\ c & d  \end{bmatrix}
$$
을 벡터 x,y
$$
\begin{bmatrix} x \\ y  \end{bmatrix}
$$
에 적용한다면 
$$
\begin{bmatrix} a &b \\ c & d  \end{bmatrix}\begin{bmatrix} x \\ y  \end{bmatrix}=x\begin{bmatrix} a \\ c   \end{bmatrix}+ y\begin{bmatrix}b\\d\end{bmatrix} =\begin{bmatrix} ax+by \\ cx + dy  \end{bmatrix}
$$

행렬은 공간의 변환이라고 생각하라네요..

----



#### 정사영 증명(projection of vector)



1. cosine으로 증명: vector a, vector b가 있을 때 a의 사영벡터 vector m 구하기 (vector m : projection of a onto vector b)

   

$$
\vec{m}= \|\vec{m}\|*\hat{m}  \quad \|m\|은 \; 길이고, \hat{m}은 \;단위벡터
$$

$$
cos(\theta) \; 알면 \;\|\vec{m}\| \;알 \;수\;있음 \quad (cos(\theta)= \frac{\|\vec{m}\|}{\|\vec{a}\|})
$$

$$
cosine \;similarity\;이용\quad cos(\theta)=\frac{\vec{a}\cdot\vec{b}}{\|\vec{a}\|\|\vec{b}\|}
$$

$$
위의 \;두\;식을\;합치면\;\|\vec{m}\|=\frac{\vec{a}\cdot\vec{b}}{\|\vec{b}\|}
$$

단위 벡터의 정의에 따라 
$$
\hat{b}=\frac{1}{\|\vec{b}\|}\vec{b}=\hat{{m}} \quad (b랑\; m이랑 \;기울기 \;같으니까 \;단위벡터를 \;공유함)
$$
따라서
$$
proj_b\vec{a}(=\vec{m}) = \frac{\vec{a}\cdot\vec{b}}{\|\vec{b}\|}*\frac{\vec{b}}{\|\vec{b}\|}=  \frac{\vec{a}\cdot\vec{b}}{\|\vec{b}\|^2}*\vec{b}
$$



2. 직교성의 정의로 증명

위의 증명에서 사용한 벡터들을 그대로 가져와서 증명한다. 단 직선 L은 벡터b를 늘린 직선, proj(a)= 벡터 m.

벡터 a의 직선 L에 대한 정사영(Projection of a onto L)과 a에서 정사영을 뺀 직선 L위의 벡터(a-proj(a))는 직선 L에 직교한다.

이때 벡터 proj(a)은 벡터 b에 스칼라 곱을 한 형태이다. 
$$
C\vec{b}
$$
 따라서 a-cb는 L에 직교한다.

**직교성의 정의**에 따라 직선 L에서 직교하는 두 벡터의 내적은 0이다 . 
$$
(\vec{a}-C\vec{b})\cdot\vec{b}=0
$$
내적은 분배법칙이 성립하기에 
$$
\vec{a}\cdot\vec{b}-C\vec{b}\cdot\vec{b}=0
$$

$$
\vec{a}\cdot\vec{b}=C\vec{b}\cdot\vec{b}
$$

$$
proj_L\vec{a}=C\cdot\vec{b}=\frac{\vec{a}\cdot{b}}{\vec{b}\cdot\vec{b}}*\vec{b}
$$

->벡터의 정사영은 PCA에서 사용된다.