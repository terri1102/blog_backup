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

