---
layout: default
title: 객체지향 프로그래밍과 파이썬 
date: 2021-05-12
parent: Python
nav_order: 1
tags: [OOP, Python]
category: [Python]
comments: true
---

# OOP 간단 정리
## 객체 지향 프로그래밍
객체 지향 프로그래밍은 컴퓨터 프로그램을 (명령어의 목록이 아닌) 여러 개의 독립된 단위, 즉 "객체"들의 모임으로 파악하고자 하는 것이다. 각각의 객체는 메시지를 주고받고, 데이터를 처리할 수 있다.


### OOP의 장점, 필요성
객체 지향 시스템의 힘은 **코드 재사용**이 용이하다는 것에 있다. 이는 생산성을 높이고 비용을 줄이며 소프트웨어 품질을 향상시킨다. 

비교 개념: 절차형 프로그래밍, 함수형 프로그래밍

주의: 상속이 많아질수록 가독성이 떨어짐

### 클래스와 오브젝트
클래스: 객체들의 상태(속성)와 행동(매서드)을 정하는 설계도

오브젝트: 클래스가 메모리에 할당되고 구체화된 것 


## OOP의 요소: 캡슐화, 상속과 포함, 추상화, 다형성

### 캡슐화(Encapsulation)
캡슐화는 내부 속성(변수)과 함수를 하나로 묶어서 클래스로 선언하는 일반적인 개념이다. 이렇게 클래스로 하나로 묶어서 사용하면 재사용이 용이하고 코드도 간단해진다. 또 캡슐화의 결과로 은닉성이 있는데, 접근 제어 등을 통해서 객체 및 소스코드 구현에 대한 상세정보를 분리하는 과정이다.


### 상속과 포함(Inheritance & Composition)
상위 클래스의 변수나 함수를 재사용할 수 있게 해주는 것을 의미한다.
상속: 상위 클래스의 모든 기능(함수, 변수)을 재사용할 수 있다.
포함: 다른 클래스의 일부 기능(함수)만을 재사용한다.

### 추상화(Abstraction)
오브젝트는 추상클래스(상위클래스)를 상속받아 개별적으로 클래스(하위클래스)를 생성하는데, OOP에서 추상화는 프로그램을 구성하는 객체들이 어떤 필드들과 메소드들을 갖는지를 (실세계의 객체로부터) 추출함을 의미한다.
추상화는 복잡한 자료, 모듈, 시스템 등으로부터 핵심적인 개념 또는 기능을 간추려 내는 것을 말한다.


### 다형성(Polymorphism)
상위 클래스로부터 상속받은 변수나 함수를 하위 클래스에서 바꿔서 사용할 수 있다.

---

## 클래스 변수, 인스턴스 변수, 속성, 매개변수
클래스 변수:  여러 인스턴스 간에 서로 공유해야 하는 값은 클래스 변수를 통해 바인딩.
인스턴스 변수: 인스턴스 별로 다른 값 가지게 됨

```python
class Account:
    num_accounts = 0 #클래스 변수. 함수 밖에 정의
    def __init__(self, name):
            self.name = name  #인스턴스 변수: 앞에 self 붙음
            Account.num_accounts += 1
    def __del__(self):
            Account.num_accounts -= 1

kim = Account("Kim")
lee = Account("Lee")

kim.name #'Kim'
lee.name #'Lee'

kim.num_accounts #2
lee.num_accounts #2
```

## 속성과 변수
클래스 안에서 속성에 접근할 때는 self.속성
클래스 밖에서 접근할 때는 인스턴스.속성

```python
class 클래스이름:
    def __init__(self, 매개변수1, 매개변수2):
        self.속성1 = 매개변수1
        self.속성2 = 매개변수2
```

## 상속과 인터페이스
상속은 물려받는 것
인터페이스는 장착하는 것

## 상속과 super
```python
class School:
    def __init__(self, name, address): #self는 School()
        self.name = name
        self.address = address
    
    def print_name(self): #self는 School.print_name()
        print("학교이름은 %s 이고, 주소는 %s이다." % (self.name, self.address))

class Classroom(School): #school을 상속
    def __init__(self, name, address): #init 사용
        self.book = []
    def num(self, num_of_students):
        self.num_of_students = num_of_students
#super로 상속하지 않고 init으로 상속한 경우 부모 클래스의 속성에 접근 못한다.

class Classroom2(School):

    def num(self, num_of_students):
        self.num_of_students = num_of_students #init 없이 사용
#부모 클래스의 속성에 접근 가능

class Classroom3(School):
    def __init__(self, name, address):
        super().__init__(name, address) #super 상속
        
    def num(self, num_of_students):
        self.num_of_students = num_of_students
#부모 클래스의 속성에 접근 가능

class Classroom4(School):
    def __init__(self):
    School.__init__(self, name, address) 
    self.num_of_students = num_of_students


cl1 = Classroom("초등학교","경기도") #얘만 오류남
cl2 = Classroom2("중학교","서울")
cl3 = Classroom3("고등학교","인천")

cl3.print_name() #학교이름은 고등학교 이고, 주소는 인천이다.
cl2.print_name() #학교이름은 중학교 이고, 주소는 서울이다.
cl1.print_name() #'Classroom' object has no attribute 'name'
```

---
클래스 내의 모든 속성과 매서드 보기
```python
print(dir(instance1))
print(dir(class()))
```

## 캡슐화와 접근제어
캡슐화를 하는 이유는 기능별 모듈화가 가능해지고 디버깅이 편해진다. 또한 프로그램이 기능별로 분리되어서 소스코드의 목적을 알기 쉬워진다. 

### 파이썬에서의 접근 제어
_변수이름 혹은 
__변수이름: 속성이나 매서드에 대한 접근을 막기 위해 double underscore 사용
이렇게 선언하게 되면 from module import *시 _로 시작하는 것들은 모두 임포트에서 무시된다. 하지만 이런 public 속성이어도 '_클래스이름__메소드이름' 형태로 이름을 변환시켜, 접근가능하게 할 수 있다. 
 

```python
class Point:
       def __init__(self, x, y):
        self.x = x
        self.y = y
        self.__private_name = "private 접근"

class Point_sub(Point):
  def __init__(self,x, y):
        Point.__init__(self,x,y)
        self.x = x
        self.y = y
    
  def __sub(self): #private
        self.__x = 10
        self.__y = 20
  def sub(self): #public
        self.__sub()
        print(self.__x)
        print(self.__y)
 
my_point = Point(1, 2)

my_point_sub = Point_sub(10,20)


 # case 1 - error case
#print(my_point.__private_name) #접근 불가: 'Point' object has no attribute '__private_name'

 # case 2
print(my_point._Point__private_name)    # 내부 사용(import에서 무시됨)을 위한 언더스코어(_)를 활용한다.
                                        #Point 클래스의 오브젝트인 "my_point._클래스__메소드이름"로 바꾸니 접근 가능

 # case 3
my_point_sub.sub()    # 변환된 이름에 대해 값 출력 
```

```python
class A(object):
  def __init__(self):   #궁금한 거..:__init__(self, x=None) 이런식으로 안 쓰고 왜 
    self.__X = 3        # self._A__X 로 변환
  def __test(self):     # _A__test() 로 변환
    print('self.__X' ,self.__X)
  def bast(self):
    self.__test()


class B(A):
  def __init__(self):
    A.__init__(self)
    self.__X = 20       # self._B__X 로 변환
  def __test(self):     # _B__test() 로 변환
    print(self.__X)
  def bast(self):
    self.__test()

a = A()
#a.__test()       #'A' object has no attribute '__test'
a._A__test()      # 변형된 이름을 통해 접근가능 # self.__X 3
a.bast()          #  self.__X 3 #왜 되지..?

print("-"*50)
b = B()
b._B__test()      # 변형된 이름을 통해 접근가능 #20
b.bast()          # 20
print(dir(b))     # 상속을 받았기 때문에 B클래스의 인스턴스에서 A클래스의 함수와 변수도 확인가능하다.
#['_A__X', '_A__test', '_B__X', '_B__test', 'bast',...]
b._A__X           # 3


```

Name | Notation | Behavior |
|:----:|:----:|:----:|
name | Public | 아무나 접근 가능 |
_name | Protected | 밖에서 직접적으로 접근 불가 |
__name | Private | 밖에서 접근 불가 |
<br>

```python
class Point:
   def __init__(self, x, y):
       self.x = x
       self.y = y
       self.__private_name = "private 접근"
```

public으로 할지 private으로 주로 회사 보안팀에서 정해주는데...private으로 하면 복잡해지기도 해서 보통 public으로 한다고 한다.

속성 충돌
```python
# 속성충돌
class parent_class:
  def __init__(self):
    self.value = 30
    self._value = 40
    self.__value = 50

  def get(self):
    return self.value

class sub_class(parent_class):
  def __init__(self):
    super().__init__()
    self.__value = 20    # 위의 parent_class의 _value와 충돌발생

s = sub_class()
print(s.value) # public # 30
print(s._value)# protected # 40
#print(s.__value) #'sub_class' object has no attribute '__value'
print('parent_class value:',s.get(),' sub_class value:', s.value) 
#parent_class value: 30  sub_class value: 30
```

### 변수 범위
로컬 변수
전역 변수
global 전역 변수

전역 변수는 잘 안 쓰게 됨..
함수 복잡하거나, 메모리 부담 많이 줄 때 전역변수는 부담스러우니까 로컬변수로 많이 쓰기

### getter/setter
클래스 인스턴스의 내부 데이터를 보호하기 위해서 데이터의 접근용 메서드를 작성하는 것. 일반적으로 `데이터를 읽어주는 메서드`를 getter(게터), `데이터를 변경해주는 메서드`를 setter(세터)라고 한다.

### property
(getter와 setter를 직접 구현해서 사용하는 경우도 많이 있지만,)
파이썬에서는 getter,setter에 대해서 명시적으로 표현하지 않기 때문에 property 키워드를 사용한다.
따라서 내장함수와 데코레이터를 활용하여 코드를 간결화하고 기존 인스턴스 속성에 새로운 기능을 제공할 수 있다.

파이썬의 내장함수 property() : getter/setter에 대해서 변수처럼 활용할 수 있게 해준다.
데코레이터로서의 @property : 기존의 메소드에 대해서 새로운 기능을 제공할 수 있다.
(단, 데코레이터를 적용한 메소드를 재사용할 수 없다.)
```python


```
## 정적 메소드
클래스에 대한 별도의 객체를 생성할 필요 없이 메소드를 바로 사용할 수 있다.
class method와 static method가 있는데, class method는 파라미터를 통해서 클래스의 변수에 접근이 가능하지만, static method는 별도의 정적변수(클래스 이름)를 선언해야 접근 가능하다.



### 상위 클래스 출력
```python
AssertionError.mro()
#[AssertionError, Exception, BaseException, object]
```

---


# 코딩스탠다드: OOP와 함수 

함수를 고치면 이 함수가 쓰이는 모든 부분에서 확인해봐야 함
-> 함수 신중히 만들기
#### 재사용을 할 것 같은 기능만 함수로 만들자(+클래스도)
예외: convention적으로 함수로 만드는 기능 등

#### 중복되는 코드가 계속 나오면 이것도 함수로 만들기


#### exception을 너무 남발하면 maintenance 측면에서 안 좋음
null: try ..., except null 이런 식보다는 조건을 name or null로 하고 assert로 디버깅?

#### null 처리
매개변수로 null이 들어올 수 있을 때는 try except을 남용하지 말고,
```python
def func(a or None):
    ...
```
이런식으로 하고,

반환값도 
```python
return dd or None
```
이런 식으로 하기


https://youtu.be/rbWSTXBYNFA

# OOP에서 클래스와 인스턴스 개념

object(객체) 지향: 객체들과의 대화
<->절차 지향: 순서를 바탕으로 프로그래밍

class: 청사진. 밑그림

instance: 실체

클래스에서 실체를 생성

프로그래밍에서 사용되는 대부분의 개념은 최소비용으로 최대효율을 얻기위해 개발되었다. OOP 또한 그것의 일부일 뿐이다.

---
과거에는 파이썬 파일 하나에 모든 함수의 기능을 다 담았지만, OOP로 넘어오면서 파이썬 파일을 여러 개로 나누게 됨

추상 클래스

인터페이스