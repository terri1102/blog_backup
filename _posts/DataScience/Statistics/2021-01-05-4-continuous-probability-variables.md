---
layout: post
title: "[Statistics] 4. 연속형 확률변수(Continuous probability varaibles)"
date: 2021-01-06
category: [Statistics]
DataScience: true
excerpt: "Examples and code for displaying images in posts."
tags: [sample post, images, test]
comments: true
---



# 1. 연속형 확률변수

---

X가 취할 수 있는 값의 수가(표본 공간의 원소 수가) 무수히 많을 때 X를 연속형 확률변수라 하고, 이는 확률밀도함수를 통하여 정의된다.



#### 확률밀도함수(pdf)

$$
P(a \leq X \leq b) = \int^b_af(x)dx
$$
f(x)가 어떤 확률변수의 확률밀도함수가 되기 위해선 두 가지 성질을 만족시켜야 한다.

1. 모든 실수값 x에 대하여 f(x) >= 0이어야 한다.

2. $$
   \int^\infin_{-\infin}f(x)dx = 1
   $$



#### 누적분포함수(cdf)

$$
F(x)=P(-\infin < X \leq x) = \int^x_{-\infin}f(x)dx
$$



#### 평균과 분산

$$
E(X) = \int^\infin_{-\infin}x*f(x)dx
$$

$$
V(X) = \int^{\infin}_{-\infin}x^2*f(x)dx-(\int^\infin_{-\infin}x*f(x)dx)^2
$$





# 2. 균일분포(Uniform distribution)

---

확률변수 X가 어느 구간(a,b)에서 정의되고, 그 구간에서 확률밀도함수가 똑같은 높이로 일정한 확률분포를 균일분포라고 한다. 

**구간 (a,b)에서 정의된 균일분포의 pdf**
$$
f(x) = \frac{1}{b-a} \;(a \leq x \leq b), \quad 0 \;(otherwise)
$$


**균일분포의 cdf**
$$
F(x) = \int^x_{-\infin}f(x)dx = [ (x <a)일 \;때 \;0 , \quad (a \leq x\leq b)일 \;때\;\frac{x-a}{b-a} \;, \quad (b < x)일\;때\;1]
$$


**균일분포의 평균과 분산**
$$
E(X) = \frac{a+b}{2}, \quad V(X)=\frac{(b-a)^2}{12}
$$


# 3. 정규분포(Normal distribution)

---

#### 정규곡선

정규분포의 확률밀도함수(pdf)의 형태는 종모양의 정규곡선으로 표현된다.

**정규분포의 pdf**
$$
f(x) = \frac{1}{\sigma\sqrt{2\pi}}exp(-\frac{(x-\mu)^2}{2\sigma^2}), \quad -\infin <x < \infin, \; -\infin <\mu < \infin, \;\sigma >0
$$


**정규확률분포의 특징**

1. 정규곡선 식은 평균에서 가장 큰 값을 가진다. 즉 mu가 최빈값이다.
2. 정규곡선은 평균에 대하여 대칭이다
3. 분산에 따라 모양이 달라진다



#### 표준정규분포(standard normal distribution)

평균이 0이고 분산이 1인 정규분포. 표준정규분포를 따르는 확률변수를 Z라고 표기하고 그 pdf는 
$$
\phi(z)
$$
그 cdf는 
$$
\Phi(z)
$$
로 표기한다.



**표준정규분포의 pdf**
$$
\phi(z)=\frac{1}{\sqrt{2\pi}}exp(-\frac{1}{2}z^2), \quad -\infin <z<\infin
$$
다른 연속인 확률변수들처럼 표준정규분포를 따르는 확률변수 Z가 구간(a,b)에 포함될 확률은 구간(a,b)에 해당하는 확률밀도함수 곡선의 아래 부분에 대한 영역으로 표시된다.
$$
P(a \leq Z \leq b) = P(Z\leq b)-P(Z\leq a)= \Phi(b)-\Phi(a)
$$
