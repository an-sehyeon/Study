# JOIN

## 1. JOIN의 개념

JOIN은 두 개 이상의 테이블을 연결하여 데이터를 조회하는 방법이다.

테이블 간 관련 있는 컬럼을 기준으로 데이터를 합쳐서 조회한다.

## 2. INNER JOIN

INNER JOIN은 두 테이블에서 조인 조건이 일치하는 행만 반환한다.

```sql
SELECT E.EMPNO, E.ENAME, D.DNAME
FROM EMP E INNER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

## 3. LEFT OUTER JOIN

LEFT OUTER JOIN은 왼쪽 테이블의 모든 행을 반환하고, 오른쪽 테이블에서 일치하는 행이 있으면 함께 반환한다.

일치하는 행이 없으면 오른쪽 테이블의 컬럼은 NULL로 표시된다.

```sql
SELECT E.EMPNO, E.ENAME, D.DNAME
FROM EMP E LEFT OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

Oracle 구문:

```sql
SELECT E.EMPNO, E.ENAME, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO(+);
```

`(+)`가 붙은 쪽이 부족한 데이터를 NULL로 채우는 쪽이다.

## 4. RIGHT OUTER JOIN

RIGHT OUTER JOIN은 오른쪽 테이블의 모든 행을 반환하고, 왼쪽 테이블에서 일치하는 행이 있으면 함께 반환한다.

```sql
SELECT E.EMPNO, E.ENAME, D.DNAME
FROM EMP E RIGHT OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

## 5. FULL OUTER JOIN

FULL OUTER JOIN은 양쪽 테이블의 모든 행을 반환한다.

일치하지 않는 데이터는 NULL로 표시된다.

```sql
SELECT E.EMPNO, E.ENAME, D.DNAME
FROM EMP E FULL OUTER JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

## 6. CROSS JOIN

CROSS JOIN은 두 테이블의 모든 행을 조합한다.

카티션 곱이라고도 한다.

```sql
SELECT E.ENAME, D.DNAME
FROM EMP E CROSS JOIN DEPT D;
```

## 7. 등가 JOIN

등가 JOIN은 `=` 연산자를 사용하여 두 테이블의 컬럼 값이 같은 경우를 조인한다.

```sql
SELECT E.ENAME, D.DNAME
FROM EMP E JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;
```

## 8. 비등가 JOIN

비등가 JOIN은 `= 이외의 연산자`를 사용하여 조인한다.

`ex) >, <, BETWEEN`

```sql
SELECT E.ENAME, E.SAL, S.GRADE
FROM EMP E JOIN SALGRADE S
ON E.SAL BETWEEN S.LOSAL AND S.HISAL;
```

## 9. NATURAL JOIN

NATURAL JOIN은 두 테이블에 동일한 이름을 가진 모든 컬럼을 자동으로 조인한다.

```sql
SELECT ENAME, DNAME
FROM EMP NATURAL JOIN DEPT;
```

주의할 점:

* 같은 이름의 컬럼이 의도치 않게 여러 개 있으면 예상과 다른 결과가 나올 수 있다.

## 10. USING 조건절

USING은 조인할 특정 컬럼을 명시한다.

두 테이블에 같은 이름의 컬럼이 있을 때 사용할 수 있다.

```sql
SELECT ENAME, DNAME, DEPTNO
FROM EMP JOIN DEPT USING (DEPTNO);
```

## 11. 3개 이상 테이블 JOIN

```sql
SELECT E.ENAME, D.DNAME, L.CITY
FROM EMP E
JOIN DEPT D ON E.DEPTNO = D.DEPTNO
JOIN LOCATIONS L ON D.LOC_ID = L.LOC_ID;
```

## 12. SELF JOIN

SELF JOIN은 같은 테이블을 두 번 이상 참조하여 조인하는 방식이다.

계층형 데이터에서 상위-하위 관계를 조회할 때 자주 사용한다.

```sql
SELECT E1.EMPNO, E1.ENAME, E2.ENAME AS MANAGER_NAME
FROM EMP E1, EMP E2
WHERE E1.MGR = E2.EMPNO(+);
```

## 13. 핵심 정리

* INNER JOIN은 조건이 일치하는 데이터만 조회한다.
* OUTER JOIN은 한쪽 또는 양쪽 테이블의 데이터를 유지하면서 조회한다.
* CROSS JOIN은 모든 조합을 만든다.
* NATURAL JOIN은 같은 이름의 컬럼을 자동으로 조인한다.
* SELF JOIN은 같은 테이블을 자기 자신과 조인한다.
