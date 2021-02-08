---
layout: post
title: "[Machine Learning]모델의 성능을 평가하는 지표(범주 분류 모델)"
date: 2021-02-06
category: [Machine Learning]
MachineLearning : true 
excerpt: "혼동행렬, Accuracy, Recall, Precision, F1-score"
tags: [혼동행렬, Accuracy, Recall, Precision, F1-score]
comments: true
---





# ML 모델의 성능을 평가하는 지표(범주 분류 모델)

예전에 다른 포스팅에 쓴 적 있는 것 같지만 다시 정리해야 할 것 같아서 적어 본다.

 

### Confusion matrix(혼동 행렬)

| n= number of subjects | Positive(Predicted) | Negative(Predicted) |
| --------------------- | ------------------- | ------------------- |
| **Positive(Actual)**  | True Positive(TP)   | False Negative(FN)  |
| **Nagative(Actual)**  | False Positive(FP)  | True Negative(TN)   |

주로 classification 모델의 성능을 평가하고 싶을 때 많이 사용된다. 

두 가지 class일 수도 여러 class일수도 있음. 여러 class일 때는 class A vs not A 이런 식으로 비교



#### sklearn에서 확인

```python
from sklearn.metrics import confusion_matrix
print(confusion_matrix(y_true, y_pred,labels=None, sample_weight=None, normalize=None))
```

#### heatmap

```python
import seaborn as sns

sns.heatmap(confusion_matrix, annot=True)
```



Performance Measures

Classification: Simple Accuracy, Precision, Recall, F-beta measure, ROC(and AUC)



Regression: Sum of Squares Error, Mean Absolute Error, RMS Error

### Accuracy(정확도)

올바르게 예측된 데이터의 수를 전체 데이터의 수로 나눈 값


$$
Accuracy = \frac{TP+TN}{TP+TN+FP+FN}
$$

**장점** 이해하기 쉽고 간편해서 많이 사용된다

**단점**: 정확도 역설(Accuracy Paradox) 실제 정답이 한쪽으로 치우쳐 있다면 하나로 찍는 모델의 점수가 높게 나오게 된다.



### Recall(재현율)(=Sensitivity)

실제 True일 때 모델이 True로 예상한 비율. 맞다고 분류해야 하는 건수 중에서 분류기가 몇 개나 제대로 분류했는가.


$$
Recall = \frac{TP}{TP+FN}
$$
**장점:**  실제 데이터에 Negative 비율이 높아서 희박한 가능성으로 발생할 상황에 대한 분류에 적합하다. 

좀 관대하게 암이라고 예측함. recall 을 높이면 암이 아닌 사람들도 암이라고 하게 된다.

**단점:** 항상 True로 예상하는 모델의 recall은 1이다.



### Precision(정밀도)

모델이 True로 예상한 데이터 중 실제 True인 것의 비율. 분류기가 맞다고 분류한 건수 중에서 실제로 맞는 건수의 개수
$$
Precision = \frac{TP}{TP+FP}
$$
Recall과 trade-off 관계

**장점:** 실제 데이터에 Positive 비율이 높을 때 더 잘 맞는다.

**단점:**  항상 Negative로 예상하는 모델의 Precision은 0/0(not defined) 이다.

암 진단 관련해서 1차 검진(recall이 더 중요), 2차 검진(precision이 중요해짐. 진짜 true인 거)



### F beta score

$$
F_\beta = (1+\beta^2)* \frac{precision*recall}{(\beta^2*precision)+recall}
$$

beta가 0.5: precision보다 recall 중시

beta가 2.0: recall이 precision보다 중시

beta를 키우면 recall의 영향을 더 많이 받음



### F1 Score(beta=1.0) 

balance the weight on precision and recall

Precision과 Recall의 조화평균을 낸 것

두 시스템을 비교할 때 precision과 recall이 각각 큰 모델이 다를 때, 비교할 수 있게 해줌

Precision과 Recall 둘 다 사용하기 때문에 두 지표의 단점을 보완한다.
$$
F1 score = 2*\frac{Precision*Recall}{Precision+Recall}
$$



#### F2 score(bata = 2.0) 

less weight on precision, more weight on recall



### Sklearn에서 확인

```python
from sklearn.metrics import accuracy_score, recall_score, precision_score, f1_score

print(accuracy_score(y_true, y_pred))
print(recall_score(y_true, y_pred))
print(precision_score(y_true, y_pred))
print(f1_score(y_true, y_pred))
```



## ROC(and AUC)

AUC 점수는 주로 n_estimator나 threshold 바꿔가며 모델 튜닝할 때 많이 쓰는 것 같고 f1 score는 모델이 완성된 후에 최종 평가할 때 주로 쓰는 것 같다. 물론 f1 score로 threshold 모델 튜닝해도 된다. 생각보다 AUC 점수가 의미있는 게 n_estimator를 



[^ ]: 출처: https://eunsukimme.github.io/ml/2019/10/21/Accuracy-Recall-Precision-F1-score/
[^ ]: http://hleecaster.com/ml-accuracy-recall-precision-f1/