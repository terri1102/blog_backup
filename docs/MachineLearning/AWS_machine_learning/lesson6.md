---
layout: default
title: Lesson 6
date: 2021-08-17
grand_parent: Machine Learning
parent: AWS Machine Learning course
nav_order: 6
use_math: true
---





# Object-oriented programming

레슨에서 배울 것은...

## 1. Procedural vs. object-oriented programming

* 절차적 프로그래밍 : 코드가 작성된 순서대로 위에서 아래로 순차적으로 실행되는 프로그램

* 객체지향 프로그래밍 : 객체를 만든 다음에 이를 이용해서 조립해서 사용함. 

  object - characteristics, actions 두 가지를 포함함

  Objects - Attributes(Characteristics), Methods(Actions)

  Object vs. Class : 클래스는 틀이고 객체는 이 틀로부터 찍어낸 것. 클래스의 인스턴스.

  캡슐화(encapsulation) : oop의 핵심 아이디어 중에 하나. 함수와 데이터들을 하나의 엔티티로 결합하는 것. oop에서 하나의 엔티티는 클래스라고 부른다. 캡슐화는 적용상 세부정보를 숨길 수 있게 해준다.

## 2. Classes, objects, methods and attributes

OOP syntax 배우기

```python
class Shirt: #클래스 이름은 대문자로 시작, Shirt뒤에 () 붙여도 되고 안 붙여도 됨
    def __init__(self, shirt_color, shirt_size, shirt_style, shirt_price):
        self.color = shirt_color
        self.size = shirt_size
        self.style = shirt_style
        self.price = shirt_price
        
     def change_price(self, new_price):
        self.price = new_price
      
     def discount(self, discount):
            return self.price * (1-discount)
```



### Self란?

self는attribute(속성)들을 저장해서 클래스 전체에서 사용 가능하게 해줌. 즉, attributes와 attribute values를 저장하는 딕셔너리라고 할 수 있음.

예를 들어 change_price 매서드는 price 속성에 접근하기 위해서 self가 필요함. 

self는 클래스의 속성에 접근하고 싶은 매서드에서 항상 첫 번째 인자로 들어감. 



```python
Shirt('red','S','short sleeve',15)
#<__main__.Shirt at 0x7f3ec05a7048>
```

메모리에서 어느 위치에 이 객체가 저장되었는지 표시



```python
new_shirt = Shirt('red','S','short sleeve',15)
```

Dot syntax를 통해서 인스턴스의 속성에 접근할 수 있음

```python
print(new_shirt.color)
#red
print(new_shirt.size)
#S
```

```python
new_shirt.change_price(10) #price가 10으로 바뀜
print(new_shirt.discount(.2)) #8.0
```



인스턴스는 파이썬의 다른 변수와 비슷함

```python
tshirt_collection = []
shirt_one = Shirt('orange','M','short sleeve', 25)
shirt_two = Shirt('red','S','short sleeve',15)
shirt_three = Shirt('purple','XL','short sleeve',10)

tshirt_collection.append(shirt_one)
tshirt_collection.append(shirt_two)
tshirt_collection.append(shirt_three)

for i in range(len(tshirt_collection)):
    print(tshirt_collection[i].color)
#orange
#red
#purple
```



## 3. Coding as class

OOP에서 **매서드를 통해서 속성을 변경하는 것** vs. 직접 속성을 변경하는 것.

매서드를 통해서 속성을 변경하면 한꺼번에 속성을 바꿔야 할 때 편리하다. 

ex) 달러에서 유로로 전체 가격 변경



## 4. Magic methods



## 5. Inheritance



