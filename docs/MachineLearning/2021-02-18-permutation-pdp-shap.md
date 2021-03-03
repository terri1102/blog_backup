---
layout: post
title: "[Machine Learning] 머신러닝 모델 학습 후 중요 특성 설명하기"
date: 2021-02-18
category: [Machine Learning]
parent: Machine Learning
nav_order: 12
tags: [permutation, pdp, shap]
comments: true
---



# Model-Agnostic Methods

모델에 구애받지 않고 독립적으로 모델을 해석할 수 있는 방법.

학습모델과 설명모델을 분리해서 설명할 수 있어서 서로 다른 모델 간에 비교가 가능하다.

XAI의 일환으로 나온 모델 설명 방법이다.



# Permutation Importance

```python
# permutation importance
import warnings
warnings.simplefilter(action='ignore', category=FutureWarning)

import eli5
from eli5.sklearn import PermutationImportance

# permuter 정의
permuter = PermutationImportance(
    model, # model
    scoring='accuracy', # metric
    n_iter=5, # 다른 random seed를 사용하여 5번 반복
    random_state=2
)


#스코어 계산
permuter.fit(X_test_encoded, y_test);

feature_names = X_test.columns.tolist()
pd.Series(permuter.feature_importances_, feature_names).sort_values()

# 특성별 score 확인  #여러 번 섞은 결과임
eli5.show_weights(
    permuter, 
    top=20, # top n 지정 가능, None 일 경우 모든 특성 
    feature_names=feature_names # list 형식으로 넣어야 함
)
```



# Partial Dependence Plot(PDP) 부분의존그림

특성이 타겟 예측에 어떤 영향을 미치는지 알 수 있게 해준다.

```python
!pip install pdpbox
```

### 특성 1개 사용

```python
from pdpbox.pdp import pdp_isolate, pdp_plot

feature = 'feature1'

isolated = pdp_isolate(
    model=model, 
    dataset=X_val_df, 
    model_features=X_val_df.columns, feature= feature1,
	grid_type='percentile', # default='percentile', or 'equal'
    num_grid_points=10) # default=10

pdp_plot(isolated, feature_name=feature);
```

![permutation1]



### ICE(Individual Conditional Expectation) curves

```python
isolated = pdp_isolate(
    model=boosting, 
    dataset=X_val_encoded, 
    model_features=X_val.columns, 
    feature=feature,
    num_grid_points=100, #default = 10
)
```

```python
pdp_plot(isolated
         , feature_name=feature
         , plot_lines=True
         , frac_to_plot=0.01 # ICE curves는 100개 (dataset 사이즈 * 0.01)
         , plot_pts_dist=True )

plt.xlim(20000,150000); #그림 확대
```

![permutation3]



### 두 특성 간 관계

```python
features = ['age', 'education_level']

interaction = pdp_interact(
    model=model, 
    dataset=X_val_df,
    model_features=X_val_df.columns, 
    features=features
)

pdp_interact_plot(interaction, plot_type='grid', 
                  feature_names=features);
```

![permutation2]



# Shapley

단일 샘플이나 일부 샘플의 타겟값 예측에 어떤 특성이 얼마나 영향 끼치는지 알려준다.

```python
!pip install shap
```

```python
row = X_test_encoded.iloc[[2]]

import shap

explainer = shap.TreeExplainer(model) #그냥 Explainer 써도 됨
shap_values = explainer.shap_values(row)

shap.initjs() #셀 마다 붙여줘야 함
shap.force_plot(
    base_value=explainer.expected_value, 
    shap_values=shap_values,       
    features=row
)
```



#### 공식 사이트의 예시

explainer는 두 종류가 있는데 기본은 Permutation explainer이고, Exact explainer도 있다. 특성이 더 많을 때 permutation explainer가 더 적합한 것 같다.

(Permutation explainer is the default model agnostic explainer used for tabular datasets that have more features than would be appropriate for the Exact explainer.)



#### Permutation explainer

```
# build a Permutation explainer and explain the model predictions on the given dataset
explainer = shap.explainers.Permutation(model.predict_proba, X)
shap_values = explainer(X[:100])

# get just the explanations for the positive class
shap_values = shap_values[...,1]

shap.plots.bar(shap_values)
```

![permutation7]



**Summary plot**

```python
shap.summary_plot(shap_values, X)
```

![summary plot]



**Force plot**

```python
import shap

explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(row)

shap.initjs()
shap.force_plot(
    base_value=explainer.expected_value, 
    shap_values=shap_values,
    features=row
)
```





#### Exact Explainer

**막대그래프**

```python
# build an Exact explainer and explain the model predictions on the given dataset
shap.initjs()
explainer = shap.explainers.Exact(model.predict_proba, X)
shap_values = explainer(X[:100])

# get just the explanations for the positive class
shap_values = shap_values[...,1]
shap.plots.bar(shap_values)
```

![permutation4]



**색 조절한 beeswarm plot**

```python
shap.initjs()
import matplotlib.pyplot as plt
shap.plots.beeswarm(shap_values, color=plt.get_cmap("cool"))
#shap.plots.beeswarm(shap_values, order=shap_values.abs.max(0)) 디폴트 order
```

![permutation6]



**한 샘플 그래프 waterfall**

```python
shap.initjs()
shap.plots.waterfall(shap_values[0])
```

![permutation5]





---------



## References

https://elapser.github.io/machine-learning/2019/03/08/Model-Agnostic-Interpretation.html

https://www.kaggle.com/dansbecker/shap-values

https://www.kaggle.com/dansbecker/partial-plots

https://shap.readthedocs.io/en/latest/