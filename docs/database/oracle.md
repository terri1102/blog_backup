---
layout: default
title: Oracle 문법 간단 정리
date: 2021-05-17
parent: Database
nav_order: 2
comments: true
---





ORACLE 실습 사이트(로그인 필요): https://livesql.oracle.com/ 

# DML 정리

## 1. 테이블 컬럼 추가 

ALTER TABLE 테이블명 ADD(컬럼명 데이터타입(사이즈)) 

```SQL
ALTER TABLE PRODUCT ADD(PRODUCT_ID NUMBER (precision, scale))
ALTER TABLE PRODUCT ADD(PRODUCT_ID NUMBER (9))
```



### 2. 테이블 컬럼 수정

ALTER TABLE 테이블명 MODIFY(컬럼명 데이터타입(사이즈))

```SQL
ALTER TABLE PRODUCT MODIFY(PRODUCT_ID NUMBER(9))
```



### 3. 테이블 컬럼 삭제

ALTER TABLE 테이블명 DROP COLUMN 컬럼명

```SQL
ALTER TABLE PRODUCT DROP COLUMN PRODUCT_ID
```



### 4. 테이블 컬럼 이름 변경

ALTER TABLE 테이블명 RENAME COLUMN 원래컬럼명 TO 바꿀컬럼명;

```SQL
ALTER TABLE PRODUCT RENAME COLUMN PROD_NM TO PROD_NAME
```

---



---

### IE 표기법

product

| PROD_ID: VARCHAR2(10) NOT NULL  |
| ------------------------------- |
| PROD_NM: VARCHAR2(100) NOT NULL |
| REG_DT: DATE NOT NULL           |
| REGR_NO: NUMBER(10) NULL        |



### Barker 표기법

```SQL
PRODUCT()

# PROD_ID VARCHAR2(10)
* PROD_NM: VARCHAR2(100) 
* REG_DT: DATE 
O REGR_NO: NUMBER(10)
```

---



ORACLE DBMS 기준 sql (primary key 설정하는 법 주의!)

```SQL
CREATE TABLE PRODUCT
( PROD_ID VARCHAR2(10) NOT NULL
 ,PROD_NM VARCHAR2(100) NOT NULL
 ,REG_DT DATE NOT NULL
 ,REGR_NO NUMBER(10) 
 ,CONSTRAINT PRODUCT_PK PRIMARY KEY (PROD_ID));
```

---
### 그 외
CHAR, VARCHAR, VARCHAR2

| CHAR   | VARCHAR | VARCHAR2 |
| ------ | ------- | -------- |
| 고정형 | 가변형  | 가변형   |
| 2000byte | 4000 byte | 4000 byte |

* 현재 VARCHAR와 VARCHAR2는 차이가 없으나 VARCHAR2를 쓰는 것 권장



References

* [ORACLE] 오라클 테이블 컬럼 추가/수정/삭제/이름변경 하는 방법(ALTER 테이블 ADD/MODIFY/DROP/RENAME) 출처: https://jwklife.tistory.com/5 [인 생]