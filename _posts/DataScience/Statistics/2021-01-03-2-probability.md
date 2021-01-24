---
layout: post
title: "[Statistics] 2. 확률과 베이즈 정리"
date: 2021-01-08
category: [Statistics]
DataScience: true
excerpt: "빈도론자와 베이지안의 차이와 베이즈 정리의 이해"
tags: [빈도론자, 베이지안, 베이즈 정리]
comments: true
typora-copy-images-to: ..\..\assets\img
---



# 1. 확률

---

**상대적 비율 접근:** 상대적 비율을 계속 기록해 나갈 때 그 수열이 어떤 값으로 수렴해간다면 그 값을 사건이 일어날 확률이라고 정의하는 것, 빈도론자의 접근

**주관적 접근:** 확률이란 각자 생각하고 있는 어떤 사건이 일어날 가능성에 대한 믿음의 정도, 베이지안의 접근 

**상호배반 사건:** 사건 A와 사건 B가 서로 겹치는 부분이 없을 때 ex) 주사위를 던져 2가 나오는 사건과 홀수가 나오는 사건

**독립 사건:** 어느 사건이 다른 사건과 일어나는 확률이 서로 무관할 때, ex) 동전이 앞, 뒤가 나오는 것과 주사위의 어떤 값이 나오는지는 무관

# 2. 베이즈 정리

---

조건부 확률을 구할 때 사용되며, 가설 검정시 Evidence가 주어졌을 때, Hypothesis가 참인 확률이라고 해설할 수 있다.
사전확률(Prior) 초기 예상값 -> 가설과 조건을 만족하는 데이터 비율(Likelihood) -> 사후확률(Posterior)
사후확률는 사전확률을 대체하는 것이 아닌 업데이트하는 것이며 시행을 반복할 수록 사후확률은 더 높아지고 모델은 정교해진다.



**빈도론자와 차이**

빈도론자들은 추론을 할 때 모수를 고정된 것으로 가정하고 변수처럼 다루지 않기 때문에 여기에 확률이 개입되지 않는다. 빈도론적 접근 한번 데이터를 수집한 후 새로운 데이터를 얻게 된다면 이 둘을 하나로 합쳐서 다시 계산해야한다. 

반면, 베이지안들은 알려지지 않은 모수를 확률적으로 접근한다. 사전확률은 고정된 것이 아닌 계속 업데이트되는 것이다. 새로운 데이터는 기존 데이터를 대체하는 것이 아니라 조정하는 것이기 때문에 한번 기존 데이터로 확률을 구하고 나면, 기존 raw데이터 없이 기존확률만으로 새로운 데이터로 사후확률 업데이트가 가능하다. 



**비판점:** 사전확률의 주관성.

이유불충분의 원리(The priciple of insufficient reason)에 의해 정보가 부족할 때 확률이 동일하다고 가정하기 때문에 사전확률이 실제 확률과 얼마나 관련되어있는지 알기 어렵다. 사전확률을 주관적으로 가정하고 이는 비판점이 되기도 하지만 시행 횟수를 늘려서 이를 계속 업데이트 하면 더 정확한 모델링이 가능하다.



**활용**: decision making, lean startup에 사용됨



### 베이즈 정리 증명 

P(AlB)=A given B: B일 때 A인 조건부확률을 구하는 방법 이며, 아래에서는 이해를 위해 A, -A 두 가지 사건만 일어난다고 가정한다.

<img src="C:\Users\Boyoon Jang\Desktop\Repository\terri1102.github.io\assets\img\베이지안.png" style="zoom:67%;" />



**1.식1**
$$
P(A|B) = \frac{P(A\cap B)}{P(B)}
$$

$$
증명1) P(A|B)= \frac{P(A \cap B)}{P(B)} ->이건 조건부확률 P(A|B)의 정의를 수식으로 표현한 것
$$

$$
P(B|A) = P(A)
$$

$$
양변에 P(B)곱해주면 P(A \cap B) = P(A|B)*P(B)
$$

$$
마찬가지로 P(B|A) = \frac{P(B \cap A)}{P(B)}이기 때문에 P(B \cap A) = P(B|A)*P(A)
$$

교환법칙에 따라서 
$$
P(A \cap B) = P(B \cap A)이기에 P(A|B)*P(B) = P(B|A)*P(A)
$$

$$
이를 정리한 것이 P(A|B) = \frac{P(B|A)*P(A)}{P(B)}
$$

조건확률의 정의를 이용하면 곱셈규칙을 바꿔쓸 수 있다. 
$$
 P(A)*P(B|A) = P(A \cap B) 
$$
즉, A와 B의 교집합의 확률은 A일 확률과 A 중에서 B인 것의 확률을 곱한 것과 같다.



 **2. 식2**
$$
P(A|B) = \frac{P(B|A)*P(A)}{P(B|A)*P(A)+P(B|-A)*P(-A)}
$$
위의 증명1 밑에서 두번째 줄의 방정식 오른쪽의 분모 P(B)를 풀어쓰면 식2의 분모가 된다. 증명1 첫줄의 P(B)에 대한 식을 참고하자.

실제 계산할 때는 이 식이 더 자주 쓰인다고 한다. 이 식은 위의 정사각형 면적의 비율로 표시한 확률을 참고하면 쉽게 이해할 수 있다. 1번식에서 곱셈규칙에 의해서 2번식이 도출된다. 분모의 각 요소가 다 다르다는 것을 기억하자. 



**3. 일반화**
$$
		 P(A_i|B) = \frac{P(B|A_i)*P(A_i)}{P(B|A_1)*P(A_1)+P(B|A_2)*P(A_2)+P(B|A_3)*P(A_3)+ ...+ P(B|A_n)*P(A_n)}
$$



### 가설검정의 입장에서 생각해보기

가설(H)을 검정하기 위해 데이터(E:evidence)를 수집했을 때 우리가 알고싶은 확률은 P(HㅣE) :  P(Hypoothesis given the evidence) 이다. 이때  P(H), P(EㅣH), P(Eㅣ-H)를 가지고 조건부확률을 구하게 된다. 즉, 어떤 조건부확률을 구할 때 그 조건 상황이 역으로 되어 있는 확률을 이용하게 된다.



* **조건부확률을 구하기 위해 알아야 할 확률**

1. 사전확률(사전확률의 비율) Prior ->P(H)

2.  가설과 조건을 만족하는 데이터 비율(Class conditional density): Likelihood(가능도, 우도) ->P(EㅣH) 
3.  조건을 만족시키지만 가설이 아닌 비율: P(Eㅣ~H)

$$
P(H|E) = \frac{P(H)P(E|H)}{P(H)P(E|H)+P(-H)P(E|-H)}
$$

<img src="C:\Users\Boyoon Jang\Desktop\Repository\terri1102.github.io\assets\img\bayes theorem.PNG" style="zoom:60%;" />

위의 세가지 확률을 알면 사후확률 P(H|E)을 알 수 있으며 새로운 데이터를 얻어 다시 추론을 하게 될 경우 기존 데이터에서 얻은 사후확률은 사전확률로 반영된다.



### 혼동행령(confusion matrix, contingency matrix, error matrix)

|          | Test Positive      | Test Negative      |
| -------- | ------------------ | ------------------ |
| Positive | True Positive(TP)  | False Negative(FN) |
| Negative | False Positive(FP) | True Negative(TN)  |

민감도, 특이도, 정확도, 개념 헷갈리니 조심

\* Sensitivity and Specificity (민감도 / 특이도) --> 의학분야에서 사용

   Sensitivity : True Positive Rate (TPR)   = TP / (TP + FN)     --> positive 구분 성능

   Specificity : True Negative Rate(TNR) = TN / (TN + FP)

\* Precision and Recall (정밀도 / 재현율)  --> information retrieval 에서 사용

   Recall = Sensitivity

   Precision = TP / (TP + FP)       --> positive rate  의 정확성 (ex. 보안분야)

[^ ]:**[출처]** [Precision(정밀도) / Recall(재현율) / Sensitivity(민감도) / Specificity(특이도) / F1-SCORE / ROC curve(수신자조작특성곡선)](https://blog.naver.com/trimurti/221387953564)|**작성자** [오영제](https://blog.naver.com/trimurti)



MCMC --->좀 더 개념 공부하고 적기



# 3. 수형도(Tree Diagram)

---

복잡한 문제는 수형도로 그리면 더 알아보기 쉬워진다. 

<img src="C:\Users\Boyoon Jang\Desktop\Repository\terri1102.github.io\assets\img\tngudeh.PNG" style="zoom: 67%;" />


