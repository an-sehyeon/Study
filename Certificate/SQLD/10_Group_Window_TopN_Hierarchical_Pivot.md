# 그룹 함수, 윈도우 함수, 계층형 질의

## 1. 그룹 함수

그룹 함수는 여러 행을 기준에 따라 묶어서 집계 결과를 만드는 함수이다.

## 2. ROLLUP

ROLLUP은 GROUP BY 절과 함께 사용하여 지정된 컬럼의 소계와 총계를 자동으로 구한다.

계층적인 집계 결과를 만들 때 사용한다.

```sql
SELECT DEPTNO, JOB, SUM(SAL) AS TOTAL_SALARY
FROM EMP
GROUP BY ROLLUP(DEPTNO, JOB)
ORDER BY DEPTNO;
```

결과에 포함되는 집계:

* 부서 + 직무별 합계
* 부서별 합계
* 전체 합계

## 3. CUBE

CUBE는 지정된 컬럼들의 가능한 모든 조합에 대해 집계를 수행한다.

```sql
SELECT DEPTNO, JOB, SUM(SAL)
FROM EMP
GROUP BY CUBE(DEPTNO, JOB);
```

결과에 포함되는 집계:

* 부서 + 직무별 합계
* 부서별 합계
* 직무별 합계
* 전체 합계

## 4. GROUPING SETS

GROUPING SETS는 원하는 특정 그룹의 집계만 선택해서 계산한다.

```sql
SELECT DEPTNO, JOB, SUM(SAL)
FROM EMP
GROUP BY GROUPING SETS ((DEPTNO, JOB), (DEPTNO), (JOB), ());
```

## 5. GROUPING 함수

GROUPING 함수는 특정 컬럼이 집계에 사용되었는지 여부를 반환한다.

* 집계에 사용되었으면 0
* 집계에 사용되지 않았으면 1

```sql
SELECT
    CASE GROUPING(DEPTNO)
        WHEN 1 THEN '전체 부서'
        ELSE TO_CHAR(DEPTNO)
    END AS DEPTNO,
    CASE GROUPING(JOB)
        WHEN 1 THEN '전체 직무'
        ELSE JOB
    END AS JOB,
    SUM(SAL) AS TOTAL_SALARY
FROM EMP
GROUP BY ROLLUP(DEPTNO, JOB);
```

## 6. 윈도우 함수

윈도우 함수는 행과 행 간의 관계를 쉽게 정의하기 위해 제공되는 함수이다.

집계 함수와 달리 행이 사라지지 않고 그대로 유지되면서 계산 결과가 추가된다.

## 7. 윈도우 함수 기본 구조

```sql
윈도우함수(인자) OVER (
    PARTITION BY 컬럼
    ORDER BY 컬럼
    ROWS 또는 RANGE BETWEEN 시작점 AND 끝점
)
```

## 8. 순위 관련 함수

### 1) ROW_NUMBER

중복 값이 있어도 순차적인 번호를 부여한다.

```sql
SELECT ENAME, SAL,
       ROW_NUMBER() OVER (ORDER BY SAL DESC) AS RN
FROM EMP;
```

### 2) RANK

동일한 값은 동일한 순위를 부여하고 다음 순위는 건너뛴다.

```sql
SELECT ENAME, SAL,
       RANK() OVER (ORDER BY SAL DESC) AS RANK_SAL
FROM EMP;
```

### 3) DENSE_RANK

동일한 값은 동일한 순위를 부여하지만 다음 순위를 건너뛰지 않는다.

```sql
SELECT ENAME, SAL,
       DENSE_RANK() OVER (ORDER BY SAL DESC) AS DENSE_RANK_SAL
FROM EMP;
```

## 9. 집계 관련 윈도우 함수

### 1) SUM OVER

```sql
SELECT ENAME, DEPTNO, SAL,
       SUM(SAL) OVER (PARTITION BY DEPTNO) AS DEPT_SUM
FROM EMP;
```

### 2) AVG OVER

```sql
SELECT ENAME, DEPTNO, SAL,
       AVG(SAL) OVER (PARTITION BY DEPTNO) AS DEPT_AVG
FROM EMP;
```

### 3) MAX / MIN OVER

```sql
SELECT ENAME, DEPTNO, SAL,
       MAX(SAL) OVER (PARTITION BY DEPTNO) AS DEPT_MAX,
       MIN(SAL) OVER (PARTITION BY DEPTNO) AS DEPT_MIN
FROM EMP;
```

## 10. 행 이동 관련 함수

### 1) LAG

이전 행의 값을 참조한다.

```sql
SELECT ENAME, HIREDATE, SAL,
       LAG(SAL) OVER (ORDER BY HIREDATE) AS PREV_SAL
FROM EMP;
```

### 2) LEAD

다음 행의 값을 참조한다.

```sql
SELECT ENAME, HIREDATE, SAL,
       LEAD(SAL) OVER (ORDER BY HIREDATE) AS NEXT_SAL
FROM EMP;
```

## 11. 누적 관련 함수

### 1) CUME_DIST

누적 분포 값을 계산한다.

### 2) PERCENT_RANK

백분위 순위를 계산한다.

## 12. Top N 쿼리

Top N 쿼리는 정렬된 결과 중 상위 N개 행을 조회하는 방식이다.

## 13. ROWNUM

ROWNUM은 Oracle에서 제공하는 가상 컬럼이다.

쿼리 결과의 각 행에 1부터 순차적인 번호를 부여한다.

```sql
SELECT ROWNUM, ENAME, SAL
FROM EMP
WHERE ROWNUM <= 5;
```

## 14. Top N 쿼리 주의점

아래 쿼리는 원하는 결과가 나오지 않을 수 있다.

```sql
SELECT ROWNUM, ENAME, SAL
FROM EMP
WHERE ROWNUM <= 5
ORDER BY SAL DESC;
```

이유는 ROWNUM이 먼저 부여된 후 정렬되기 때문이다.

올바른 방식은 인라인 뷰에서 먼저 정렬하고 바깥 쿼리에서 ROWNUM을 적용하는 것이다.

```sql
SELECT ROWNUM, E.*
FROM (
    SELECT ENAME, SAL
    FROM EMP
    ORDER BY SAL DESC
) E
WHERE ROWNUM <= 5;
```

## 15. 계층형 질의

계층형 질의는 테이블에 저장된 상위-하위 구조의 데이터를 조회하는 방법이다.

`ex) 조직도, 카테고리, 메뉴 구조`

Oracle에서는 `START WITH`, `CONNECT BY`를 사용한다.

```sql
SELECT ENAME, EMPNO, MGR, LEVEL
FROM EMP
START WITH MGR IS NULL
CONNECT BY PRIOR EMPNO = MGR;
```

## 16. 계층형 질의 주요 요소

* START WITH : 계층 구조의 시작점 지정
* CONNECT BY : 부모-자식 관계 정의
* PRIOR : 부모와 자식의 방향 지정
* LEVEL : 현재 계층의 깊이
* CONNECT_BY_ROOT : 현재 행의 최상위 조상 반환
* SYS_CONNECT_BY_PATH : 루트부터 현재 노드까지의 경로 반환
* CONNECT_BY_ISLEAF : 현재 행이 리프 노드인지 여부 반환
* ORDER SIBLINGS BY : 같은 레벨 내에서 정렬

## 17. PIVOT

PIVOT은 행을 열로 변환하는 기능이다.

```sql
SELECT *
FROM (
    SELECT DEPTNO, JOB
    FROM EMP
)
PIVOT (
    COUNT(*)
    FOR JOB IN (
        'CLERK' AS CLERK,
        'MANAGER' AS MANAGER,
        'ANALYST' AS ANALYST,
        'SALESMAN' AS SALESMAN
    )
);
```

## 18. UNPIVOT

UNPIVOT은 열을 행으로 변환하는 기능이다.

```sql
SELECT *
FROM 피벗된_테이블
UNPIVOT (
    EMP_COUNT
    FOR JOB_TYPE IN (CLERK, MANAGER, ANALYST, SALESMAN)
);
```

## 19. 핵심 정리

* ROLLUP은 계층적 소계와 총계를 구한다.
* CUBE는 가능한 모든 조합의 집계를 구한다.
* GROUPING SETS는 원하는 집계 조합만 선택한다.
* 윈도우 함수는 행을 유지한 채 계산 결과를 추가한다.
* Top N 쿼리는 먼저 정렬하고 바깥에서 ROWNUM을 적용해야 한다.
* 계층형 질의는 상위-하위 구조를 조회할 때 사용한다.
* PIVOT은 행을 열로, UNPIVOT은 열을 행으로 바꾼다.
