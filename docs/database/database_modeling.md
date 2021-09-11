---
layout: default
title: 데이터 모델링
date: 2021-05-12
parent: Database
nav_order: 1
tags: [data, data model, entity, attribute, relationship]
category: [Database]
nav_exclude: true
search_exclude: true
comments: true
---



# Database Modeling



### 데이터베이스 모델링이란?

현실 세계에서의 작업이나 사물들을 DBMS의 테이블로 옮기기 위한 과정



### 모델링의 특징

1. 추상화(모형화, 가설적)
2. 단순화
3. 명확화



### 데이터 모델링시 유의점

1. 중복
2. 비유연성
3. 비일관성



### 데이터 모델링의 3단계

1. 개념적 데이터 모델링(Conceptual Data Modeling)
2. 논리적 데이터 모델링(Logical Data Modeling)
3. 물리적 데이터 모델링(Physical Data Modeling)



## 전체 데이터베이스 구성도

* 데이터: 하나하나의 단편적인 정보
* 테이블: 표 형태의 데이터
* 데이터베이스: 테이블이 저장되는 장소
* DBMS: 데이터베이스 관리 시스템 또는 소프트웨어
* 열: 테이블의 세로
* 열 이름: 각 열을 구분하기 위한 이름으로 열 이름은 테이블 내에서는 서로 달라야 함.
* 데이터 형식: 열에 저장될 데이터의 형식
* 행: 데이터가 저장되는 단위?
* 기본 키(PK): 기본 키 열은 각 행을 구분하는 유일할 열
* SQL: 사람과 DBMS가 소통하기 위한 언어

* 스키마: 데이터베이스 내부 자료의 구조 및 자료 간의 관계를 정의한 구조



MySQL의 [File]-[New Model]

[Model Overview] 패널에서 "Add Diagram"



## 기본 SQL

* CREATE : CREATE {개체종류} {개체이름}..

* UPDATE

  ```sql
  UPDATE `shop_db`.`product` SET `product_name` = '삼각김밥' WHERE (`product_name` = '삼각김밤');
  
  ```

  SELECT

* DELETE :





MySQL에서 조심해야 하는 백틱키!

```sql
INSERT INTO `user_db`.`user` (`user_id`, `user_name`, `user_addr`) VALUES ('1234','terri','서울 중구') #데이터베이스, 테이블이름, 열이름은 모두 백틱키로 감싸고, 값만 따옴표로 감싼다
```



## 데이터베이스 개체

* 인덱스 : 데이터 조회시 결과가 나오는 속도를 획기적으로 빠르게 해줌
* 뷰 : 테이블의 일부를 제한적으로 표현
* 스토어드 프로시저 : SQL에서 프로그래밍이 가능으하게 해줌
* 트리거 : 잘못된 데이터가 들어가는 것을 미연에 방지 
* 함수 
* 커서



**인덱스 달아주기**

```sql
CREATE INDEX idx_user_name ON user(user_name);
```

execution-plan에서 key-lookup은 인덱스로 찾은 것



**뷰**

가상의 테이블. 뷰는 실제 데이터를 가지고 있지 않으며, 진짜 테이블에 링크된 개념

뷰의 실체는 select문이다?

```sql
CREATE VIEW user_view
AS
		SELECT * FROM user;

```

뷰를 사용하는 이유

* 보안에 도움이 됨
* 긴 SQL문을 간략하게 만들 수 있음



**스토어드 프로시저**

MySQL에서 제공하는 프로그래밍 기능. 여러 개의 SQL문을 하나로 묶어서 편리하게 사용할 수 있음. 그 외 연산식, 조건문, 반복문 사용 가능

```sql
DELIMITER //
CREATE PROCEDURE myProc()
BEGIN
		SELECT * FROM member WHERE member_name = '나훈아';
        SELECT * FROM product WHERE product_name = '삼각김밥';
END //
DELIMITER ; #;를 띄어써야함
```

```sql
CALL myProc();
```

```sql
DROP PROCEDURE myProc();
```







---

## SQL 기본 문법

### Use 문

```sql
USE user_db; --이 데이터베이스 사용할 것
```



### Select 문

```sql
SELECT select_expr
[FROM table_references]
[WHERE where_condition]
[GROUP BY {col_name | expr | position}]
[HAVING where_condition]
[ORDER BY {col_name | expr | position}]
[LIMIT {[offset,] row_count | row_count OFFSET offset}]
```



별칭 지정(alias)

```mysql
SELECT addr 주소, debut_date "데뷔 일자", mem_name FROM member;
--addr 컬럼은 "주소", debut_date 컬럼은 "데뷔 일자"로 불러와짐
```



범위 지정 : AND, BETWEEN ~ AND

```mysql
SELECT user_name FROM user
WHERE height >= 163 AND height <= 165
```

```mysql
SELECT user_name from user 
WHERE height BETWEEN 163 AND 165;
```



IN() : 어디 하나에 속할 때..

```sql
SELECT user_name, addr FROM user
WHERE addr IN("경기", "서울", "충북");
```

```mysql
--이렇게 해도 됨
SELECT user_name, addr FROM user
WHERE addr = '경기' OR addr = '서울' OR addr = '충북';
```



LIKE : 문자열의 일부 글자 검색

```mysql
SELECT * 
FROM user
WHERE user_name LIKE '우%'; --우 뒤엔 아무거나 와도 됨
```

```mysql
SELECT * FROM user
WHERE user_name LIKE '__한';--언더바는 글자 하나 의미. 즉 글자 두 개 + '한'
```



서브 쿼리: select 안에 또 select 들어갈 때

```mysql
SELECT mem_name, height FROM member
WHERE height > (SELECT height FROM member WHERE mem_name = ‘에이핑크’);
```



ORDER BY, LIMIT, DISTINCT, GROUPBY ~ HAVING

```mysql
SELECT 열_이름
FROM 테이블_이름
WHERE 조건식
GROUP BY 열_이름
HAVING 조건식
ORDER BY 열_이름
LIMIT 숫자
```

* ORDER BY 절은 WHERE 절 다음에 나와야 함!

```mysql
SELECT user_id, user_name, user_date, height 
	FROM user
	WHERE height >= 164
	ORDER BY height DESC;
```

* 여러 조건으로 정렬하기

```mysql
SELECT user_id, user_name, user_date, height 
	FROM user
	WHERE height >= 164
	ORDER BY height DESC, user_date ASC;
```

* LIMIT : 출력 개수 제한

```mysql
SELECT user_id, user_name, user_date, height 
	FROM user
	WHERE height >= 164
	ORDER BY height DESC, user_date ASC
	LIMIT 3,2;   --세번째부터 2건 조회. LIMIT 3 OFFSET 2와 동일
	
```

* DISTINCT

```mysql
SELECT DISTINCT height FROM user;
```

* GROUP BY + 집계함수 SUM

```mysql
SELECT user_id "회원 아이디", SUM(amount)"총 구매 개수" FROM user
GROUP BY user_id
ORDER BY user_id;
```

* GROUP BY + 집계 함수 응용

```mysql
SELECT mem_id “회원 아이디”, SUM(price*amount) "총 구매 금액"
FROM buy GROUP BY mem_id;
```

* 그 외 GROUP BY와 함께 사용되는 집계 함수들

SUM(), AVG(), MIN(), MAX(), COUNT(), COUNT(DISTINCT)

* HAVING

```mysql
SELECT mem_id "회원 아이디" , SUM(price*amount) "총 구매 금액" from buy 
GROUP BY mem_id
HAVING SUM(price*amount) > 1000 --SUM(price*amount) 으로 쓰거나 "총 구매 금액"으로 써도 됨
ORDER BY "총 구매 금액" DESC;
```



## 데이터 변경을 위한 SQL 문

`INSERT`, `AUTO_INCREMENT`, `INSERT INTO ~ SELECT`, `UPDATE`, `DELETE`



### INSERT



INSERT문 형식

```mysql
INSERT INTO 테이블(열1, 열2, ...) VALUES (값1, 값2,...);
INSERT INTO 테이블 VALUES (값1, 값2,...);
INSERT INTO 테이블 VALUES (값1, 값2,...), (값1, 값2,...), (값1, 값2,...);
```

입력 안 한 열은 NULL값이 들어감



마지막에 insert한 id 보기

```mysql
SELECT LAST_INSERT_ID();
```



다른 테이블의 데이터를 가져와서 한 번에 입력 : INSERT INTO ~SELECT

```mysql
INSERT INTO 테이블 (열_이름1, 열_이름2, ...)
	SELECT 문 ;
```



테이블 스키마 살펴보기

```mysql
DESC database_name.table_name;
```



다른 테이블의 값을 가져오기. 가져오려는 열의 개수와 새로운 테이블의 스키마가 맞아야 함

```mysql
INSERT INTO city_popul 
	SELECT Name, Population FROM world.city;
```





AUTO_INCREMENT 자동으로 증가하는 값이 들어감. AUTO_INCREMENT로 설정한 열은 자료 입력시 NULL값 넣으면 됨

```mysql
CREATE TABLE toytable (
	toy_id INT AUTO_INCREMENT PRIMARY KEY,
	toy_name CHAR(4),
	age INT);
```

AUTO_INCREMENT 시작 값 바꾸기

```mysql
ALTER TABLE toytable AUTO_INCREMENT=100;
INSERT INTO toytable VALUES (NULL, ‘소희’, 15);
```

AUTO_INCREMENT 증가 값 바꾸기 : 시스템 변수 변경

```mysql
CREATE TABLE toytable (
	toy_id INT AUTO_INCREMENT PRIMARY KEY,
	toy_name CHAR(4),
	age INT);
	ALTER TABLE hongong3 AUTO_INCREMENT=1000; --1000부터 시작
	SET @@auto_increment_increment=3;	--3씩 증가
```

* SHOW GLOBAL VARIABLES: 시스템 변수 보기
* SELECT @@시스템변수 : 시스템 변수의 값 확인



### UPDATE

```mysql
UPDATE 테이블이름
SET 열1=값1, 열2=값2, ...
WHERE 조건 ;
```



```mysql
USE market_db;
UPDATE city_popul
SET city_name = ‘서울’
WHERE city_name = ‘Seoul’;
SELECT * FROM city_popul WHERE city_name = ‘서울’;
```



여러 열의 값 변경

```mysql
UPDATE city_popul
	SET city_name = '뉴욕', population = 0
	WHERE city_name = 'New York';
```

WHERE가 없으면 전체 열의 값이 변하니 주의



```mysql
UPDATE city_popul
	SET population = population/ 10000;
SELECT * FROM city_popul LIMIT 5;
```



### DELETE

행 단위로 삭제함



```mysql
DELETE FROM 테이블 WHERE 조건;
```

```mysql
DELETE FROM city_popul
	WHERE city_name LIKE 'New%';
```



### DROP, DELETE, TRUNCATE

* DELETE: 삭제가 오래 걸림(행 별로 지우는 듯). 빈 테이블이 남음
* DROP: 테이블 아예 삭제. 순식간에 삭제됨
* TRUNCATE: DELETE과 같은 효과 내지만 더 빠름



---



MYSQL을 사용하면서 겪을 수 있는 세세한 문제도 잘 설명되어 있습니다. UPDATE 및 delete를 허용하지 않는다..
