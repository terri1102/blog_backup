---
layout: post
title: "2021년 2월 첫째주 TIL"
date: 2021-02-01
categories: [TIL]
excerpt: "2021.02.01-2021.02.07까지 TIL"
permalink: /TIL/2021-02-01-1st-week-of-Feb/
tags: [TIL]
TIL: true
comments: true
typora-copy-images-to: ..\..\assets\img
---



## 2021.02.01(월)

---

**Fact**: Ridge regression을 배웠다.

**Feeling**: 🤯 와...진짜 제일 어렵다

**Finding** 아직 제대로 이해를 못 해서 finding에 적을 게 없다.

내가 ridge regression을 어려워 한다는 것?? 일단 엄청 헤맨 이유는ridgecv로 alpha값을 여러개 넣어서 돌리는데 alpha값이 분명히 0은 아닌데 작은 값을 넣어도 넣어도 계속 mae가 내려가서 당황했던 것 같다. 근데 간단한 cv로는 아예 다른 값이 나와서..이해없이 복붙하다보니 그렇게 된 것 같다.

pipeline을 이해 못 했는데 뭔가 여기에 저장된 모델 때문에 엉킨 것 같다.

**Future Action** ridge, lasso, elasticnet 더 알아보기

ridge와 ridgecv을 어떻게 다르게 쓰는지 좀 정리하자. 난 과제할 때 모델 3개를 만들었는데, 두 개는 ridgecv로 한 듯.

**Food for thought**: 이상치 찾을 때 boxplot이나 errorbar로 그려보고...얼마나 잘라내야 할지 고민해보기. 무조건 상위, 하위 5%씩 자르는 건 아닌듯. 이번 과제도 상위만 좀 잘라냈어야 하는데, 깊게 생각하지 않을 것 같다.

모델의 람다값을 찾고 마지막에 전체 데이터로 훈련 시킬지 안 시킬지...이거는 선택이겠지만, 뭔가 가이드라인이 있을 것 같아



## 21.02.02(화)
---

**Fact** 로지스틱 회귀를 배웠다.

**Feeling** ✨

**Finding**

알바 알아보다가 데이터 라벨러라는 노가다 재택 알바를 알게되었다. 단가가 잘 해야 200원인데 되게 간단한 작업들이었다. 이미지 인식 관련한 라벨링작업으로 윤곽과 마디에 라벨을 달아주면 된다. 내가 본 작업들은 두 가지 방식이 있었는데, 사람이 처음부터 이미지에 라벨을 달거나 ai가 단 라벨을 사람이 교정하는 방식이 있었다. 이걸보고 어떤 프로세스로 학습이 이뤄지는지 생각해봤는데, 일단 기계가 훈련데이터로 쓸 데이터 셋에 내가 y값을 제공하는 셈인가??;; 물론 내가 제공하는 y_train 값도 검수가 필요하다. 그래서 회사에서 알바생 검수를 하고 좀 잘하는 사람들한테 계속 일을 주는것 같다.

**Scaler**: minmax 이상치가 많을 경우 영향을 크게 받는다. 이미지 처리할 때는 색이 이상치가 아니기 때문에 minmax 많이 씀. 대부분 standardscaler를 많이 쓰긴 한다 함

**Future Action** 9시 이후로 집중력이 증발하기 때문에 이 시간에 집중하는 방법을 찾아봐야 겠다.

**Food for thought**: 결측치 처리하는 imputation. 이상치(물리적으로 불가능한 값들)를 바꿀 때도 imputation을 쓰는지 아니면 drop하는지...일단 오늘 자료는 전체 샘플 사이즈가 크기 때문에 drop해도 별 문제 없었다. (상위 1%, 하위 0.1%정도) 근데 만약에 진짜 이상한 값들이 들어있는지 작은 사이즈의 데이터 셋이라면 어떻게 해야할까?





## 21.02.03(수)

---

**Fact**: 스챌했던 날, 시간도 모자라서 분석설명을 제대로 못 쓴 것 같다.

**Feeling**: 흠..다들 파일 용량이 엄청 크다니...:disappointed_relieved:

**Finding**

동기분들 과제 보고 배운점! 숫자로 되어 있는 nominal을 원핫인코딩하기 위해 'string'으로 바꾸기 보다는 category 로 데이터 형변환을 하면 이 과정을 생략할 수 있다.

```python
from pandas.api.types import CategoricalDtype
df['column'] = df['column'].astype(CategoricalDtype(ordered=True)) #ordinal data

df['column'] = df['column'].astype('category') #nominal data
```

데이터의 분포를 보고 싶다면 커널밀도 추정을 그려보자. 히스토그램보다 구간설정 영향을 덜 받는다

```python
sns.kdeplot(dataset1, shade=True) #shade는 선 아래를 칠해준다

#이상치 제거 전과 후의 밀도 추정을 그래프로 보여주신 분이 있었는데 데이터 분포가 정규분포 비슷하게 변해가는 모습이 인상적이었다.
```

summary 매서드

```python
df.summary()
```



**Future Action**: 동기분들 과제랑 블로그 더 자주 보기! 과제는 랜덤하게 읽는 편이고, 블로그는 일단 이름순으로 보고 있는데 재밌고 자극도 많이 받게 된다!

**Food for thought**: EDA가 이렇게 까지 중요한지 몰랐다. 앞으로 EDA를 신경 써서 하게 루틴을 만들면 좋을 것 같다.



## 21.02.04(목)

---

**Feeling:**:bed:

**Finding:**  특성 분석?선택?에 너무 시간을 오래 쓰니까 설명을 너무 짧게 쓰고 시각화도 거의 못 한듯...

내가 조합해서 새로 만든 특성이 타당한지 안 한지는 그래프를 그려서 보면 나온다. (이렇게 극명하게 잘못 만들었다는 그래프가 나올 줄은 몰랐다...충격 먹어서 삭제한 특성 복구시켜서 다시 모델 만드니까 정확도가 올랐다ㅋ) 나중에 모델 다 만들고 검증 정확도 비교해도 된다.

이렇게 해도 되는지는 모르겠지만, 처음에 모든 특성 넣고 한번 모델 돌린 후 중요 특성 순위 나오면, 그 순위에 있는 것들을 조합해서 새로운 특성으로 추가하면 가중되서 반영되니까 검증 정확도에 더 좋은 거 아닌가?

주피터 노트북이랑 colab이랑 명령어가 조금씩 달라서 슬프다😩kaggle api도 겨우 설정했는데(보니까 colab이랑 json 저장 위치도 다른듯? 이건 내 colab 디렉토리 문제인듯ㅠㅠ) 명령어도 다시 찾아봐야 된다

**Future Action:** 슬프다고 오래 절망하지 말자

**Food for thought:**Knn imputation이 오래 걸린다는데...난 fancyimpute로 해서 찾아봐야 할 것 같다.



## 21.02.05(금)

**Fact**

**Feeling**

**Finding**

**Future Action**

**Food for thought**