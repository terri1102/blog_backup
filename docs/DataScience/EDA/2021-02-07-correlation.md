---
layout: default
title: "[EDA] correlation"
date: 2021-02-07
parent: Data Science
nav_order: 4
tags: [상관계수, 피어슨 계수, 스피어만 계수, 파이,biserial, gamma]
comments: true
---



"데이터의 종류에 따른 상관계수"

Pearson r: 연속 변수들 간 상관계수

선형적 관계 가정



Spearman's r: 서열 척도들 간의 상관계수

연속변수라 하더라도 극단적 값들이 존재하면 스피어만 상관계수 사용

자료의 서열을 정한 후 이 서열들 간 피어슨 상관계수를 계산



Phi(
$$
\Phi
$$
) coefficient(Mathew's correlation coefficient)

두 범주 변수들간 상관계수

각 범주 변수를 0과 1로 바꾼 다음 이 둘 간의 상관계수로 계산할 수 있다.

부호는 의미가 없고 최솟값은 0

Nan 값 있으면 안 돌아감

```python
from sklearn.metrics import mathews_corrcoef
mathews_corrcoef(y_true, y_pred)
```





Tetrachoric 상관계수

범주들간의 상관계수이나 범주들이 인위적으로 이분화된 경우 사용된다.

이분화되기 전 원래 변수는 정규분포를 띠고 있다고 가정한다.



Point-biserial correlation(점이연 상관계수)

하나가 연속변수이고 다른 하나는 이분변수(범주 개수 2개)일 때 사용

이분변수를 0과 1로 코딩한 다음 피어슨 상관계수를 계산하면 상관계수가 됨



biserial correlation(이연 상관계수)

하나가 연속변수이고 다른 하나는 이분변수일 때 사용하지만 이분변수가 원래 연속변수인데 이분화한 경우에 사용한다.

이는 이분화되지 않았을 때 두 연속변수들 간의 상관계수를 추정하는 방식으로 구한다.





[^ ]:https://dohwan.tistory.com/394
[^ ]: http://qpsy.snu.ac.kr/teaching/multivariate/R_V.pdf
[^ ]: https://www.andrews.edu/~calkins/math/edrm611/edrm13.htm#WHY

