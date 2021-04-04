---
layout: default
title: Docker 정리
date: 2021-03-02
categories:
  - TIL
tags:
  - TIL
comments: true
nav_order: 6
parent: 
---



HTML, CSS for Web Scrapping

HTML, CSS, Java Script 

HTML 

Cascading Style sheet

java script 



한 폴더 안에 만들기



# HTML

웹과 관련된 언어. 웹의 토대

**HTML의 본질?**

**정보**-정보 생성, 수정, 보관하려고!

HTML은 전자 출판을 위해 만들어진 언어니까

태그를 통해 HTML은 이제 정보+디자인 기능 갖게 됨

예시) font 태그

근데 태그가 너무 많이 달려서 디자인 기능을 이제 CSS로 분리함



**HTML**

```html
<li>HTML</li>
<li>CSS</li>
<li>JavaScript</li>
```



**css**

<!--모든 li태그의 색 바꿈-->

<style>
    li{
    color:red;
    }
</style>



웹 브라우저에서 ctrl + o 누르면 파일 열기 실행됨





# CSS



# DOM

document object model

자바스크립트는 DOM을 통해서 HTML을 제어한다.

자바 스크립트는 웹브라우저에서만 사용되는 것이 아님

node.js 서버측 언어. javascript 언어를 통해 제어하는 대상이 서버쪽 자원 핸들링

자바스크립트를 통해 돔을 제어해서 웹브라우저를 제어한다.

dom document: 문서를 제어하기 위한 API. 문서 전체

dom events

dom elements: 문서에 참여하고 있는 요소



Web Crawling

웹 상의 내용을 수집하는 작업



w3schools.com 웹의 바이블. 참고하기 좋은 사이트



html 헤더태그는 <h1>에서 <h6>까지 있음

'<ul>': unordered list
    :불렛 포인트로 나옴

'<ol>' ordered list

'<div>' 태그: 상자



여러 개에 속성을 주고 싶으면 id 대신 class를 쓰지만 ㅑㅇ



html은 마지막으로 지정된 값을 쓴다.

클래스 여러개일 때는 띄어쓰기로 구분한다.



class는 css안 에서 존재한다.



DOM:

scrapping할 때 HTML만 가지고는 부족..html은 그냥 언어니까

DOM: Document(html)  object model

html을 하나의 객체처럼 다룰 수 있게 해줌

dom을 통해 html과 프로그램 언어를 연결해줌

html을 글자다.

css는 이를 꾸미는 것

 DOM은 언어가 아닌 개념. 프로그램 언어에서 html과 상호작용할 수 있게 해줌

document.querySelectorAll('.Spongebob')

dom은 document을 통해 접근 가능함

우리는 html parsing을 통해 문자열 접근할 것 



class나 id로 태그를 선택이 가능한것은 정적인 방법이고, DOM으로 객체화 하여 dictionary처럼 다룰수 있게 된다는것은 좀더 유연하게(상호작용) 다룰수 있게 된다는 뜻일까요 ?

---

스크래이핑

1. 통신

requests를 통해 웹에서 정보를 가져옴



2. 작업

1) 파싱  html을 객체로 변환

2) css/html을 통해서 원하는 정보만 추출함

3)데이터 변환

4)데이터 활용



스크래이핑과 달리 크롤링은 자동화 단계가 더 들어감

웹 자체를 다 파고 들어서,  페이지 내 링크마다 해당 조건에 해당하는 정보를 모음



requests나 beautifulsoup4보다

scrappy나 selenium이 조금 더 고급툴임

tr : table row

td: table data

테이블 데이터 자체를 가져오기

```python
'<table cellspacing="0 class="list_netizen"> == $0

class 아니라 class_로 써야 함!

table1 =soup.find(class_="list_netizen")

for i in table1.children:

​	print(i)

children = [x for x in table.children]
```



html parser는 

1)html 태그

2)css selector를 찾아서 찾아봄 id, class



스크래이핑의 저작권 문제?

회색 영역..

robots.txt 같은 파일에서 크롤링을 안 했으면 좋겠다는 파일을 두는 사람도 있음

알아서 판단해야 함



서버에 내용은 다 저장되고,

프론트에서 차단당한 리뷰를 필터링을 하는 것

selenium 같은 걸 쓰면 필터링 가능할 수도



페이지 넘기면서 긁을 수 있나?

url base로 만들고 페이지 넘버만 달라지게 해도 됨

for i in range(5):

​	url = f'http://movie.naver.com/movie/point/af/list.nh

​    resp = request.get(url)

​    soup = 

​    table1

​    table_rows



table_data.



== $0은 무슨 의미??

크롬 개발자 도구에서 붙이는 것, 현재 선택된 행을 콘솔에 $0으로 바로 선택할 수 있는 기능





----

orm

어플리케이션 만들 때 신경 쓸 게 너무 많아서 처음 애플리케이션 만들 때는 orm으로 만들게 하려고 배운다



app

1)데이터 -> 2)트위터 계정에서 데이터 가져와서 작업

3)RDBMS에 저장(이 때 ORM 사용할 것)

4)주소 설정

5) 환경



orm은 sql과 파이썬 중간다리 역할

class를 통해서 테이블을 만들고, 

base를 통해서 테이블을 명시하는 것이 중요하다!



세션: 세션 안에는 객체들이 있다. 



클래스 안 인스턴스(레코드) 를 세션에 추가했음. add 매서드를 통해서. 



그 후 세션이 db에 커밋하면 db에 반영됨

(autocommit은 추천하진 않음)



인스턴스를 delete하고 싶으면 먼저 db에서 가져와야 함



이미 db에 있는 테이블을 sqlalchemy로 조작하고 싶을 때

```python
class MyClass(Base):
    __table__ = Table('mytable', Base.metadata,
                    autoload=True, autoload_with=some_engine)
```



count

```python
session.query(User).filter(User.name.like('%ed')).count()
```

https://docs.sqlalchemy.org/en/13/orm/tutorial.html



```python
"""
Session.execute() accepts any executable clause construct, such as select(), insert(), update(), delete(), and text(). Plain SQL strings can be passed as well, which in the case of Session.execute() only will be interpreted the same as if it were passed via a text() construct. That is, the following usage:
"""
result = session.execute(
            "SELECT * FROM user WHERE id=:param",
            {"param":5}
        )
```



여태까지 배운 데이터베이스 연결 정리

1. 서버없이 데이터베이스와 연결(DBAPI)

ex) sqlite

```python
import sqlite3

conn = sqlite3.connect('ddd.db')
cursor = conn.cursor()
cursor.execute("sql문")
```



2. 서버 사용하는 데이터베이스 연결(DBAPI)

ex) psycopg2: postgresql에 맞는 라이브러리

connection 객체는 db세션을 캡슐화한다.

얘도 cursor로 직접 sql문 날려서 db 조작



3.파이썬 코드로 데이터베이스 연결(ORM)

ex)sqlalchemy: 파이썬 코드로 sql쿼리문을 '생성' 후 db에 날림->db별로 조금씩 달라지는 sql쿼리문을 다 알지 않아도 돼서 편리

세션: 여러 개의 transaction을 관리할 수 있게 도와줌