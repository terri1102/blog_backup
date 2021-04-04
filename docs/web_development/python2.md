---
layout: default
title: 파이썬 자료형
date: 2021-03-19
categories:
  - TIL
tags:
  - TIL
comments: true
nav_order: 6
parent: Web Development
---



# Python Data types



파이썬 함수

함수와 메소드

메소드: 메소드도 함수 안에 속함. 어떠한 객체와 관련이 있다면 메소드-> 앞에 객체를 변수화한 것을 메소드 앞에 붙여줘야 함!

 메소드가 아닌 함수: 앞에 객체 불필요        

```python
#함수:
print()
#메소드
element.click()
```

![method1]



그냥 함수: 단순한 계산, 단순 변환   ex)함수()

복잡한 객체를 만드는 함수: 외부에서 만든 복잡한 객체 생성 -> 외부모듈에서 함수를 통해 객체 소환..ex)  외부모듈.함수()

메소드: 특정 객체에 대해서만 적용되는 기능 -> 실제로 동작이 일어남. ex) 객체.메소드()



## 클래스

클래스는 자기소개서다. 설계서다.

클래스 안에 변수(attribute)와 함수(method) 있음

파이썬에서 보통 함수는 소문자, 클래스는 대문자로 시작하는 경우가 많음

클래스도 객체, 인스턴스도 객체

모든 클래스가 오브젝트(객체)에서 상속을 받는다. 모든 것의 어머니는 오브젝트

모든 것은 객체로 부터 파생된 거-python의 object 

인스턴스 생성(instantiate)이 객체화

```python
class JSS:
    def __init__(self): #init 클래스를 선언하는 순간 실행되는 함수
        				#a = JSS() -> __init__ 함수 안의 내용 실행
        self.name = input('이름:')
        self.age = input("나이:")

```

```python
class JSS:
    def __init__(self):           #인스턴스 생성하는 순간 실행 #self: 클래스를 저장할 변수
    	self.name = input("이름 : ")
        self.age = input("나이: ")
    
    def show(self):               #인스턴스.매서드 해야 실행
        print("나의 이름은 {}, 나이는 {}세 입니다.".format{self.name, self.age})
        
a = JSS()
#a를 인스턴스로 선언하는 동시에 __init__ 함수 실행됨
#이름 :  입력받는 프롬프트 뜬다 
#나이 :

a.show()
#"나의 이름은 ㅇㅇ, 나이는 30세입니다.
#a= JSS() 실행시 입력된 이름과 나이가 뜬다.

#클래스를 선언할 때 쓴 self는 a이기 때문에 괄호 안이 비게됨 
-#class JSS:
	#def show(self): 여기서의 self가
    #a.show()에서 a임
```

상속을 이용해 클래스 업데이트

```python
class JSS2(JSS):   #새 클래스(상속받을 클래스)
    def __init__(self): #새로 덮어씀
   
	def __init__(self):
    	super().__init__() #->상속받은 클래스 다 들고옴
        self.gender = input("성별 : ")
    def show(self):               #인스턴스.매서드 해야 실행
        print("나의 이름은 {}, 성별은 {}자, 나이는 {}세 입니다.".format{self.name, self.gender, self.age})    
   
    def __init__(self):
    	pass ->JSS의 init 함수를 똑같이 들고옴
   


a = JSS2()
a.name = "oo"
a.age = 30
a.gender = "여"
```



인수와 파라미터:

파라미터: 함수를 정의할 때 들어감

```python
def new_func(파라미터):
```

인수: 함수를 실행할 때 넣어주는 값(넘기는 값)

```python
new_func('인수')
#필수인수는 입력되지 않으면 함수 실행되지 않음
new_func(param_2 = 'dd', param_1 = 'ee') #인수 넣을 때 파라미터 이름 직접 써서 순서 바꿀 수 있다.
```

기본인수 설정해 놓으면 값을 인수 입력 안 해도 함수 실행됨



클래스의 속성을 직접 바꿀 수도 있고, 인스턴스에 속성을 overwrite 할 수도 있다.

```python
class new_class:
    attribute1 = "속성1"

aclass = new_class()
print(aclass.attribute1)
#속성2

aclass.attribute1 = "새속성"

print(aclass.attribute1)
#새속성
print(new_class.attribute1)  #인스턴스의 속성 바꿔도 클래스의 속성은 안 바뀜
#속성1
```







return과 print 차이

return은 실제로 값이 저장되는 거고, print는 출력만 해줌

none이 나오는 경우:  return을 안 쓰거나, return 뒤에 아무것도 안 쓰거나, return None



에러

```python
IndentationError
TypeError: func() missing 1 required positional argument:
```





underscore

1개

2개



파이썬은 클래스에서 public, private한 개념이 흐지부지함..

->해결: property decorator, underscore 1개(클래스 내부에서만 사용할 거다)

double underscore(underscore 2개(여러 클래스에서 사용가능, 상속받는 클래스에서도 쓸 수 있다!)

_, __쓸 때 convention임



---

파이썬에서 모듈 구분?

src 디렉토리 안에 file1.py, file2.py가 있더라도 '__Init__.py' 파일이 없으면 src는 모듈로 인식이 안 됨 (init.py 안에는 아무 내용도 없어도 됨)

```python
__init__.py
```



근데 python -m pytest(pytest는 명령어임..모듈 아님, 시스템 내에서 불러올 수 있는 명령어)는 실행 됨

```python
pytest #경로를 잘 못 찾음?

python -m pytest #-m이 경로를 인지해서 접근할 수 있게 해줌 

#setup.cfg에서 명시해서 path 내에서 python 파일 찾을 수 있게 해줌
```





```python
python -m pip install package #현재 쓰고 있는 pip에서 install하겠다고 명시적으로 표시
pip install package #이렇게 해도 됨. pip이 다른 파이썬이랑 연결되었을 때 오류날 수 있음
```





인스턴스는 클래스내 속성, 인스턴스 내 속성 접근 가능

클래스는 인스턴스 내의 속성에 접근불가



---



## 데이터베이스

커서: 쿼리문에 의해서 반환되는 결과값들을 저장하는 메모리공간

  	Fetch: 커서에서 원하는 결과값을 추출하는 것



세션

```python
#retrieving data

import psycopg2

con = psycopg2.connect(database="postgres", user="postgres", password="Kaliakakya", host="127.0.0.1", port="5432")
print("Database opened successfully")

cur = con.cursor()
cur.execute("SELECT admission, name, age, course, department from STUDENT")
rows = cur.fetchall()

for row in rows:
    print("ADMISSION =", row[0])
    print("NAME =", row[1])
    print("AGE =", row[2])
    print("COURSE =", row[3])
    print("DEPARTMENT =", row[4], "\n")

print("Operation done successfully")
con.close()

'''
Database opened successfully
ADMISSION = 3420
NAME = John
AGE = 18
COURSE = Computer Science
DEPARTMENT = ICT
'''
```



```python
#Updating Tables
import psycopg2

con = psycopg2.connect(database="postgres", user="postgres", password="Kaliakakya", host="127.0.0.1", port="5432")
print("Database opened successfully")

cur = con.cursor()

cur.execute("UPDATE STUDENT set AGE = 20 where ADMISSION = 3420")
con.commit()
print("Total updated rows:", cur.rowcount)

cur.execute("SELECT admission, age, name, course, department from STUDENT")
rows = cur.fetchall()
for row in rows:
    print("ADMISSION =", row[0])
    print("NAME =", row[1])
    print("AGE =", row[2])
    print("COURSE =", row[2])
    print("DEPARTMENT =", row[3], "\n")

print("Operation done successfully")
con.close()
```



```python
#deleting row
import psycopg2

con = psycopg2.connect(database="postgres", user="postgres", password="Kaliakakya", host="127.0.0.1", port="5432")
print("Database opened successfully")

cur = con.cursor()

cur.execute("DELETE from STUDENT where ADMISSION=3420;")
con.commit()
print("Total deleted rows:", cur.rowcount)

cur.execute("SELECT admission, name, age, course, department from STUDENT")
rows = cur.fetchall()
for row in rows:
    print("ADMISSION =", row[0])
    print("NAME =", row[1])
    print("AGE =", row[2])
    print("COURSE =", row[3])
    print("DEPARTMENT =", row[4], "\n")

print("Deletion successful")
con.close()
```





---



References