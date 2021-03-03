---
layout: post
title: "[Machine Learning] 모델의 성능을 평가하는 지표(범주 분류 모델)"
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



#### plot_confusion_matrix

```python
from sklearn.metrics import plot_confusion_matrix
import matplotlib.pyplot as plt

#estimator, X, y_true, *, labels=None, sample_weight=None, normalize=None, 
#display_labels=None, include_values=True, xticks_rotation='horizontal', values_format=None, cmap='viridis', ax=None, colorbar=True)

fig, ax = plt.subplots()
pcm = plot_confusion_matrix(pipe, X_val, y_val,
                            cmap=plt.cm.Blues,
                            ax=ax);
plt.title(f'Confusion matrix, n = {len(y_val)}', fontsize=15)
plt.show()
```



#### plt_confusion_matrix에서 테이블 가져오기

```python
cm = pcm.confusion_matrix
cm[1][1]
#>>TP  #이건 설정에 따라 바뀔 수 있음
```



#### heatmap

```python
import seaborn as sns

sns.heatmap(confusion_matrix, annot=True)
```



### Performance Measures

**Classification:** Simple Accuracy, Precision, Recall, F-beta measure, ROC(and AUC)

**Regression:** Sum of Squares Error, Mean Absolute Error, RMS Error



### Accuracy(정확도)

올바르게 예측된 데이터의 수를 전체 데이터의 수로 나눈 값


$$
Accuracy = \frac{TP+TN}{TP+TN+FP+FN}
$$



**장점** 이해하기 쉽고 간편해서 많이 사용된다

**단점**: 정확도 역설(Accuracy Paradox) 실제 정답이 한쪽으로 치우쳐 있다면 하나로 찍는 모델의 점수가 높게 나오게 된다.



### Recall(재현율)(=Sensitivity)

`실제` True일 때 모델이 True로 예상한 비율. 맞다고 분류해야 하는 건수 중에서 분류기가 몇 개나 제대로 분류했는가. 양성 항목 검출율.

$$
Recall = \frac{TP}{TP+FN}
$$

**장점:**  FN의 비용이 높을 때 사용된다.  즉 암인데 암이 아니라고 판정하는 것이 암이 아닌데 암이라고 판정하는 것보다 더 심각한 문제이기 때문에, 좀 관대하게 암이라고 예측한다. recall 을 높이면 암이 아닌 사람들도 암이라고 하게 된다. 

**단점:** 항상 True로 예상하는 모델의 recall은 1이다.



### Precision(정밀도)

모델이 True로 `예상`한 데이터 중 실제 True인 것의 비율. 분류기가 맞다고 분류한 건수 중에서 실제로 맞는 건수의 개수. 양성 항목 정답률. 주로 모델의 관점에서 모델 평가할 때 쓰임
$$
Precision = \frac{TP}{TP+FP}
$$


Recall과 trade-off 관계

**장점:**  FP의 비용이 높을 때 사용한다. 예시로 이메일 스팸을 검출하는 것이 있다. (스팸인 것: Positive) 중요 이메일을 스팸으로 분류하는 것의 비용이 높기 때문에, precision을 높여서 검출한다. 

**단점:**  항상 Negative로 예상하는 모델의 Precision은 0/0(not defined) 이다.

암 진단 관련해서 1차 검진(recall이 더 중요), 2차 검진(precision이 중요해짐. 진짜 true인 거)



### F beta score

$$
F_\beta = (1+\beta^2)* \frac{precision*recall}{(\beta^2*precision)+recall}
$$

beta가 0.5: precision을 recall 보다 중시

beta가 2.0: recall이 precision보다 중시

beta를 키우면 recall의 영향을 더 많이 받음



### F1 Score(beta=1.0) 

Precision과 Recall을 같은 비중으로 고려한다.

Precision과 Recall의 조화평균을 낸 것

두 시스템을 비교할 때 precision과 recall이 각각 큰 모델이 다를 때, 비교할 수 있게 해준다.

Precision과 Recall 둘 다 사용하기 때문에 두 지표의 단점을 보완한다.
$$
F1 score = 2*\frac{Precision*Recall}{Precision+Recall}
$$

$$
F1 score = \frac{2TP}{2TP+FP+FN}
$$



#### F2 score(bata = 2.0) 

Recall을 Precision보다 강조하는 지표이다.



### Sklearn에서 확인

```python
from sklearn.metrics import accuracy_score, recall_score, precision_score, f1_score

print(accuracy_score(y_true, y_pred))
print(recall_score(y_true, y_pred))
print(precision_score(y_true, y_pred))
print(f1_score(y_true, y_pred))
```



#### 한꺼번에 여러 지표 확인

```python
from sklearn.metrics import classification_report
print(classification_report(y_val, y_pred)) 
```



## 임계치 조정(threshold)

만약 class 분포가 치우쳐 있다면 threshold 조절을 통해서 모델의 예측력을 높일 수 있다. 

```python
#pipe든 model 인스턴스 든 이미 모델이 만들어졌다고 가정하자
pipe.classes_  #class 종류
pipe.predict(X_val)  #1이나 0으로 예측
y_pred_proba = pipe.predict_proba(X_val)[:, 1] #타겟값을 1로 예측할 확률
```

임계치 조정은 이미 모델을 다 만든 후에 하는 것이고, 모델을 만들 때 class_weight='balanced'를 통해서 치우친 분포를 고려할 수도 있다. 

threshold 구하는 법은 아래 참고.



## ROC(and AUC)



**ROC 그래프**

y축: True Positive Rate(Sensitivity)

x축: False Positive Rate(1-Specificity)
$$
TRP = Sensitivity = \frac{TP}{TP+FN}
$$

$$
FPR = 1 - Specificity = \frac{FP}{FP+TN}
$$

|                     | Actual_True | Actual_False |
| ------------------- | ----------- | ------------ |
| **Predicted_True**  | TP          | FP           |
| **Predicted_False** | FN          | TN           |



모든 값을 True로 예측할 때(역치는 0) FN은 0이기 때문에 TPR = 1

모든 값을 True로 예측할 때 TN은 0이기 때문에 FPR = 1

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/img/roc1.png?raw=true" alt="roc1" style="zoom:67%;" />

역치를 높히면 True로 분류될 확률이 떨어지기 때문에 FP이 감소하고, TN이 증가한다. 따라서 FPR이 감소한다. 

하지만 역치가 너무 올라가면 TPR이 떨어지게 된다. 역치가 1이면 FP=0이다.

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/img/roc2.png?raw=true" alt="roc2" style="zoom:67%;" />

ROC 그래프는 역치별 혼동행렬을 요약해서 보여준다.



<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/img/roc3.png?raw=true" alt="roc" style="zoom:67%;" />

negative 값들의 평균을 감소시키면 positives와 negatives의 분포 그래프가 점점 멀어지면서 ROC 곡선은 더 오목해진다. 여기서 오목해진다는 것은 우측하단을 원점으로 두었을 때 오목하다는 것이다.  가운데의 threshold 선을 조절하면 positive나 negative로 분류하는 비율을 조절할 수 있다.



AUC 점수는 주로 n_estimator나 threshold 바꿔가며 모델 튜닝할 때 많이 쓰는 것 같고 f1 score는 모델이 완성된 후에 최종 평가할 때 주로 쓰는 것 같다. 물론 f1 score로 threshold나 하이퍼 파라미터를 튜닝해도 된다. 생각보다 AUC 점수가 의미있는 게 n_estimator를 바꿔가며 두모델을 비교했었는데, 검증 정확도(accuracy)가 낮은데도 AUC 점수가 높은 모델의 테스트 f1 score가 더 높았다. AUC, ROC는 이진분류에만 쓰인다.



**AUC:** ROC 아래 면적을 의미하며 모델 별 ROC 비교를 쉽게 해준다.  랜덤포레스트와 로지스틱 모델도 비교 가능하다.



FPR를 Precision으로 바꾼 PPV(Positive Predictive Value)를 쓰기도 한다. 

Precision: Negative가 많을 때? TN을 포함하지 않음->불균형에 영향을 안 받음



### fpr, tpr, thresholds, roc_curve

```python
from sklearn.metrics import roc_curve

# roc_curve(타겟값, prob of 1)
fpr, tpr, thresholds = roc_curve(y_val, y_pred_proba)

roc = pd.DataFrame({
    'FPR(Fall-out)': fpr, 
    'TPRate(Recall)': tpr, 
    'Threshold': thresholds
})
roc
```



### tpr과 fpr의 차이가 가장 큰 지점: 최적화된 threshold

```python
optimal_idx = np.argmax(tpr - fpr)         #가장 차이가 큰 지점을 찾으면 최적화된  threshold가 나옴
optimal_threshold = thresholds[optimal_idx]

print('idx:', optimal_idx, ', threshold:', optimal_threshold)
```



### AUC score

```python
from sklearn.metrics import roc_auc_score
auc_score = roc_auc_score(y_val, y_pred_proba)
auc_score
```





**Reference**

statQuest

 https://eunsukimme.github.io/ml/2019/10/21/Accuracy-Recall-Precision-F1-score/

http://hleecaster.com/ml-accuracy-recall-precision-f1/

http://www.navan.name/roc/