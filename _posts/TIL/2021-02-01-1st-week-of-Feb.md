---
layout: post
title: "2021년 2월 첫째주 TIL"
date: 2021-02-01
categories: [TIL]
excerpt: "2021.02.01-2021.02.07까지 TIL"
permalink: /TIL/2021-02-01-1st-week-of-Feb/
tags: [TIL]
TIL: true
Jan21: true
comments: true
typora-copy-images-to: ..\..\assets\img
---



## 2021.02.01(월)

---

**Fact**: Ridge regression을 배웠다.

**Feeling**: 🤯 와...진짜 제일 어렵다

**Finding** 아직 제대로 이해를 못 해서 finding에 적을 게 없다.

내가 ridge regression을 어려워 한다는 것?? 일단 엄청 헤맨 이유는ridgecv로 alpha값을 여러개 넣어서 돌리는데 alpha값이 분명히 0은 아닌데 작은 값을 넣어도 넣어도 계속 mae가 내려가서 당황했던 것 같다. 근데 간단한 cv로는 아예 다른 값이 나와서..이해없이 복붙하다보니 그렇게 된 것 같다.

pipeline을 이해 못 했는데 뭔가 여기에 저장된 값 때문에 엉킨 것 같다.

**Future Action** ridge, lasso, elasticnet 더 알아보기

ridge와 ridgecv을 어떻게 다르게 쓰는지 좀 정리하자. 난 과제할 때 모델 3개를 만들었는데, 두 개는 ridgecv로 한 듯.

**Food for thought**: 이상치 찾을 때 boxplot이나 errorbar로 그려보고...얼마나 잘라내야 할지 고민해보기. 무조건 상위, 하위 5%씩 자르는 건 아닌듯. 이번 과제도 상위만 좀 잘라냈어야 하는데, 깊게 생각하지 않을 것 같다.

모델의 람다값을 찾고 마지막에 전체 데이터로 훈련 시킬지 안 시킬지...이거는 선택이겠지만, 뭔가 가이드라인이 있을 것 같아
