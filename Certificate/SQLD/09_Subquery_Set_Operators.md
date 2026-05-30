# 서브쿼리와 집합 연산자

## 1. 서브쿼리

서브쿼리는 하나의 SQL문 안에 포함된 또 다른 SQL문이다.

복잡한 조건 필터링, 계산된 컬럼 생성, 임시 결과 사용 등에 활용한다.

## 2. 서브쿼리 사용 위치

### 1) WHERE 절 서브쿼리

조건 비교에 사용한다.

```sql
SELECT ENAME, SAL
FROM EMP
WHERE SAL > (SELECT AVG(SAL) FROM EMP);
```

### 2) HAVING 절 서브쿼리

그룹화된 결과를 조건 비교할 때 사용한다.

```sql
SELECT DEPTNO, AVG(SAL)
FROM EMP
GROUP BY DEPTNO
HAVING AVG(SAL) > (SELECT AVG(SAL) FROM EMP);
```

### 3) SELECT 절 서브쿼리

조회 결과에 계산된 값을 컬럼처럼 추가할 때 사용한다.

```sql
SELECT ENAME, SAL,
       (SELECT AVG(SAL) FROM EMP) AS AVG_SAL
FROM EMP;
```

### 4) FROM 절 서브쿼리

인라인 뷰라고 한다.

서브쿼리 결과를 임시 테이블처럼 사용한다.

```sql
SELECT E.ENAME, E.SAL, A.AVG_SAL
FROM EMP E,
     (SELECT AVG(SAL) AS AVG_SAL FROM EMP) A
WHERE E.SAL > A.AVG_SAL;
```

## 3. 동작 방식에 따른 서브쿼리 분류

### 1) 연관 서브쿼리

메인 쿼리의 각 행과 관련되어 처리되는 서브쿼리이다.

메인 쿼리 한 행을 읽을 때마다 서브쿼리가 실행될 수 있다.

```sql
SELECT ENAME, DEPTNO, SAL
FROM EMP E1
WHERE SAL > (
    SELECT AVG(SAL)
    FROM EMP E2
    WHERE E1.DEPTNO = E2.DEPTNO
);
```

### 2) 비연관 서브쿼리

메인 쿼리와 독립적으로 실행되는 서브쿼리이다.

서브쿼리가 먼저 한 번 실행되고, 그 결과를 메인 쿼리가 사용한다.

```sql
SELECT ENAME, SAL
FROM EMP
WHERE SAL > (SELECT AVG(SAL) FROM EMP);
```

## 4. 반환 형태에 따른 서브쿼리 분류

### 1) 단일행 서브쿼리

하나의 행만 반환한다.

사용 가능한 연산자:

* =
* >
* <
* >=
* <=
* <>

### 2) 다중행 서브쿼리

여러 행을 반환한다.

사용 가능한 연산자:

* IN
* ANY
* ALL
* EXISTS

```sql
SELECT ENAME, SAL
FROM EMP
WHERE DEPTNO IN (
    SELECT DEPTNO
    FROM DEPT
    WHERE LOC = 'NEW YORK'
);
```

### 3) 다중 컬럼 서브쿼리

여러 컬럼을 반환한다.

```sql
SELECT *
FROM EMP
WHERE (JOB, DEPTNO) IN (
    SELECT JOB, DEPTNO
    FROM EMP
    WHERE ENAME = 'SMITH'
);
```

## 5. DML에서의 서브쿼리

### 1) INSERT

```sql
INSERT INTO EMP (EMPNO, ENAME, SAL)
VALUES ((SELECT MAX(EMPNO) + 1 FROM EMP), 'NEW_EMP', 3000);
```

### 2) UPDATE

```sql
UPDATE EMP
SET SAL = SAL * 1.1
WHERE DEPTNO = (
    SELECT DEPTNO
    FROM DEPT
    WHERE DNAME = 'SALES'
);
```

### 3) DELETE

```sql
DELETE FROM EMP
WHERE SAL < (SELECT AVG(SAL) FROM EMP);
```

## 6. 집합 연산자

집합 연산자는 두 개 이상의 SELECT 결과를 하나의 결과로 합치는 연산자이다.

## 7. UNION

UNION은 두 SELECT 결과를 합치고 중복을 제거한다.

```sql
SELECT DEPTNO FROM DEPT
UNION
SELECT DEPTNO FROM EMP;
```

## 8. UNION ALL

UNION ALL은 두 SELECT 결과를 합치고 중복도 그대로 포함한다.

```sql
SELECT DEPTNO FROM DEPT
UNION ALL
SELECT DEPTNO FROM EMP;
```

UNION보다 중복 제거 과정이 없기 때문에 일반적으로 더 빠르다.

## 9. INTERSECT

INTERSECT는 두 SELECT 결과에 모두 포함된 행만 반환한다.

```sql
SELECT DEPTNO FROM DEPT
INTERSECT
SELECT DEPTNO FROM EMP;
```

## 10. MINUS / EXCEPT

MINUS 또는 EXCEPT는 첫 번째 SELECT 결과에서 두 번째 SELECT 결과에 포함된 행을 제외한다.

Oracle은 `MINUS`, SQL Server는 `EXCEPT`를 사용한다.

```sql
SELECT DEPTNO FROM DEPT
MINUS
SELECT DEPTNO FROM EMP;
```

## 11. 집합 연산자 사용 조건

1. 각 SELECT 문의 컬럼 수가 동일해야 한다.
2. 대응되는 컬럼의 데이터 타입이 같거나 호환 가능해야 한다.
3. 결과 컬럼명은 첫 번째 SELECT 문의 컬럼명을 따른다.
4. ORDER BY는 전체 결과에 대해 마지막에 한 번만 사용한다.

## 12. 핵심 정리

* 서브쿼리는 SQL 안에 포함된 SQL이다.
* FROM 절의 서브쿼리는 인라인 뷰라고 한다.
* 연관 서브쿼리는 메인 쿼리의 행과 관련되어 반복 실행될 수 있다.
* UNION은 중복 제거, UNION ALL은 중복 포함이다.
* 집합 연산자는 컬럼 수와 데이터 타입이 맞아야 한다.
