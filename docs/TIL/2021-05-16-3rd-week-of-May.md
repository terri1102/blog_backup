---
layout: default
title: 2021년 5월 셋째주 TIL
date: 2021-05-16
categories:
  - TIL
tags:
  - TIL
comments: true
nav_order: 10
parent: TIL
---



## 2021.05.16(일)

**Fact:** 요즘 취준 관련해서 이것저것 많이 알아보고 같은 공부하는 사람들 많이 만나고 있는데, 오늘은 자연어처리 공부하는 친구를 만나서 얘기를 나눴다. 이 친구는 석사 논문에서 BERT의 tokenizer에 품사를 추가한 모델을 만들었는데 (정확히 어떤 원리일까?ㅎㅎ) 기존 모델보다도 성능이 더 떨어졌다고 했다. 또한 BERT의 원래 tokenizer는 wordpiecetokenizer인데 이것을 자모 단위로 자르면 성능이 많이 떨어진다는 얘기도 들었다ㅎㅎ...BERT는 wordpiecetokenizer로 xlnet은 sentencepiecetokenizer를 쓰는 것처럼 역시 각 모델에 맞는 tokenizer를 써야 성능이 가장 잘 나오는 것 같다.

**Feeling:** 이것저것 알아보고 동기분들과 이런저런 얘기하다보니 초조함이 좀 줄어드는 것 같다.

**Finding:** 블로그와 깃헙 정리하기! 벌써 깃헙에 깔끔하게 이력 정리하신 동기 분도 계신는데 확실히 보기 좋고, 어떤 사람인지 파악하기에 편하다. 

**Future Action:** 자연어처리와 시계열 데이터 공부하기! 현재 읽고 있는 책: 실전 시계열 분석(O'raily)

**Food for thought:** :tropical_drink: 자연어처리에서 text generation을 아직도 잘 모르겠다. 이 부분에서 한국어를 어떤 단위로 해야할지..더 조사하고 생각해보기! 오늘 들은 얘기로는 음절단위로 주로 한다고 듣긴했다. 