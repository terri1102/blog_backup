---
layout: default
title: 데이터베이스와 SQL 기초
date: 2021-03-02
parent: Web Development
tags:
  - TIL
comments: true
nav_order: 6
parent: Web Development
---



# Database 기초

## Schema (스키마)

![entity_relation_diagram]()

**스키마**: 데이터베이스의 테이블 구조를 의미한다. 

스키마에서 **엔티티**는 고유한 정보의 단위. 엔티티는 데이터베이스에서 테이블

**필드**: 엔티티의 특성, 테이블에서는 열(column)

**레코드:** 테이블에 저장된 항목, 행렬에서의 행



**트랜잭션**: 데이터베이스 시스템에서 작업의 기본 단위



### 트랜젝션의 특성(ACID)

Atomicity(원자성)

Consistency(일관성)

Isolation(격리성)

Durability(영속성)



1대 다의 관계일 때 N인 테이블에 필드 넣어줘도 됨



N:N 관계 (다대 다 관계)

보통 테이블 하나 만들어서 연결 관계 저장(조인 테이블)



## Sql문 간단 정리

쿼리 작성시 대소문자 구분 없음

들여쓰기는 해도 안 해도 됨 

```sql
--SELECT

SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```



```sql
select customerid, firstname from customers
where firstname like 'H%'
order by 2  --(firstname)
```

select 안 된 필드에 따라서도 정렬 가능(테이블 안에 있는 필드면 가능)

```sql
select customerid, firstname 
	from customers as c   --as 생략가능--앞으론 c.firstname이라고 사용가능
where c.firstname like 'H%'  --like 쓰면 % 사용 가능(특수기호 가능)
order by 2  --(firstname)      --in 은 '정확한 값' 검색
```

여러 쿼리 날리면 마지막 쿼리만 보여주기도 함(어플리케이션 따라 다름)

```sql
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

join과 where는 짝이다

```sql
select e.employeeid, e.firstname from customers c
join emplyess e --e가 여기서 선언되었지만 윗줄에서 써도 됨
 where c.supportrepid = e.employeeid
```

쿼리 순서와 실행 순서는 다르다





sqlite에서는 right outer join이 없음

```sql
SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table 
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

```sql
SELECT column, another_column, …
FROM mytable
INNER/LEFT/RIGHT/FULL JOIN another_table 
    ON mytable.id = another_table.matching_id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```



컬럼에 not null은 조건을 설정해 준 것임

not null에 체크 안 되어 있어도 null값 없을 수 있음



---



## DBAPI 종류에 따라 데이터베이스와 연결하는 방법

dbms 종류별, dbapi나 orm 라이브러리별로 너무너무 헷갈려서 예시를 자세하게 정리했다.



#### 1. 서버없이 데이터베이스와 연결(DBAPI)

```python
import sqlite3
conn = sqlite3.connect('test.db')
cursor = conn.cursor()
cursor.execute("CREATE TABLE passenger(...)")
conn.commit()
conn.close()
```



#### 2. 서버를 사용하는 데이터베이스와 연결(DBAPI)

##### 1) Postgresql

```python
#postgresql

import psycopg2  #postgresql에 맞는 DBAPI

#elephantsql 사용해서 서버 생성했음

host = ' '
user = ' '
password = ' '
database = ' '

connection = psycopg2.connect(
    host=host,
    user=user,
    password=password,
    database=database
)
#connection 객체는 db 세션을 캡슐화
cursor = connection.cursor()

cursor.execute("""
CREATE TABLE passenger(
    ID INTEGER,
    Survived INTEGER,
    Pclass INTEGER,
    Name VARCHAR(128),
    Sex VARCHAR(12),
    Age FLOAT, 
    Siblings_Spouses_Aboard INTEGER,
    Parents_Children_Aboard INTEGER,
    Fare FLOAT)""")

import csv

with open('C:/~절대주소/titanic.csv', 'r') as f:
    reader = csv.reader(f) #reader 객체 상태: 이때 데이터는 보이지 않음
    row_list = []          #왠만하면 이렇게 리스트에 row를 넣는 식으로 하기!
    for row in reader:
        row_list.append(row)
    row_list = row_list[1:]
 
    id_list = []
    for i in range(878):
        id_list.append(i)
    
    for row, i  in zip(row_list, id_list):
       row.insert(0, i)
    ##########id 칼럼값까지 만들었음
        
    for row in row_list:
        cursor.execute(
            "INSERT INTO passenger VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s)", row           #여기 뒤에 row 붙는 거 주의!!!!
        )
connection.commit()
cursor.close()
connection.close()
```

##### 2) MongoDB

```python
from pymongo import MongoClient
import os 
import json
import pandas as pd

host = 'cluster0.randomsdf.mongodb.net'
user = 'userid'
password = 'userpassword'
database_name = 'dbname'
collection_name = 'colname'

MONGO_URI = f"mongodb+srv://{user}:{password}@{host}/{database_name}?retryWrites=true&w=majority"
connection = MongoClient(MONGO_URI)
#print(connection.list_database_names()) 

#collection: table
#document: rows
#fields: columns

titanic = connection["titanic"] #titanic이라는 db 만듦->이미 존재하면 안 만듦
passenger = titanic['passenger'] #passenger라는 collection 정의 -> 이 때 안 생김. 아래에서 생김 
#print(passenger)

print(titanic.list_collection_names()) #passenger(다큐먼트 들어가서 생기면 이름 나옴)


with open('C:/Users/절대주소/titanic.csv', 'r') as f:
    #CSV to JSON Conversion
    reader = csv.DictReader(f) #csv 파일을 딕셔너리로 읽어옴
    header = ['Survived','Pclass','Name', 'Sex','Age', 'Siblings/Spouses Aboard', 'Parents/Children Aboard','Fare'] #csv 파일 이랑 같아야함

    for each in reader:
        passenger.insert_one(each)
        
        #여기 까지 가니까 드디어 passenger 컬렉션 생겼음 (컬렉션은 정의할 때가 아니라 다큐먼트 들어갈 때 생김)
        #다큐먼트 다 들어가는데 꽤 시간 걸림 30초 이상인듯 
        #렉 걸린 줄 알았는데 Mongodb atlas에서 새로고침할 때마다 다큐먼트 개수 늘어남
```



#### 3. ORM으로 데이터베이스를 연결(ORM)

psycopg2 같은 DBAPI는 사용자가 입력한 sql쿼리문을 DB에 날리지만, sqlalchemy는 파이썬 코드로 sql쿼리문을 '생성'하여 DB에 날린다. 따라서 DBMS별로 조금씩 달라지는 Sql쿼리문을 다 알지 않아도 돼서 편리하다. 반면, 단점으로는 DB연결을 할 때 DBAPI 보다 한 단계 더 생기는 것이기 때문에 ORM은 오버헤드 문제가 있다.

session: 여러 개의 트랜잭션을 관리할 수 있게 도와줌

core에서의 엔진은 ORM에서 하나의 세션에 바인딩되어 사용이 된다.

```python
from csv import DictReader
from sqlalchemy import create_engine, Column, Integer, String, Float
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import Session


DATABASE_URI = "sqlite:///titanic_orm.db"
CSV_FILEPATH = 'titanic.txt'

engine = create_engine(DATABASE_URI)
Base = declarative_base(bind=engine)
session = Session(bind=engine)


#Survived,Pclass,Name,Sex,Age,Siblings/Spouses Aboard,Parents/Children Aboard,Fare

class Passenger(Base):
    __tablename__='Passenger'
    Id = Column(Integer, primary_key=True)
    Survived = Column(Integer)
    Pclass = Column(Integer)
    Name = Column(String)
    Sex = Column(String)
    Age = Column(Integer)
    Siblings_Spouses_Aboard = Column(Integer)
    Parents_Children_Aboard = Column(Integer)
    Fare = Column(Float)

with open(CSV_FILEPATH,'r') as f:
    reader = DictReader(f) #csv 파일도 dict으로 바꿔줌! 
    row_list = []
    for row in reader:
        row_list.append(row)
                                  #dict 형식으로 바뀌어서 키값 넣어줌
        query1 = Passenger(Survived=row['Survived'], Pclass=row['Pclass'], Name=row['Name'], Sex=row['Sex'], Age=row['Age'], Siblings_Spouses_Aboard=row['Siblings/Spouses Aboard'],\
            Parents_Children_Aboard=row['Parents/Children Aboard'], Fare=row['Fare'])
        session.add(query1)
    session.commit()
```



flask-sqlalchemy는 session대신 db.session으로 쓰는 등 약간 문법이 또 달라진다. 



## 정규화

데이터를 효율적 관리. 정확하게 관리하기 위해 사용

목적: 효율성. 무결성. 이상현상 방지



sql문 실행 순서

from - where - groupby - having - select - order by



```bash
sqlite3 chinook.db
```







# 7 Database Paradigms

1. Key-Value

redis, memcast

best for caching pub/sub, leaderboards

2. Wide column db

cassandra, hbase

no schema 

cql 

3. Document oriented db

mongodb, dynamo,

general purpose

4. relational db

mysql, postgres

primary key, foreign key

schema required

ACID compliant 

cockroach..

5. Graph db

node, edge만 있으면 됨

neo4j,

fraud deter

6. search db

full text search db

solr, meilisearch

document oriented 와 비슷, 책 뒤의 index와 비슷, 빠름

7. multi model db

fauna db

json..

user model, post model

---

이중에서 Key-Value Database, Document Database, Wide-Column Database, Graph Database는 Nosql

NoSQL:  generic term used for databases that do not depend on a relational model





psql

cli로 postgresql에 접속할 수 있게 해줌



DBMS 종류



elephant sql ->postgresql 서비스로 제공(용량/접근/비용)

elephantsql은 AWS를 이용해서 db서비스 제공

sqlite, 파일, 파일기반형 db, 따로 엔진이 필요없어서 가벼움, 기능 제한적

postgresql:  엔진이 필요, 인덱스, 효율화, 최적화 가능

그 외 mysql, oracle 등 기능 많은 db 있음

postgre는 데이터 서버가 필요함

도커대신 로컬에서 postgreSQL 설치해서 써도 됨

docker는 postgresql 서버를 띄우는 것, 엔진을 실행. 컨테이너 안에서 postgre서버가 돌고 있는 것

도커의 기능들은 도커 엔진으로부터 이루어진다??

도커 엔진: 컨테이너를 해석하는 애..컨테이너를 열 수 있는 애...

도커 컨테이너를 여러 개 띄우고 도커 엔진으로 컨테이너를 관리할 수 있음

dbeaver나, pgadmin과는 postgre와 연동 잘 안 됨

public에서 tables 보면 있음!



AWS-RDS에서 db 생성 가능

드랍박스도AWS-s3 서비스 사용함

esql은 AWS 데이터 센터를 사용한다. AWS에서는 데이터 백업도 해줌..

리전은 데이터센터 여러개 묶어놓은 것

직접접으로 AWS 연결하는 작업은 안 해봄.. 해보면 좋음





### Mongodb Atlas

회원가입 후에 프로젝트 생성하면 프로젝트 당 하나의 클러스터 생성가능

하나의 클러스터 안에  데이터베이스 여러 개 만들 수 있음

데이터베이스 안에 여러 컬렉션 만들 수 있음

특이한 것은...컬렉션은 컬렉션을 정의할 때가 아니라 컬렉션 안에 다큐먼트를 넣을 떄 생김 



Database Access 유저 추가해야 함

Network Access 어디서 접속할 수 있는지..ip 주소 정해주는 것

allow access from anywhere

cluster에 driver는 python로 3.6이상

데이터베이스 이름, 



---

```sql
create table Customer(
	customer_id	int	not null,
	customer_name varchar(32) not null,
	customer_age int,    
	primary key (customer_id)
)

create table Customer_Package(
    cp_id int not null,
    SELECT
    customer_id
    FROM Customer
    Inner Join (package_id from Pakage
              on Customer.customer_id = Pakage.package_id) AS Customer_Package

```



```sqlite
create table Customer(
	customer_id	int	not null PRIMARY KEY,
	customer_name varchar(32) not null,
	customer_age int
);

create table Pakage(
package_id	int	not null PRIMARY KEY,
package_name	varchar(32) not null,
package_date 	DATE);

create table Customer_Package(
cp_id INT NOT NUL PRIMARY KEY,
customer_id INT,
package_id INT,
FOREIGN KEY (customer_id) REFERENCES Customer(customer_id),
Foreign Key (package_id) REGERENCES Package(package_id));
```



Mongodb crud query

```sql
#조건과 입출력 모두 json 형태로 지정
--1. create
db.person.save({'name':'john'})
--2. read
db.person.find():
--3. update
db.users.update({name:'Johny'}, {name:'Cash', languages: ['english']});
--4. delete
db.users.remove({name: 'Sue'});
```





**데이터 베이스 설계 4단계**

요구사항 분석

걔념적 설계(추상적 표현)

논리적 설계(ER 다이어그램)

물리적 설계(스키마)

DBeaver에서 ERD는 스키마 의미해서 혼용함





더 알아볼 점

웹사이트 만들어서 db 연결을 해놓으면 로그인 기능으로 유저별 db 다르게 쓰게 하는 것 말고, 아예 db 권한을 나눌 수 있나?







Reference

https://phoenixnap.com/kb/nosql-database-types

https://phoenixnap.com/kb/what-is-nosql