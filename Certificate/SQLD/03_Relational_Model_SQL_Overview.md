# 관계형 데이터베이스와 SQL 개요

## 1. 관계형 데이터베이스 기본 용어

1. 릴레이션
   * 데이터를 표 형태로 표현한 것
   * 일반적으로 테이블이라고 부른다.

2. 속성
   * 릴레이션의 열
   * 컬럼 또는 필드라고도 한다.

3. 튜플
   * 릴레이션의 행
   * 로우 또는 레코드라고도 한다.

4. 도메인
   * 하나의 속성이 가질 수 있는 원자값의 집합

5. 차수
   * 릴레이션에 있는 속성의 개수

6. 카디널리티
   * 릴레이션에 있는 튜플의 개수

## 2. SQL의 종류

### 1) DDL

DDL은 Data Definition Language의 약자이며 데이터 정의어라고 한다.

* CREATE
* ALTER
* DROP
* TRUNCATE

데이터베이스 객체의 구조를 생성, 변경, 삭제할 때 사용한다.

`ex) 테이블 생성, 컬럼 추가, 테이블 삭제`

### 2) DML

DML은 Data Manipulation Language의 약자이며 데이터 조작어라고 한다.

* SELECT
* INSERT
* UPDATE
* DELETE

테이블에 저장된 데이터를 조회, 삽입, 수정, 삭제할 때 사용한다.

`ex) 회원 목록 조회, 상품 데이터 추가, 주문 상태 변경`

### 3) DCL

DCL은 Data Control Language의 약자이며 데이터 제어어라고 한다.

* GRANT
* REVOKE

사용자 권한을 부여하거나 회수할 때 사용한다.

`ex) 특정 사용자에게 테이블 조회 권한 부여`

### 4) TCL

TCL은 Transaction Control Language의 약자이며 트랜잭션 제어어라고 한다.

* COMMIT
* ROLLBACK

트랜잭션의 결과를 확정하거나 취소할 때 사용한다.

## 3. 관계 대수와 SQL

### 1) 셀렉션

* 행을 선택하는 연산
* SQL의 `WHERE` 절과 관련 있다.

```sql
SELECT *
FROM EMP
WHERE DEPTNO = 10;
```

### 2) 프로젝션

* 열을 선택하는 연산
* SQL의 `SELECT` 절과 관련 있다.

```sql
SELECT ENAME, SAL
FROM EMP;
```

### 3) 조인

* 두 개 이상의 릴레이션을 관련 조건에 따라 연결하는 연산

```sql
SELECT E.ENAME, D.DNAME
FROM EMP E
JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

### 4) 디비전

* 특정 조건을 모두 만족하는 데이터를 찾는 연산
* SQLD에서는 개념 위주로 이해하면 된다.

## 4. 일반 집합 연산자

* 합집합
* 교집합
* 차집합
* 카티션 프로덕트

## 5. SELECT 문 작성 순서와 실행 순서

### 1) 작성 순서

```sql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
```

### 2) 실행 순서

```sql
FROM
WHERE
GROUP BY
HAVING
SELECT
ORDER BY
```

`FWGHSO`로 외우면 된다.

## 6. 별칭 사용 시 주의점

* 별칭은 `AS`를 사용하지만 생략할 수 있다.
* `WHERE` 절에서는 `SELECT` 절의 별칭을 사용할 수 없다.
* 이유는 `WHERE` 절이 `SELECT` 절보다 먼저 실행되기 때문이다.

```sql
SELECT SAL * 12 AS YEAR_SAL
FROM EMP
WHERE YEAR_SAL > 30000; -- 잘못된 사용
```

## 7. 핵심 정리

* 릴레이션은 테이블, 속성은 컬럼, 튜플은 행이다.
* DDL은 구조 정의, DML은 데이터 조작, DCL은 권한 제어, TCL은 트랜잭션 제어이다.
* SQL 실행 순서는 `FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY`이다.
