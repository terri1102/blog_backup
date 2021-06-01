---
layout: default
title: SQL server 문법
date: 2021-05-17
parent: Database
nav_order: 3
comments: true
---

ORACLE vs. SQL server

SQL server는 괄호를 사용하지 않는다. (처음에 테이블 정의할 때는 씀)

SQL server에서는 여러개의 컬럼을 동시에 수정하는 구문을 지원하지 않는다.

oracle에서는 DDl 문장 수행 후 자동으로 COMMIT 수행. 또한 DDL 문장의 수행은 트랜잭션을 종료시킴.

SQL server는 DDL 문장이어도 사용자가  COMMIT해야 커밋됨

충격적인 것...데이터 INSERT 할 때 작은 따옴표로 해야함. 큰 따옴표는 에러남

#### 공백문자

```sql
INSERT INTO 서비스 VALUES('999','','2015-11-11')
```

 ORACLE: 공백문자는 NULL로 입력됨. 조회시 WHERE 컬럼 IS NULL로 조회

SQL server: 공백문자는 공백문자('')로 입력됨. 조회시 WHERE 컬럼 = ''로 조회

## 기본 지식

정보처리기사 시험 후 머릿속에서 사라진 SQL 기초 지식 복기

* primary key는 한 테이블 당 하나이며, null 값을 가질 수 없다.

### 낮은 단계의 격리성에서 발생하는 문제들

1. Dirty Read: 다른 트랜젝션에 의해 수정됐지만 아직 커밋되지 않은 데이터를 읽는 것. 변경 후 아직 커밋되지 않은 값을 읽었는데 변경을 가한 트랜잭션이 최종적으로 롤백된다면 그 값을 읽은 트랜잭션은 비일관된 상태에 놓이게 된다.

2. Non-Repeatable Read: 한 트랜잭션 내에서 같은 쿼리를 두 번 수행했는데, 그 사이에 다른 트랜잭션이 값을 수정 또는 삭제하는 바람에 두 쿼리 결과가 다르게 나타나는 현상.
3. Phantom Read: 한 트랜잭션 내에서 같은 쿼리를 두번 수행했는데, 첫 번째 쿼리에서 없던 유령 레코드가 두번째 쿼리에서 나타나는 현상.



## CASE 표현

IF-THEN-ELSE 처럼 SQL의 비교 연산 기능을 보완하는 역할

1) SIMPLE_CASE_EXPRESSION 

```SQL
CASE
SIMPLE_CASE_EXPRESSION 조건
ELSE 표현절
END
```

```SQL
--LOC 칼럼을 구분하여 AREA 칼럼 만들기
SELECT LOC,
CASE LOC
WHEN 'NEW YORK' THEN 'EAST'
WHEN 'BOSTON' THEN 'EAST'
WHEN 'CHICAGO' THEN 'CENTER'
WHEN 'DALLAS' THEN 'CENTER'
ELSE 'ETC'
END as AREA
FROM DEPT;
```

| LOC      | AREA |
| -------- | ---- |
| NEW YORK | EAST |
| DALLAS | CENTER |
|CHICAGO| CENTER|
|BOSTON| EAST|



 2) SEARCHED_CASE_EXPRESSION: SIMPLE CASE 보다 더 다양한 조건 적용 가능

```SQL
CASE
SEARCHED_CASE_EXPRESSION 조건
ELSE 표현절
END

```

```SQL
--사원정보에서 급여가 3000이상이면 상등급, 100이상이면 중등급, 1000미만이면 하등급으로 분류
SELECT NAME,
CASE WHEN SAL >= 3000 THEN 'HIGH'
	WHEN SAL >= 1000 THEN 'MID'
	ELSE 'LOW'
END AS SALARY_GRADE
FROM EMP;
```

| ENAME | SALARY_GRADE |
| ----- | ------------ |
| SMITH | LOW          |
|ALLEN|MID|



```SQL
--중첩해서 사용 가능
SELECT ENAME, SAL
CASE WHEN SAL >= 2000
THEN 1000
ELSE (CASE WHEN SAL >= 1000
     		THEN 500
     		ELSE 0
     		END)
END as BONUS
FROM EMP;
```



3) DECODE(ORACLE)

```SQL
DECODE (표현식, 기준값1, 값1)
```



## NULL 값

* NVL/ISNULL(표현식1, 표현식2):  표현식1의 결과값이 NULL이면 표현식2의 값 출력

```SQL
SELECT ISNULL(COL2, 'X') FROM TAB1 WHERE COL1 = 'A';
--조건에 맞는 값이 NULL일 때 'X'를 반환
```

* NULLIF

```SQL
NULLIF(표현식1, 표현식2) 
--표현식1이 표현식2와 같으면 NULL을, 다르면 표현식1을 반환
```

* COALESCE(표현식1, 표현식2,...)

```SQL
COALESCE(표현식1, 표현식2,...)
--임의의 개수 표현식에서 NULL이 아닌 최초의 표현식을 나타낸다. 모든 표현식이 NULL이면 NULL을 반환
```

TAB1
| C1   | C2   | C3   |
| ---- | ---- | ---- |
| 1    | 2    | 3    |
|      | 2    | 3    |
|      |      | 3    |

```SQL
SELECT SUM(COALESCE 
          (C1,C2,C3))
          FROM TAB1
 --결과: 6 (각 컬럼에서 NULL이 아닌 첫번째 값(1,2,3)을 더한 값)
```



Null 값은 집계함수에서 빠진다. 예를 들어 30, 10, null의 avg는 20이다. 하지만 Null을 포함한 연산은 모두 Null이다. 예를 들어 Null + 2 는 null이다.

Null 값들은 select count(*)에 잡히진 않지만, GROUP BY시 하나의 그룹으로 인식됨.

## 참조

* Delete (/Modify) Action: Cascade, Set Null, Set Default, Restrict (<u>부서</u> - 사원)

1) <u>Cascade</u>: Master 삭제시 Child 같이 삭제

2) Set Null :  Master 삭제시 Child 해당 필드 Null

3) Set Default : Master 삭제시 Child 해당 필드 Default 값으로 설정

4) <u>Restrict:</u> Child 테이블에 PK값이 없는 경우만 Master 삭제 허용

5) No Action : 참조무결성을 위반하는 삭제/수정 액션을 취하지 않음



* Insert Action: Automatic, Set Null, Set Default, Dependent (부서 - <u>사원</u>)

1) <u>Automatic</u>:  Master 테이블에 PK가 없는 경우 Master PK를 생성 후 Child 입력

2) Set Null : Master 테이블에 PK가 없은 경우 Child 외부키를 Null 값으로 처리

3) Set Default: Master 테이블에 PK가 없는 경우 Child 외부키를 지정된 기본값으로 입력

4) <u>Dependent</u>: Master 테이블에 PK가 존재할 때만 Child 입력 허용

5) No Action: 참조무결성을 위반하는 입력 액션을 취하지 않음



---

# 테이블

## 1. 테이블 정의하기

sql server

```sql
CREATE TABLE IF NOT EXISTS PLAYER  (
  Player_id CHAR(7) NOT NULL, --고정자릿수
  Player_name VARCHAR(20) NOT NULL, --변동자릿수
  CONTRAINT PLAYER_PK PRIMARY KEY (Player_id),
  CONTRAINT Player_FK FOREIGN KEY (Team_id) REFERENCES TEAM(Team_id)
);
```



## 2. 테이블 이름변경(ANSI 표준)

오라클과 동일

```sql
RENAME 이전테이블이름 TO 새로운테이블이름;
```







# 컬럼

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

DROP TABLE 로 전체 테이블을 날려버린 일을 기억하며...삭제는 늘 신중히 하자.





### 4. 테이블 컬럼 이름 변경

ALTER TABLE 테이블명 RENAME COLUMN 원래컬럼명 TO 바꿀컬럼명;

```SQL
ALTER TABLE PRODUCT RENAME COLUMN PROD_NM TO PROD_NAME
```

---



### 5. 테이블 칼럼 정의 변경

* SQL server

ALTER TABLE 테이블명 ALTER COLUMN 칼럼명 데이터유형 [NOT NULL]; 

ALTER TABLE 테이블명 ALTER COLUMN 칼럼명 데이터 유형;

* Oracle

ALTER TABLE 테이블명 MODIFY (칼럼명 데이터유형, 칼럼명 데이터유형);

---

# 레코드

### 1. 테이블에 데이터 입력

```SQL
1) INSERT INTO 테이블명 (컬럼1, 컬럼2) VALUES(값1, 값2)
```

명시하지 않은 컬럼엔 Null 값이 들어가기 때문에 NOT NULL인 컬럼은 무조건 명시해야 함

```SQL
2) INSERT INTO 테이블명 VALUES(값1, 값2, 값3)
```

모든 컬럼에 값을 넣어야함. 넣을 값이 없으면 NULL로라도 넣어야 함



### 2. DROP, TRUNNCATE, DELETE



| DROP                                     | TRUNCATE                                                     | DELETE                               |
| ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------ |
| DDL                                      | DDL(일부 DML)                                                | DML                                  |
| ROLLBACK 불가                            | ROLLBACK 불가                                                | COMMIT 이전에 ROLLBACK 가능          |
| AUTO COMMIT                              | AUTO COMMIT                                                  | 사용자 COMMIT                        |
| 테이블이 사용했던 Storage를 모두 Release | 테이블이 사용했던 Storage 중 최초 테이블 생성시 할당된 Storage만 남기고 Release | 사용했던 Storage는 Release 되지 않음 |
| 테이블 정의 자체를 삭제                  | 테이블을 최초 생성된 초기상태로 만듦                         | 데이터만 삭제                        |



### 2-1. 테이블의 스키마는 남기면서 데이터 삭제 (+삭제 데이터에 대한 로그 남음)

```SQL
DELETE FROM 테이블이름;
--DELETE * FROM 테이블;-> 틀린 문법
```



### 2-2. 테이블의 스키마는 남기면서 데이터를 삭제하면서 디스크 사용량 초기화

```SQL
TRUNCATE TABLE 테이블이름
```



---

## COMMIT, ROLLBACK

[테이블 : 상품]

| 상품ID | 상품명 |
| ------ | ------ |
| 001    | TV     |

```sql
BEGIN TRANSACTION;
SAVE TRANSACTION SP1;
UPDATE 상품 SET 상품명 = 'LCD-TV' WHERE 상품ID = '001';
SAVE TRANSACTION SP1;
UPDATE 상품 SET 상품명='평면-TV' WHERE 상품ID = '001';
ROLLBACK TRANSACTION SP2;
COMMIT;
```

sp2의 transaction이 취소되어 상품ID '001'의 상품명은 'LCD-TV'

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

VARCHAR은 숫자도 문자도 들어갈 수 있음



* SELECT 문의 실행 순서: FROM - WHERE - GROUP BY- HAVING - SELECT- ORDER BY



* ORDER BY일 때 같은 순위인 것 까지 SELECT : WITH TIES

```SQL
SELECT TOP(3) WITH TIES 팀명, 승리건수
FROM 팀별성적
ORDER BY 승리건수 DESC;
```



* LIKE 연산자

LIKE 연산자를 쓰면 여러 조건에 해당되면 레코드가 중복되어 SELECT된다.

[EMP_TBL]

| EMPNO | ENAME |
| ----- | ----- |
| 1000  | SMITH |
|1050|ALLEN|
|1100|SCOTT|

[RULE_TBL]
|RULE_NO|RULE|
|-----|-----|
|1|S%|
|2|%T%|

```SQL
SELECT COUNT(*) CNT
FROM EMP_TBL A, RULE_TBL B
WHERE A.ENAME LIKE B.RULE
```

[결과]

|EMPNO|ENAME|RULE|
|---|---|---|
|1000|SMITH|S%|
|1100|SCOTT|S%|
|1000|SMITH|%T%|
|1100|SCOTT|%T%|




---

# 2장 SQL 활용

## 1. 표준조인



## 2. 집합 연산자



## 3. 계층형 질의와 셀프 조인



## 4. 서브쿼리









---



References

* [ORACLE] 오라클 테이블 컬럼 추가/수정/삭제/이름변경 하는 방법(ALTER 테이블 ADD/MODIFY/DROP/RENAME) 출처: https://jwklife.tistory.com/5 [인 생]

