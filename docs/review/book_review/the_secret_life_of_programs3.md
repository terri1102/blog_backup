---
layout: default
title: 한 권으로 읽는 컴퓨터 구조와 프로그래밍 3장
parent: 한 권으로 읽는 컴퓨터 구조와 프로그래밍
Grand_parent: Book Reviews
nav_order: 3


---





<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/slp1.jpg?raw=true" alt="slp1" style="zoom:33%;" />

한 권으로 읽는 컴퓨터 구조와 프로그래밍

조너선 스타인하트 지음. 오현석 옮김. 책만



:toc



# 3장 메모리와 디스크의 핵심: 순차 논리

-컴퓨터는 비트를 어떻게 기억하는가?



## 순차논리(Sequential Logic)

2장에서 배운 '조합 논리'는 입력에 의해서만 출력이 결정된다. 조합 논리만으로는 연산의 부분결과를 기억해 둘 수 없기 때문에 `순차 논리`가 필요하다. 이런 순차 논리를 위해 디지털 회로에서 시간을 만들어내는 것이 필요해진다.



## 시간 표현과 상태기억

주기 함수를 이용해 시간을 측정할 수 있는 것처럼 컴퓨터는 '주기적'인 전기 신호를 이용해서 시간을 측정한다.



### 1. 발진자(Oscillator)

인버터를 이용한 출력을 다시 입력에 넣는 것을 되먹임(feedback)이라고 하는데, 중간에 인버터를 거치기 때문에 출력값은 0과 1 사이를 진동한다(oscillate). 이 발진자를 사용하면 시간을 측정할 수 있다.



**수정 발진기(결정 진동자, crystal oscillator)**





### 2. 클록

시간이 중요한 (저수준의) 이유는 전파 지연이 회로가 작업을 수행하는 속도에 영향을 미치기 때문에 시간을 잴 수 있으면 결과가 안정적이고 올바르다고 확신할 수 있는 시점이 될 때까지 최악의 경우를 가정해 가산기의 지연시간을 기다릴 수 있다. 



진동의 개수를 세면 시간을 나타낼 수 있다. **클럭**이란 CPU의 속도를 나타내는 단위로 순차회로에 가해지는 진동의 속도를 나타내며 Hz로 표기한다. 클럭은 1초 동안 파장이 한 번 움직이는 시간을 의미하는데 이 시간 동안 처리하는 데이터 양에 따라 CPU의 속도가 달라지게 된다.



https://library.gabia.com/contents/infrahosting/1227/



발진자는 컴퓨터에 클럭(clock)을 제공한다. 회로의 최대 클럭 속도나 가장 빠른 템포는 회로의 전파 지연 시간에 의해 결정된다. 



> 컴퓨터 컴포넌트 제작에는 여러 통계가 필요하다. 컴포넌트를 이루는 부품들 사이에 편차가 크기 때문이다. 비닝(binning)과정은 부품을 측정해서 그 특성에 따라 여러 다른 빈이나 무더기로 분류한다. 지연 시간이 짧아서 빨리 반응할 수 있는 부품은 가장 가격이 높은 빈에 들어가고, 더 느리고 싼 부품은 다른 빈에 들어가며 이런 분류를 여러 단계의 빈을 사용해 반복한다. 부품 전체의 편차보다 더 작은 편차를 갖도록 부품을 빈에 나눠 담는다. 이런 이유로 부품의 전파 지연 시간을 표시할 때도 범위를 사용한다. 생산자는 전형적인 값과 함께 최솟값과 최댓값을 제공하는데 일반적인 논리 회로 설계 오류는 최댓값이나 최솟값을 사용하지 않고 전형적인 값을 사용하기에 생긴다.

오버 클로킹: 통계적으로 빈의 중간 정도에 위치하는 부품을 부품이 고장나지 않을 범위 안에서 클럭을 빠르게 공급하는 것.





### 3-1. 래치(Latch)

이제 시간을 표현했으니 정보를 1비트 기억할 방법을 생각해야 한다. OR 게이트의 출력을 되먹임하는 OR 게이트 래치는 반전이 없기 때문에 진동이 없어 출력이 하나의 값으로 유지된다. 되먹임을 끊고 회로를 재설정(reset)하기 위해 인버터를 단 AND-OR 게이트 래치는 



* 반전(opposite) 

* 액티브 하이, 액티브 로우:  `액티브`는 출력 기준! `하이, 로우`는 입력 신호 기준!

  입력 신호가 high일 때 동작하게 할 수도 있고(액티브 하이), low일 떄 동작하게 할 수도 있음(액티브 로우)

S-R 래치 :

### 3-2. 게이트가 있는 래치



### 4. 플립플롭

플립플롭 또는 래치는 전자공학



D 플립플롭

D는 데이터 또는 딜레이라고 함. 입력 D의 갑사을 클럭의 엣지에서 캡처해서 Q에 반영한다.엣지가 발생하지 않는 시간에는 Q가 변하지 않고 유지된다.



###### 위키피디아 https://ko.wikipedia.org/wiki/%ED%94%8C%EB%A6%BD%ED%94%8C%EB%A1%AD



### 5. 카운터



### 6. 레지스터





---

## 시간 표현과 상태 기억

변화를 세야 시간을 잴 수 있다.

0과 1을 반복



래치: 빗장 같이 유지함. 클록은 스테이터스를 유지하는데는 적합하지 않음. 내가 필요한 신호에 유지가 필요한 경우일 때 로직을 적용해서 주기를 변경



입력 신호가 하이와 로우를 반복하더라도 출력은 하나로 유지할 수도 있다. 그래서 리셋이 들어와야지만 출력의 set을 clear 시킬 수 있다. (리셋을 통해서 래치를 풀 수 있다)

---









## 게이트가 있는 래치

리셋은 셋의 반대!!


