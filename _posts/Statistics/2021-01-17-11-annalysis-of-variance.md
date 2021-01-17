---
layout: post
title: "11. 분산분석"
date: 2021-01-17
category: [Statistics]
excerpt: "분산분석의 의미와 일원분산분석(ANOVA)과 다중비교, 이원분산분석"
tags: [ANOVA, 일원분산분석, 이원분산분석]
comments: true
---

## ANOVA 검정

---

3개 이상의 샘플에 대한 분산 검정

```python
scipy.stats.f_oneway(sample1, sample2, sample3)
```

