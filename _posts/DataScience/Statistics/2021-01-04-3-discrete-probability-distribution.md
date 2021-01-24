---
layout: post
title: "[Statistics] 3. 이산형 확률변수"
date: 2021-01-04
category: [Statistics]
DataScience: true
excerpt: "확률변수와 이산형 확률분포, 평균과 표준편차, 이항분포, 포아송분포"
tags: [이산형확률분포, 이항분포, 베르누이, 평균, 표준편차, 포아송분포, 베르누이]
comments: true
---



# 1. 확률변수(Random variable)

---

**이산형 확률변수(discrete random variable)**: 유한 개이거나 셀 수 있는 값을 갖는 확률변수 ex)주사위 던지기

**연속형 확률변수(continuous random variable)**: 셀 수 없는 값을 갖는 확률변수 ex)공정 소요 시간, 불량률



# 2. 이산형 확률분포(Discrete probability distribution)

---

이산형 확률변수에 대응되는 확률분포를 이산형 확률분포라 한다. 이산형 확률변수 X가 주여졌을 때 이의 확률분포 또는 확률질량함수는 f(x)로 표기되고, 이는 확률변수 X가 값 x를 갖는 확률로 해석할 수 있다.



# 3. 평균과 표준편차(Mean and Standard deviation)

---

X의 평균은 E(X)이며, X의 분산은 E[X-E(X)]^2이다. 즉 X의 평균은 자기 자신, 즉 X의 기댓값이며, X의 분산은 확률변수와 평균의 차이 제곱이다. 

**이산형 확률변수의 분산과 표준편차**


$$
Var(X)= \sigma^2=\Sigma(x-\mu)^2*f(x)=E(X^2)-E(X)^2
$$

$$
SD(X)= \sigma = \sqrt{Var(X)}
$$



# 4. 이항분포(Binomial distribution)

---

**이항분포**: 실험이 오직 두 가지의 결과만을 갖는 경우를 설명



#### 베르누이 시행(Bernoulli trial): 

1. 각 시행은 성공과 실패로 표현될 수 있는 두 가지 결과만을 갖는다. 
2. 각 시행에서 성공의 확률은 p이고, 실패의 확률은 1-p이다.

**베르누이 확률과정(Bernoulli stochastic process)**: 베르누이 실험을 독립적으로 반복하여 나온 결과 

베르누이 시행에서 우리의 주된 관심사는 'n번의 독립적으로 반복된 베르누이 시행' 중에서 성공의 회수이다. 이 성공의 횟수를 확률변수 X라고 하면, X는 0,1,2,3,...,n의 값을 취하는 이산형 확률변수이고, 이 확률변수의 확률분포를 이항분포라 한다.



#### 이항분포의 확률질량함수: 

$$
f(x) = \binom{n}{x}p^x(1-p)^{n-x},x=0,1,...,n
$$

[^ ]: nCx는 "n choose x" 또는 n Combination x



예시: 확률변수 X가 이항분포 B(10, 1/3)을 따를 때, P(X<=2)를 구하라

P(X<=2)  =  P(X=0)  +  P(X=1)  +  P(X=2)


$$
P(X=0)= \binom{10}{0}(\frac{1}{3})^0(\frac{2}{3})^{10}, \quad P(X=1)= \binom{10}{1}(\frac{1}{3})^1(\frac{2}{3})^{9},\quad P(X=2)= \binom{10}{2}(\frac{1}{3})^2(\frac{2}{3})^{8}
$$

$$
참고 \quad nCr = \frac{nPr}{r!}=\frac{n!}{r!(n-r)!}
$$



* 이항분포의 표시: X~B(n, p) 또는 X~Bin(n,p): 확률변수 X는 모수가 (n,p)인 이항분포를 따른다.



```python
np.random.binomial(n, p, size) # n번의 베르누이 시행 중 사건의 발생확률 P인 10개의 수
df = pd.DataFrame({'coinflips': np.random.binomial(n = 1, p = 0.5, size = 10)}) #binominal: 1,0만 나옴
```



#### 이항확률분포의 평균과 분산

$$
E(X) = \mu - n*p
$$

$$
Var(X) = n*p*(1-p)
$$



#### 두 확률변수의 가법성

두 확률변수 U와 V가 서로 독립이고 B(n,p)와 B(m,p)를 따른다고 하면, 이때 U+V의 분포는 이항분포 B(n+m,p)가 된다. 
$$
X\sim Bin(n,p),\quad Y\sim Bin(m,p)이고,\quad X와\; Y가\; 독립이면 \; X+Y \sim Bin(n+m,p)
$$



# 5. 포아송분포(Poisson distribution)

---

고정된 지역, 시간 또는 부피 등에서 관심 있는 사건의 관찰수 또는 발행 횟수 X를 표현하는 데 자주 사용된다.

ex) 어느 하루 동안 지정된 생산라인에서 발생한 불량품의 개수

포아송분포의 확률질량함수
$$
f(x) = \frac{\lambda^xe^{-\lambda}}{x!}, \quad x=0,1,2,..., \quad \lambda>0
$$
포아송분포의 확률질량함수는 
$$
\lambda
$$
에 의하여 결정되고, 모수 
$$
\lambda
$$
를 갖는 포아송확률분포 또는 평균이
$$
\lambda
$$
인 포아송확률분포라고 부른다. 이때 이 확률변수의 평균과 분산은 
$$
\lambda
$$
로 서로 같다. 하지만 평균은 위치를 대표하는 값이고, 분산은 퍼짐이나 변동을 대표하는 값이다. 

**포아송분포와 이항분포의 관계:** 포아송분포는 이항분포 B(n,p)에서 n이 충분히 크고, p가 아주 작은 경우에 만들어지는 근사분포이다. 근사분포로 만들어진 포아송분포의 평균값은 이항분포의 평균값과 같은 
$$
\lambda=np
$$
이 되며, 이항분포의 분산은 
$$
np(1-p)=\lambda\frac{(1-\lambda)}{n}
$$
이 되므로 n이 무한대로 커지면 분산값은 
$$
\lambda
$$
로 수렴한다. 

**포아송분포의 평균과 분산**: E(X) = lambda, V(X) = lambda

#### 포아송 분포의 성질

1. 포아송 분포의 가법성

$$
만약 \; X_1,.., X_n이 \;서로 \;독립인\; 포아송분포이고, 각각의\; 평균이 \lambda_1,.., \lambda_n이라 하면, 
$$

$$
\;X_1+..+X_n은 \;평균이 \lambda_1+..+\lambda_n인 \;포아송분포를\; 따른다.
$$



2. 포아송 분포의 확산성

$$
기본\;단위\;당 \;관심있는 \;사건의 \;발생수가 \;평균이 \;\lambda인\; 포아송분포를\; 따른다면\;
$$

$$
 t단위당 \;발생하는 사건의\;수는 \;평균이 \;t\lambda인 \;포아송분포를\;따른다
$$



```python
np.random.poisson(lam=1.0, size=None)
```

