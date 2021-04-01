---
layout: default
title:  "[EDA] 데이터셋 불러오기"
date:   2020-01-15
parent: Data Science
nav_order: 1
tag:
- EDA
- csv
- pandas
comments: true
---

# EDA 

EDA의 목적 및 기능

1. 시각화를 통해 데이터를 직관적으로 이해하기
2. 데이터 셋 이해를 통해 더 나은 가설 세우는 것 도와줌
3. 어떤 통계 검정을 해야하는지 알게 해줌



## 1. 데이터 셋 불러오기

* pd.read_csv
```python
pd.read_csv()
```
*  URL

```python
data_url = 'https://raw.githubusercontent.com/mwaskom/seaborn-data/master/car_crashes.csv'
df = pd.read_csv(data_url, error_bad_lines=False) #error_bad_lines=False: 오류나는 데이터는 불러오지 않음
df.head
```

* 데이터셋 바로 열기

```python
#1
!pip install palmerpenguins
from palmerpenguins import load_penguins_raw 
penguins = load_penguins_raw()

#2
from sklearn.datasets import load_iris
iris = load_iris()
```

* 로컬 파일

```python
from google.colab import files
uploaded = files.upload()
```
* 로컬에서 업로드한 파일 지우기     
```python
!rm file_name.xlsx 
```

* xlsx 파일을 sheet별로 불러오기
```python
df = pd.read_excel(file_url, sheet_name= ['008770 ', '035250'])
df = pd.read_excel('users.xlsx', sheet_name = [0,1,2])
df = pd.read_excel('users.xlsx', sheet_name = None)      
```
* xlsx 파일을 csv로 바꾸기
```python
file_path = 'C:\path1\path2'
df.to_csv(file_path+'file_name', encoding='utf-8') #한글이면 'CP949'
```

* **한글이 들어있는 데이터셋 불러올 때 오류나면 인코딩**

```python
df = pd.read_csv('file_name.csv', encoding='cp949') 
```

* **한 폴더 안에 있는 여러 파일 불러와서 합치기(열이 동일할 때 가능)**

```python
import pandas as pd
import glob

path = r'F:\Ai\dd\Section2\RA'        # r' 이용
#path='C:\\Users\\hvill\\Destop\\'    #escape back slashes
all_files = glob.glob(path + "/*.csv")

li = []

for filename in all_files:
    df = pd.read_csv(filename, index_col=None, encoding='CP949', header=0)
    li.append(df)

frame = pd.concat(li, axis=0, ignore_index=True)
```



## 2. preprocessing

* Transpose하고 첫번째 row를 column_header로 정하기
```python
def clean(file_name):
  df = pd.read_csv(Url_head + file_name).transpose()                           #.transpose() 대신 .T 써도 됨
  column_head = df.iloc[0]
  df = df[1:]
  df.columns = column_head
return df                                                                    #마지막 row만 return하고 싶으면 df[-1:]
```

* 결측치 처리

```python
df.info() #non-null object의 개수 정리해서 column별로 알려줌

df.isnall().sum() #결측치의 개수를 column별로 알려줌
np.sum(pd.isnull(df))

df.fillna(0) #결측치를 0으로 채워줌
```

