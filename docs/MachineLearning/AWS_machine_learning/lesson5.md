---
layout: default
title: Lesson 5
date: 2021-08-10
grand_parent: Machine Learning
parent: AWS Machine Learning course
nav_order: 5
---





## Testing and Data Science

데이터 사이언스 분야에서 생기 수 있는 문제들

- Bad encoding
- Inappropriate features
- Unexpected features



**TDD** : writing test before developing

**Unit Tests:** 함수처럼 작은 유닛을 테스팅



## Unit Test

좋은 테스트: repeatable하고 automated된 테스트

Unit Test의 장점: 유닛별 테스트를 하기 때문에 독립적 테스트 가능. 하지만 프로그램 전체를 테스트하기는 어려움. 그럴 때는 integration test

Integration test: https://www.fullstackpython.com/integration-testing.html



## Unit Testing Tools: Pytest

설치

```python
pip install -U pytest
```

테스트할 function

```python
#nearest.py
def nearest_square(num):
    """ Return the nearest perfect square that is less than or equal to num"""
    root = 0
    while (root+1) ** 2 <= num:
        root += 1
    return root ** 2
```

test 파일은 test_ 로 시작하는 이름이어야함

unit test 함수를 test_로 시작하게 만듦

```python
#test_nearest.py

from nearest import nearest_square

def test_nearest_square_5(): #테스트 당 assert 하나만 넣기
    assert (nearest_square(5) == 4)

def test_nearest_square_n12():
    assert (nearest_square(-12) == 0)

def test_nearest_square_9():
    assert (nearest_square(9) == 9)
    
def test_nearest_square_23():
    assert (nearest_square(23) == 16)
```

이렇게 두 파일을 만든 후에 pytest 실행

```bash
$ pytest
```



## Test-Driven Development and Data Science

TDD란? 테스트할 코드를 쓰기 전에 테스트 코드를 작성하는 것.

테스트 코드를 작성하고 여러 케이스를 넣어보면서 빠뜨린 것 있나 확인 



```python
def email_validator(email):
    if (email.count('@') != 0) and (email.count('.') != 0):
        return True
    else:
        return False
```

```python
def test_email_validator():
    assert email_validator('juno@email.com') == True
    assert email_validator('juno@email@.com') == False
```



# Log

### Log message 작성 팁

1. 명확하고 정제된 언어로 작성
2. Logging의 레벨을 잘 정하기

>Debug : 프로그램에서 나타나는 모든 일에 대해서 작성
>
>Error : 오류가 나면 로그 작성
>
>Info :  all actions that are user driven or system specific, such as regularly scheduled operations

3. 주요 정보 작성 ex) 데이터 위치 등



# Code Reviews

**코드 리뷰의 장점:** 

- 에러 찾음
- readability 확인
- standard에 맞췄는지 확인
- 팀내 지식 공유



**코드 리뷰시 유용한 질문들**

### Is the code clean and modular?

- Can I understand the code easily?
- Does it use meaningful names and whitespace?
- Is there duplicated code?
- Can I provide another layer of abstraction?
- Is each function and module necessary?
- Is each function or module too long?

### Is the code efficient?

- Are there loops or other steps I can vectorize?
- Can I use better data structures to optimize any steps?
- Can I shorten the number of calculations needed for any steps?
- Can I use generators or multiprocessing to optimize any steps?

### Is the documentation effective?

- Are inline comments concise and meaningful?
- Is there complex code that's missing documentation?
- Do functions use effective docstrings?
- Is the necessary project documentation provided?

### Is the code well tested?

- Does the code high test coverage?
- Do tests check for interesting cases?
- Are the tests readable?
- Can the tests be made more efficient?

### Is the logging effective?

- Are log messages clear, concise, and professional?
- Do they include all relevant and useful information?
- Do they use the appropriate logging level?



**코드 리뷰시 유용한 팁**

1. code linter 사용
2. 코드제안을 할 때는 이 방법으로 했을 때의 효과까지 설명

3. 객관적인 커멘트하기
4. 코드 예시도 제시하기

**Best Practices** https://github.com/lyst/MakingLyst/tree/master/code-reviews

https://www.kevinlondon.com/2015/05/05/code-review-best-practices.html



