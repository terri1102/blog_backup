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



# 이번 강의의 목표

* Determinant를 사용하지 않고 Elimination을 이용해서 해를 구하는 법

* Elimination으로 해를 구할 수 없는 경우 알기



# 1. Elimination으로 해를 구하기

<justify>

3개의 미지수와 3개의 방정식이 주어졌을 때 <span style="background:#FFD9EC">가우스-조던 행렬 소거법</span>을 이용해서 해를 구해보자


$$
x + 2y + z = 2 \\
3x+8y+z=12 \\
4y+z = 2
$$



이를 행렬 Ax=b 형태로 바꾸면



$$
\begin {bmatrix} 1 & 2 & 1 \\
3 & 8 & 1 \\
0 & 4 & 1 \end{bmatrix} \begin {bmatrix} x \\ y \\ z \end{bmatrix} = \begin {bmatrix} 2 \\ 12 \\ 2 \end{bmatrix}
$$



**[STEP 1]** 첫 번째 방정식에 적당한 수를 곱해서(Scaling) 두 번째 방정식과 연립했을 때 x를 소거하게 만들자(행렬 A의 (2,1) 원소를 0으로 만들기). 이때 첫 번째 행은 pivot row이기 때문에 그대로 두고, 첫 번째 행에 3을 곱한 후 두 번째 행에서 이를 제한다(Substration). 식의 오른쪽 부분은 일단 무시한다.




$$
\begin {bmatrix} 1 & 2 & 1 \\
0 & 2 & -2 \\
0 & 4 & 1 \end{bmatrix} \begin {bmatrix} x \\ y \\ z \end{bmatrix}
$$



**[STEP 2]** 세 번째 방정식의 x자리도 0으로 만들자(행렬 A의 (3,1) 위치). 하지만 이미 0이기 때문에 넘어간다.



**[STEP 3]** 이제 A의 (3,2) 위치도 0으로 만들어 주기 위해 두 번째 행에 2을 곱한 후 세 번째 행에서 제한다. 



$$
\begin {bmatrix} 1 & 2 & 1 \\
0 & 2 & -2 \\
0 & 0 & 5 \end{bmatrix} \begin {bmatrix} x \\ y \\ z \end{bmatrix} 
$$



이제 행렬 A는 upper triangle(상삼각)의 형태의 U행렬이 되었으며, 대각선 원소인 1, 2, 5는 각 행의 pivot 값이 된다. U행렬은 가우시간 소거법에 의한 결과로 Reduced row-echelon form이라고 한다. 

</justify>

##  Failure case

이런 Elimination으로 연립 일차 방정식의 해를 못 구하는 경우는 Pivot 값을 구할 수 없는 경우이다. Pivot을 못 구하는 상황은  <span style="background:#fff28c">대각선의 값이 0</span>인 경우이고, 이는 <span style="background:#fff28c">not invertible</span>한 것을 의미한다. 그렇다면 언제 Pivot 값을 못 구할까? 

**[Temporary failure]** Pivot 값은 0이 될 수 없기 때문에 대각선에 0이 있을 때는 Elimination으로 해를 구할 수 없다. -> 다른 행, 같은 열에 0이 아닌 수가 있으면 행을 바꿔서(exchange) 해결할 수 있다.

**[Complete failure]** 다른 행, 같은 열에 0이 아닌 경우가 없으면 Elimination으로 해를 구할 수 없다.





# 2. Back-Substitution

이제 식의 오른쪽 부분(b)을 가져와서 A 행렬에 붙이고 이를 <span style="background:#FFD9EC"> Augmented matrix</span>라고 붙인 후에 다시 Elimination 과정을 시행해보자.


$$
\begin {bmatrix} 1 & 2 & 1 & 2 \\
3 & 8 & 1 &12 \\
0 & 4 & 1 &2\end{bmatrix}
$$

$$
\begin {bmatrix} 1 & 2 & 1 & 2 \\
0 & 2 & -2 & 6 \\
0 & 4 & 1 &2\end{bmatrix}
$$

$$
\begin {bmatrix} 1 & 2 & 1 & 2 \\
0 & 2 & -2 & 6 \\
0 & 0 & 5 & -10\end{bmatrix}
$$


이렇게 구한 행렬을 이제 방정식으로 써본 후 해를 구해보자. 아래부터 해를 구하기 때문에 Back-substitution이라고 한다.


$$
x + 2y + z = 2 \\
2y -2z=6 \\
5z = -10
$$


식을 풀면 z = -2, y = 1, x = 2가 나온다. 



# 3. Elimination Matrices

위의 연산 과정들(Scaling & Substitution)을 행렬로 표현할 수 있을까?

지난 시간에 우리는 행렬식(Ax)은 Combination of the columns of A라는 개념을 배웠다. 그렇다면 xA는 combination of rows of A라고 볼 수 있을 것이다. 이를 이용해서 Elimination Matrix(혹은 Elementary Matrix)를 구해보자. 

**[STEP 1]** (2,1)위치의 Pivot값을 구하기 위해 행렬 A에 곱해야 하는 Elimination matrix E는 첫 번째 행과 마지막 행은 그대로 유지해야 하기 때문에, 첫 번째 행은 [1, 0, 0]이고 마지막 행은 [0, 0, 1]이다. 두 번째 행은 A행렬의 두 번째 행에서 첫 번째 행\*3한 값을 제거하는 연산을 해야 하기 때문에 (subtract 3 \* row1 from row2) [-3, 1, 0]이다.


$$
E_{21}A= P_{21} 
$$

$$
\begin {bmatrix} 1 & 0 & 0 \\
-3 & 1 & 0\\
0 & 0 & 1 \end{bmatrix} \begin {bmatrix} 1 & 2 & 1 \\
3 & 8 & 1 \\
0 & 4 & 1 \end{bmatrix} = \begin {bmatrix} 1 & 2 & 1 \\
0 & 2 & -2 \\
0 & 4 & 1 \end{bmatrix}
$$


**[STEP 2]** 위의 STEP 1처럼 (3,2)위치의 Pivot 값을 구하기 위해 해야 하는 연산은 세 번째 행에서 두 번째 행에 2를 곱한 값을 제거하는 연산을 해야하기 때문에 E32의 세 번째 행은 [0,  -2, 1]이 된다.


$$
E_{32}E_{21}A=P_{32}
$$

$$
\begin {bmatrix} 1 & 0 & 0 \\
0 & 1 & 0\\
0 & -2 & 1 \end{bmatrix} \begin {bmatrix} 1 & 2 & 1 \\
0 & 2 & -2 \\
0 & 4 & 1 \end{bmatrix} = \begin {bmatrix} 1 & 2 & 1 \\
0 & 2 & -2 \\
0 & 0 & 5 \end{bmatrix}
$$


**[STEP 3]** 이렇게 나온 식
$$
E_{32}(E_{21}A)=U
$$
은 행렬의 결합 법칙(Associative law)에 의해서 
$$
(E_{32}E_{21})A=U
$$
로 쓸 수 있다.



행렬연산에서 결합 법칙은 성립하지만, 교환 법칙(communitive law)은 성립하지 않는다. 여기서 잠깐 샛길로 새면.. 행렬 내에서 행이나 열을 switch해주는 다른 형태의 elemantary matrix인 치환 행렬이 있다. 



## Permutation Matrix(치환 행렬)

Permutation matrix은 단위 행렬의 row를 교환한 것으로 행렬의 행이나 열을 교환(exchange)할 수 있게 한다.

Row operation을 위해 치환 행렬은 **왼쪽**에서 곱해준다.  (행 1과 행 2를 바꿔줌) 


$$
\begin {bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix} \begin {bmatrix} a & b \\ c & d \end{bmatrix} =  \begin {bmatrix} c &d \\ a & b \end{bmatrix}
$$


Column operation을 위해 치환 행렬은 **오른쪽**에서 곱해준다. (열 1과 열 2를 바꿔줌)


$$
 \begin {bmatrix} a & b \\ c & d \end{bmatrix}\begin {bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix} =  \begin {bmatrix} b &a \\ d & c \end{bmatrix}
$$


다시 돌아와서...그렇다면 
$$
E_{32}E_{21}
$$
를 매번 곱해서 Elimination 행렬을 구할까?

매번 Elimination 행렬들을 곱하기 보다는 역행렬을 U에 곱해서 표현하는 것이 더 깔끔할 것이다. 이를 위해 다음 시간에는 Elimination을 undone하는 행렬을 찾기 위해 역행렬을 찾아볼 것이다.







## Glossary

Reduced row echelon form

Permutation matrix

Elementary matrix:  nxn 크기의 단위행렬(*In*)에서 기본행연산(elementary row operation)을 한 번 실행하여 얻어지는 행렬이다. 또한 기본행연산의 존재여부에 따라 [단위 행렬](https://ko.wikipedia.org/wiki/단위_행렬)과 기본행렬로 구분된다.

# Reference

[MIT 18.06 강의](https://www.youtube.com/watch?v=QVKj3LADCnA&list=PLE7DDD91010BC51F8&index=3)

[가우스 소거법의 기하학적 의미](https://angeloyeo.github.io/2019/09/09/Gauss_Jordan.html )
