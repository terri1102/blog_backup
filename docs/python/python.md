---
layout: default
title: python 기타
date: 2021-03-02
parent: Python
comments: true
nav_order: 2

---



# Python Miscellaneous



### main : 최상위 스크립트 환경

`'__main__'` 은 최상위 코드가 실행되는 스코프의 이름입니다. 모듈의 __name__ 은 표준 입력, 스크립트 또는 대화식 프롬프트에서 읽힐 때 `'__main__'` 으로 설정됩니다.

모듈은 자신의 `__name__` 을 검사하여 메인 스코프에서 실행 중인지를 확인할 수 있습니다. 이 때문에 임포트될 때는 실행되지 않지만, 스크립트로 실행되거나 `python -m` 으로 실행될 때 조건부로 동작하는 공통 관용구를 사용할 수 있습니다:

```python
if __name__ == "__main__":
    # execute only if run as a script
    main()
```

패키지의 경우, `__main__.py` 모듈을 포함 시키면 같은 효과를 얻을 수 있습니다. 모듈의 내용은 모듈이 `-m` 으로 실행될 때 실행됩니다.

```python
if __name__ == "__main__":
#해당 모듈이 임포트된 경우가 아니라 인터프리터에서 직접 실행된 경우에만, if문 이하의 코드를 돌리라는 명령
#__name__: interpreter가 실행 전에 만들어 놓은 글로벌 변수
```

name? 명시된 파일의 파일이름

아래의 예에서 name은 module_script

직접 호출 안 하면 name은 파일이름

직접 호출하면 name은 main 

```bash
python module_script.py 
#직접 호출
```



module_script.py의 예시

```python
def func():
    print("function working")

if __name__ == "__main__":
    print("직접 실행")
    print(__name__)
else:
    print("임포트되어 사용됨")
    print(__name__)
```



module_script.py가 실행될 수 있는 방법

1. 인터프리터에서 직접 실행

```python
python module_script.py
#직접 실행됨 
#__main__
```

2. 다른 모듈에 임포트해서 실행

```python
#파일이름: main.py라고 가정
import module_script.py
module_script.func()
#임포트 되어 사용 
#module_sciprt
```



---

breakpoint()

디버깅할 때 사용

파이썬 이 부분에서 멈춤

터미널에서 파이썬과 상호작용할 수 있게 됨

```bash
$ python filename.py
 #filename.py의 스크립트 내에서 breakpoint()를 넣고 파일 실행함
 
#내부 명령
dir()
__doc__
__package__
__main__
```

---

언어 실행할 때 compiler, interpreter가 사용됨

컴파일러: 컴퓨터가 이해할 수 있는 언어(byte code)로 번역

인터프리터: 파이썬에서 한 줄씩 순서대로 실행 

파이썬에서 

```python
import module.py
#1)현재 경로(같은 폴더 내)를 확인 ./라고 가정
#현재 경로에서 module.py이 있는지 확인함
#2)파이썬 패키지에 있는지 확인
#3)시스템에서 확인(가상환경 사용시 가상환경 확인)site-package, standard library 등
```

 

하위 디렉토리에 있는 스크립트를 import하고 싶으면

```python
from 하위디렉토리 import hello.py
```



---

파이썬은 동적 타이핑 언어

```python
a,b =0
print(a,b)
#cannot unpack non-iterable int object
iterable 예시: list, tuple
숫자는 non-iterable
```



while: indefinite loop

for loop: definite loo

```python
#for: 자동으로 숫자 하나씩 올라감
for i in range(0,5):
    print(i)

#while: 직접 숫자 하나씩 더해줘야 함    
count = 0
while i < 5:
    print(i)
    count =+ 1
```



if는 True(참)일 때 실행한다!

truthy python: 데이터가 존재한다

falsy python: none, 0, 빈 문자열("")->빈칸 있으면 truthy

python 에서는 truthy 값을 통해서 if문 실행 가능

```python
if True:  #True는 자체로 참 ->truthy값이어서 출력됨
    #그 외 1(숫자), 문자 
    print('truthy')
#if None, 0, ' ':이면 출력 안 됨
```

```python
if 15 % 3 == 0: # 15% 3 -> 0 == 0 -> True -> 코드 실행
    print('hello world')
```

```python
#bool([])으로 판단 가능


if []: #빈 리스트이니까 false
    print('hello')
    
if [False, False, False]: #값 있으니까 true
    print('hello') 
```

외울 거면 false 값을 외워야 함

[],  '', 0, False



==과 is 차이

```python
1 == 1  #1은 객체
1은 메모리에 저장해야 함
==은 값을 확인
is은 메모리 주소 확인

False == False
False is False 
둘 다 True 
주소도 같고, 값도 같으니까

x = [1,2,3]
y=[1,2,3]
x is y #False #x, y리스트는 따로 만들어짐
x == y #True

x[0] == y[0] #True #둘 다 메모리에 저장된 1 가리키니까
```



---

리스트와 딕셔너리! 가 중요

리스트는 동적이다 (다른 언어는 선언한 길이 만큼만 가능)

초기 설정한 리스트 길이 보다 더 길게 수정도 가능

파이썬: 메모리를 본인이 관리하지 않아도 되는 편리함

```python
xlist = []
xlist.append(1)
```



튜플과 리스트 차이

튜플은 변경 불가, 메모리 덜 씀



딕셔너리

인덱싱할 때 리스트나 튜플처럼 숫자로 인덱싱 불가...

```python
dict3 = {'a': 1, 'd':2}
dict3[0] #오류
dict3['a'] #1
```

```python
dict3.items()
dict3.keys()
dict3.values()

for key, value in dict3.items():
    print(key, value)
```



Try except

```python
try:
    assert 1 == 1
except AssertionError as e:
    print('오류가 발생했습니다.')
    print(e)
    
#try raise
```



Assert 함수 : 맞는지 안 맞는지 확인

사실이면 아무것도 안 뜨고 거짓이면 AssertionError

```python
        assert False is self.truth_function(' ')
        assert False is self.truth_function('0')
```



warning은 무시해도 되지만...그래도 나중을 위해 해결 가능한 거면 해결하는 게 좋다.





---

클래스

```python
class Pokemon:
	pokemon_a = 'pikachu'

	def print_pika(self):
		print("pika")
        
poke_a = Pokemon()  #poke_a를 인스턴스화

poke_a.print_pika() #=> 'pika' #인스턴스.매서드()
```



--

name

언더바 2개: 파이썬에서 특별한 기능을 하는 함수, 변수

```python
__name__ -> 모듈(파일)의 이름
#단 하나의 예외, 시작 파일의 __name__은 __main__
__main__ 

__init__: 함수
```

```python
if __name__ == '__main__': 시작 모듈(파일)인지 확인
    #엔트리포인트 파악
    #파일의 시작점을 잡는 것(제일 먼저 실행해야 하는 파일)
    #코드를 실행할 떄 엔트리 포인트를 지정하지 않으면 그저 클래스의 나열임. 어디서 시작할지 알려주는 것. 엔트리 포인트 없으면 메모리에 그냥 올라갔다가 내려감? 
    #파이썬스러운 거(객체지향스러운 거): 클래스 선언하고 엔트리 포인트로 시작시키는 거
    #모듈 중에 시작 모듈 알려고 쓰는 것.
    #디버깅이랑은 관계 없음
```

first_file.py 안에서만 __name __ 이 main, 나머지 파일에서 name은 자신의 이름

```python
For the instance to access the name of a class, it has to work its way upward with Foo().__class__.__name__

 Also note that classes have other attributes such as __bases__ that aren't in the class dictionary and likewise cannot be directly accessed from the instance.
```



### 모듈과 패키지의 차이

모듈: 파일

패키지: 모듈의 묶음

라이브러리: 모듈과 패키지의 묶음

from 패키지 import 모듈

from 패키지 import *

*을 쓰더라도 다 불러오는 게 아니라

```python
__all__ = [모듈이름1, 모듈이름2]
여기에 있는 것만 불러와짐
```



---



References

https://medium.com/@chullino/if-name-main-%EC%9D%80-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%A0%EA%B9%8C-bc48cba7f720

https://docs.python.org/ko/3/library/__main__.html