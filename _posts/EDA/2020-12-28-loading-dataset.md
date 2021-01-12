---
layout: post
title:  "데이터셋 불러오기"
date:   2020-12-28
excerpt: "pandas로 데이터셋 불러오기"
tag:
- EDA
- csv
- pandas
comments: true
---

# EDA 

## 1. 데이터 셋 불러오기

* URL

```python
data_url = 'https://raw.githubusercontent.com/mwaskom/seaborn-data/master/car_crashes.csv'
df = pd.read_csv(data_url, error_bad_lines=False) #error_bad_lines=False: 오류나는 데이터는 불러오지 않음
df.head
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
df.to_csv(file_name, encoding='utf-8')
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
