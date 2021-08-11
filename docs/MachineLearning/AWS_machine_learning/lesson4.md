---
layout: default
title: Lesson 4
date: 2021-08-05
grand_parent: Machine Learning
parent: AWS Machine Learning course
nav_order: 4
---



# Software Engineering

GOALS
* 깨끗하고 모듈화된 코드 작성
* 코드 효율성 증대
* 효율적 다큐먼트 작성
* 버전 컨트롤 사용



## 1. Clean and Modular code

프로덕션 레벨의 코드를 작성하는 법



Clean: readable, simple, concise

Modular: functions와 modules로 나누어야 함. 모듈은 파일 이름-> import로 불러올 수 있음.



## 2. Refactoring Code

Restructuring your code to improve its internal structure without changing its external functionality



## 3. Clean code

변수명을 알아보기 쉽게 적자. 좀 길더라도 한 눈에 알아보기 쉬운 이름으로 지어야 함. 그래도 너무 길면 안 됨...

```python
# bad
[x ** 0.5 * 10]

#good
import math
[math.sqrt(x) * 10] # 더 빠르고 알아보기 쉬움
```

* 리스트 이름은 ages 보다 age_list가 나음.
* 불리언 변수명은 is_integer 이런식으로 is 붙임
* whitespace, 4 spaces, blank 잘 조정. 한 줄은 75 character 이내

[PEP 8](https://www.python.org/dev/peps/pep-0008/?#code-lay-out)



## 4. Write Modular code

1. DRY(Don't repeat yourself) 반복하지 말기
2. 추상화하기

```python
import math
import numpy as np

def flat_curve(arr, n):
    return [i + n for i in arr]

def square_root_curve(arr):
    return [math.sqrt(i) + 10 for i in arr]

test_scores = [90,67,87,98,90]
curved_5 = flat_curve(test_scores, 5)
cursed_10 = flat_curve(test_scores, 10)
curved_sqrt = square_root_curve(test_scores)

for score_list in test_scores, curved_5, curved_10, curved_sqrt:
    print(np.mean(score_list))
```

3. Minimize the number of entities(functions, classes, modules)

4. 하나의 함수는 하나의 일만 해야함! 계산과 프린트하는 것 분리

5. 자의적인 변수 이름이 효과적일 때도 있음 ex) arr, n
6. 함수당 3개 이하의 argument를 사용하는 것이 권장됨

```python
#컬럼이름들 한번에 수정
df.columns = [label.replace(' ', '_') for label in df.columns]

#피쳐 평균 구하기
## Analyzing Features

def numeric_to_buckets(df, column_name):
    median = df[column_name].median()
    for i, val in enumerate(df[column_name]):
        if val >= median:
            df.loc[i, column_name] = 'high'
        else:
            df.loc[i, column_name] = 'low' 

for feature in df.columns[:-1]:
    numeric_to_buckets(df, feature)
    print(df.groupby(feature).quality.mean(), '\n')
```





## 5.  Efficient Code

효율적인 코드란 실행시간이 빠르고, 메모리를 적게 사용하는 코드



## 6. Optimizing Code

### ex 1) Common books

1. Use vector operations over loop when possible

```python
#안 좋은 예
recent_coding_books = []

for book in recent_books:
    if book in coding_books:
        recent_coding_books.append(book)
        
#좋은 예
import numpy as np
recent_coding_books = np.intersect1d(recent_books, coding_books)

#좋은 예2
list(set(recent_books).intersection(coding_books))
```



2. Know your data structures and which methods are faster

set이 list보다 빠른 이유: 해시 테이블 구조를 이용하기 때문. 리스트가 처음부터 탐색하는 것과 다르게 해시테이블은 인덱스를 알면 바로 그 값에 접근 가능하다.



## Documentation

1. inline comments
2. docstring: function
3. project level: readme file



1. in-line Comments : 코드를 이해하는 데 도움을 줌. 코드로 쓰이진 않았지만 실험한 내용들을 설명하기도 함. 코드의 수치를 왜 선정하게 되었는지도 설명.
2. docstring: 모든 함수에 docstring이 있는 것이 좋음

```python
def population_density(population, land_area):
    """Calculate the population density of an area.
    
    Args:
    population: int. The population of the area.
    land_area: int or float. 
    This function is...
    
    Returns:
    population_density: population/land_area"""
    return population / land_area
```



3. Project Documentation

what it does, list its dependencies, and provide sufficiently detailed instructions on how to use it. Make it as simple as possible for others to understand the purpose of your project and quickly get something working

## Version Control in Data Science

1. switch to the develop branch

```git
git checkout develop
```

2. pull the latest changes

```git
git pull
```

3. 새로운 브랜치를 만들고 checkout

```git
git checkout -b demographic
```

4. 변경사항을 커밋함
5. 다시 develop branch로 checkout

```git
git checkout develop
```

6. 새로운 브랜치를 만들고 checkout

```git
git checkout -b friends
```

7. 커밋 후 develop branch에 merge

```git
git checkout develop
```

```git
git merge --no--ff friends
```

8. remote 레포에 push

```git
git push origin develop
```



로그 보기

```git
git log
```

커밋으로 가기 (checkout a commit)

```git
git checkout bc90fkdjsjdlfs
```



A successful git branching model

https://nvie.com/posts/a-successful-git-branching-model/



Model Versioning 

https://algorithmia.com/blog/how-to-version-control-your-production-machine-learning-models

https://towardsdatascience.com/version-control-ml-model-4adb2db5f87c
