---
layout: default
title: "2021년 1월 넷째주 TIL"
date: 2021-01-18
tags: [kaggle]
parent: TIL
nav_order: 1
comments: true
typora-copy-images-to: ..\..\assets\img
---



## 2021.01.18(월)

---

**Fact**: 오늘 스챌은 지난 과제랑 매우 흡사해서 괜찮게 풀었다. 근데 저번주에 배운 개념들로 kaggle 문제 풀었는데ㅋㅋㅋ0.67 나왔다. 캐글 입문 문제인 타이타닉 생존자 예측하는 것이었는데 성별 feature 하나만으로 예측한 것보다도 낮은 점수다. 아마 결측치가 엄청 많은 column 두 개 드랍하긴 했는데, 이걸 amputation처럼? 제대로 처리해야 할 것 같다. cabin 좌석 위치에 대한 사전지식도 필요하고! 

**Feeling** 🥺

**Finding**: one hot encoding 후 feature가 여러 column으로 갈리면 crosstab이 안 되고,  manual하게 하나의 feature를 index로 지정해야 함

랜덤 포레스트 트리모델이라는 알고리즘이 있구나..라는 것. 트리들이 투표해서 다수결로 결정한다는데 궁금하다. 굳이 예습은 안 하는 걸로. 배워야 할 게 많을 때는 너무 깊게 파지 말자.

train data를 보고 feature 분석을 한 후에 test data를 조작하는 거 였구나! train data를 답지라고 생각해서 아예 안 봤는데, 각 feature별로 survived랑 무슨 상관이 있는지 파악을 먼저 해야한다.

**Future Action**: 일주일에 kaggle 문제 하나씩이라도 풀기, 매일매일 꾸준히 블로그 글 쓰기, 조급해 하지말고 조금씩 꾸준히 하기

**Food for thought**: 🍇 결측치 처리...요즘 계속 날 괴롭히는 녀석..나중에, 나중에 imputation 방법 찾아보기

---



## 2021.01.19(화)

**Fact** project 첫 날 데이터 preprocessing 작업을 했다.

**Feeling** 🥺

**Finding**

contains, extract 함수

Series를 typecast할 땐 'string'으로 해야함. 'str' 안 돼!!!

```python
df['column'] = df['column'].astype('string')
```

특정 문자열이 들어있는 것을 확인하는 거...contains, extract, isalpha == True 다 써봤는데, 다 잘못쓰고 있는듯

오늘 함수 만드는 거 한 6번은 실패한 것 같다. 결국 lambda x로 해결했다.

그래도 유용한 거 많이 배웠다..따흑

https://stackoverflow.com/questions/39684548/convert-the-string-2-90k-to-2900-or-5-2m-to-5200000-in-pandas-dataframe

여기에 나랑 똑같은 문제에 직면한 사람이 있어서 답변을 복붙했더니..에러 난다.

문제 원인: df.dtypes해도 object이라고 나왔지만 replace('-', 0)으로 바꾼 column들의 replace된 데이터들 이미 float로 바뀌어 있음. 

즉, 한 column 안에 데이터형이 섞여있게 됨. 근데 dtypes로 보면 object라고 뜸

따라서 일률적으로 str.replace(',','')를 통해 ','를 없애려 했을 때 float을 포함한 column들의 일부 float은 Nan으로 바뀌게 됨->float에 replace 쓰면 Nan됨



* float(series[:-1]) #series 값이 '452M' 이런 식이어도 마지막 자리 빼고 typecast되니 float으로 바꿀 수 있음

* return은 def 안에서 쓰인다. 

* int, float : for loop 안에 못 들어가? 'int' object is not iterable 

​      (list, dictionary, set, string, tuple, bytes가 iterable한 타입입니다.)

**Future Action** regex 공부하기, 너무 어렵다.  기본 문법암기랑 compile 해서 쓰는 법이랑 pattern으로 쓰는 법 두개라도 익히자

**Food for thought**🍔 결측치,,,,,

---



## 21.01.20(수)

**Fact** 오늘은 가설 검정, 시각화 하는 날

**Feeling** 😟💦  공부한 파일 다시 보는데 내가 이걸했단 말이야..?하는 기분만..다 베낀 것도 아닌데 내가 쓴 코드들이 왜 이렇게 낯설게 보이지? 

**Finding**

imputation 고민은 이번 과제에선 필요없던 것이었다...sales value가 0인 게 원자료임

**Future Action**:그래도 imputation 방법 더 찾아보기. iterativeimputer는 다 실험적인 버전이라고 나와있음. 근데 mean, median등 single imputer는 쓰지 말란 말도 많아서 잘 모르겠고, regression을 통해 처리하게 될 듯

**Food for thought** 🧇

https://scikit-learn.org/stable/auto_examples/impute/plot_iterative_imputer_variants_comparison.html

---



### 21.01.22(금)

**Fact**: 발표 망한 거 같아..

**Feeling**: 😰월요일에 동료평가가 걱정된다. 제출이 늦어서 동료평가가 가능할지도 모르겠다.

**Finding**: 생각보다 편집 작업이 오래 걸린다. 어제 영상 편집 간단하게 하는데 컴퓨터 처음으로 느리다고 생각했다. 발표할 때 말을 엄청 더듬는다. 마음이 급하니까 계속 제출 실수했다.

**Future Action**: 미리미리 하자

**Food for thought**:ㅜㅜ



### 21.01.23(토)

**Fact:** 오늘 할 거- for loop, regex 공부, 소프트웨어 개발 3강

**Feeling:**

**Finding:**

```python
map(변환 함수, 순회 가능한 데이터)
```

```python
+ 1 을 +=로도 쓸 수 있다
count += 1   #count = count+1
```

프린트 문 사이에 , 들어가면 스페이스 생김

스페이스 없이 중간에 다른 값 넣고 싶으면 format 쓰기

```python
print('Case #{}:'.format(x), A+B)
```

**Future Action:**

**Food for thought:**



### 21.01.24(일)

list = [] 이걸로 생성하면 0이 들어감..

```python
lst = list(map(int, input().split()))
#입력한 값들이 정수로 리스트에 들어감, 0 안들어감
```

