# 트랜잭션, NULL, 뷰

## 1. 트랜잭션

트랜잭션은 데이터베이스의 상태를 변화시키는 하나의 논리적 작업 단위이다.

하나의 트랜잭션은 전체가 수행되거나 전체가 취소되어야 한다.

`ex) 계좌 이체는 출금과 입금이 모두 성공해야 하나의 작업으로 인정됨`

## 2. 트랜잭션의 특성

트랜잭션의 특성은 ACID로 정리할 수 있다.

### 1) 원자성

트랜잭션의 연산은 모두 수행되거나 모두 수행되지 않아야 한다.

### 2) 일관성

트랜잭션 수행 전과 수행 후에도 데이터베이스는 일관된 상태를 유지해야 한다.

### 3) 고립성

하나의 트랜잭션이 실행 중일 때 다른 트랜잭션이 중간 결과에 접근할 수 없어야 한다.

### 4) 영속성

성공적으로 완료된 트랜잭션의 결과는 시스템 장애가 발생해도 영구적으로 유지되어야 한다.

## 3. 트랜잭션 상태

### Partially Committed

트랜잭션이 수행한 최종 결과를 데이터베이스에 아직 반영하지 않은 상태이다.

즉, Commit 연산 실행 전 상태이다.

## 4. 병행 제어

병행 제어는 여러 트랜잭션이 동시에 실행될 때 데이터의 일관성을 유지하기 위한 기법이다.

대표 기법은 다음과 같다.

* 로킹 기법
* 타임 스탬프 기법
* 최적 병행 수행 기법
* 다중 버전 기법

## 5. 로킹

로킹은 트랜잭션이 접근하는 데이터를 잠가서 다른 트랜잭션이 동시에 접근하지 못하도록 보호하는 기법이다.

### 로킹 단위가 큰 경우

* 관리가 쉽다.
* 잠금 범위가 넓어져 병행성이 낮아진다.

### 로킹 단위가 작은 경우

* 병행성이 높아진다.
* 관리가 복잡해진다.

## 6. NULL의 특성

NULL은 값이 없거나 알 수 없는 상태를 의미한다.

### 1) 산술 연산

NULL이 포함된 산술 연산 결과는 NULL이다.

```sql
SELECT 100 + NULL
FROM DUAL;
```

결과는 NULL이다.

### 2) 비교 연산

NULL과 비교한 결과는 TRUE 또는 FALSE가 아니라 Unknown이다.

```sql
WHERE COMM = NULL -- 잘못된 비교
```

NULL 비교는 `IS NULL`, `IS NOT NULL`을 사용해야 한다.

```sql
WHERE COMM IS NULL
```

### 3) 논리 연산

NULL은 3가지 값 논리를 따른다.

* TRUE AND Unknown = Unknown
* FALSE AND Unknown = FALSE
* TRUE OR Unknown = TRUE
* FALSE OR Unknown = Unknown
* NOT Unknown = Unknown

## 7. NULL 관련 함수

### 1) NVL

NULL인 경우 지정한 값으로 대체한다.

```sql
SELECT NVL(COMM, 0)
FROM EMP;
```

### 2) NULLIF

두 값이 같으면 NULL을 반환하고, 다르면 첫 번째 값을 반환한다.

```sql
SELECT NULLIF(10, 10)
FROM DUAL;
```

### 3) COALESCE

인수 중 첫 번째로 NULL이 아닌 값을 반환한다.

```sql
SELECT COALESCE(COMM, BONUS, 0)
FROM EMP;
```

## 8. 집계 함수와 NULL

* SUM, AVG, MAX, MIN은 NULL을 제외하고 연산한다.
* COUNT(컬럼)은 NULL을 제외하고 카운트한다.
* COUNT(*)는 NULL 포함 전체 행 수를 카운트한다.
* AVG는 NULL을 제외한 평균이므로 전체 행 기준 평균과 다를 수 있다.

## 9. 뷰

뷰는 하나 이상의 테이블에서 필요한 데이터만 논리적으로 보여주는 가상 테이블이다.

실제 데이터를 저장하는 것이 아니라 SQL문을 저장한다.

## 10. 뷰의 특징

* 논리적 데이터 독립성을 제공한다.
* 실제 데이터를 물리적으로 저장하지 않는다.
* 인덱스를 가질 수 없다.
* 복잡한 쿼리를 단순화할 수 있다.
* 필요한 컬럼만 보여주어 보안을 강화할 수 있다.
* 삽입, 삭제, 갱신에 제약이 있을 수 있다.
* 뷰 자체를 변경하기보다 삭제 후 재생성하는 방식으로 관리한다.

## 11. 뷰 예시

```sql
CREATE VIEW EMP_DEPT_V AS
SELECT E.EMPNO, E.ENAME, D.DNAME, E.SAL
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO;
```

```sql
SELECT *
FROM EMP_DEPT_V
WHERE SAL > 2000;
```

## 12. 핵심 정리

* 트랜잭션은 하나의 논리적 작업 단위이다.
* 트랜잭션의 특성은 원자성, 일관성, 고립성, 영속성이다.
* NULL이 포함된 산술 연산 결과는 NULL이다.
* NULL 비교는 `IS NULL` 또는 `IS NOT NULL`을 사용한다.
* 뷰는 실제 테이블이 아니라 논리적인 가상 테이블이다.
