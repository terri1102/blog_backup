---
layout: default
title: "One Hot Encoding과 원핫인코딩 처리된 데이터의 PCA, 이를 Scree plot으로 그리기"
date: 2021-01-16
category: [Data Science]
parent: Machine Learning
nav_order: 1
tags: [One hot encoding, PCA, scree plot]
comments: true

---



# One Hot Encoding

---

문자를 숫자로 바꾸는 기법 중 하나.

각 단어에 고유한 정수 인덱스를 부여하고 이를 원-핫 벡터로 만든다.



### One Hot Encoding 순서



1. 인코딩해야 하는 feature 파악하기

2. column하나를 정해서 value 뽑아내기

```python
from numpy import array
from numpy import argmax
from sklearn.preprocessing import labelEncoder
from sklearn.preprocessing import OneHotEncoder

#Define example
data = df['column1']          #column 대신 직접 encoding할 value 적어도 된다. data ='a','b','c'
values = array(data)
#print(values)
```

3. Integer encode

```python
label_encoder = LabelEncoder()
integer_encoded = label_encoder.fit_transform(values)
print(integer_encoded)
>>>[0 1 2]
```

4. Binary encode

```python
onehot_encoder = OneHotEncoder(sparse=False)
integer_encoded = interger_encoded.reshape(len(interger_encoded), 1)
onehot_encoded = onehot_encoder.fit_transform(interger_encoded)
print(onehot_encoded)
```

5. Get dummies

```python
df = pd.get_dummies(data = df, columns = ['column1']), prefix = 'column1'
#column1을 dummy 데이터가 대체하게 된다. 데미데이터의 column 이름 앞에 column1이 붙음
```
```python
#더미코딩: 원핫인코딩하고 하나 줄인 것, 정보의 양 동일한데 컬럼 하나 줄인 거
df_dum = pd.get_dummies(df, prefix=['City'], drop_first=True)  
#자유도 개념까지 봤을 때 이렇게 하는 것이 더 좋음? 통계학적으로 봤을 때 쓸데없는 정보를 줄인 것
df_dum
```

6. 더 바꾸고 싶은 column이 있으면 이를 계속 반복한다.

* 만약 결측치가 있다면...? 일단 'nan'으로 바꿨는데 이게 최선인가?

코엔엘파이의 Okt 형태소 분석기를 통해 문장을 통째로 넣은 후 단어별로 잘라서 분석하는 것도 가능하다.

---



# One hot encoding을 수행한 데이터로 PCA

---



1. 데이터 프레임을 array로 바꾼다.

```python
new_df = pd.DataFrame.to_numpy(df)
```

2. 정규화 시키기

```python
from numpy import array          #얘는 어디쓰이는지 잘 모르겠네
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

scaler = StandardScaler()
Z = scaler.fit_transform(new_dr)
#print("\n Standardized Data:\n", Z)
```

3. PCA

```python
pca = PCA(2)                #PC의 개수 설정, 나중에 principalDf의 column 개수랑 맞춰야 함
pca.fit(Z)
```

4. PCA components

```python
principalComponents = pca.fit_transform(Z)
principalDf = pd.DataFrame(data=principalComponents, columns = ['PC1, PC2'])
#print("\n Eigenvectors: \n", pca.components_)
print("\n Eigenvalues: \n", pca.explained_variance_)
print("\n Explained variance ratio: \n", sum(pca.explained_variance_ratio_))
```

---



# Scree plot

---

```python
def scree_plot(pca):
    num_components = len(pca.explained_variance_ratio_)
    ind = np.arange(num_components)
    vals = pca.explained_variance_ratio_
    
    ax = plt.subplot()
    cumvals = np.cumsum(vals)
    ax.plot(ind, cumvals, color = '#b4cccc')
    ax.bar(ind, vals, color = ['#ffa6b6', '#ffc67a',  '#b3ff70', '#66faf7', '#c591ff'])
    for i in range(num_components):
        ax.annotate(r"%s" % ((str(vals[i]*100)[:3])), (ind[i], vals[i]), va = "bottom", ha = "center", fontsize = 13)
     
    ax.set_xlabel("Number of PC")
    ax.set_ylabel("Variance")
    plt.title('Scree plot')
    
scree_plot(pca)
```

<img src="C:\Users\Boyoon Jang\Desktop\Repository\terri1102.github.io\assets\img\screeplot.PNG" style="zoom:67%;" />



category_encoder()

```python

```

