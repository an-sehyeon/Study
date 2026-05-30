# GROUPING / CASE / DECODE에 대해..

# 1. GROUPING 함수
* ROLLUP이나 CUBE로 만들어진 소계/총계 행인지 구분하기 위해 사용하는 함수
* 특정 컬럼이 집계에 사용되었는지 여부를 반환
* 집계에 사용되었으면 0, 그렇지 않으면 1을 반환
```
기본 문법
GROUPING(컬럼명)

반환값
0 : 해당 컬럼이 실제 데이터로 그룹화된 행
1 : 해당 컬럼이 소계/총계 때문에 비워진 행

GROUPING(컬럼) = 0
→ 실제 데이터 행

GROUPING(컬럼) = 1
→ ROLLUP/CUBE가 만든 소계 또는 총계 행
```

# 2. DECODE 함수
* DECODE는 오라클에서 사용하는 조건 처리 함수.
```
기본 구조
DECODE(비교대상, 조건값1, 결과값1, 조건값2, 결과값2, 기본값)
→ 비교대상이 조건1과 같으면 결과값1 반환
→ 비교대상이 조건2와 같으면 결과값2 반환
→ 아무 조건에도 해당하지 않으면 기본값 반환
```
### 예시
```plantuml
SELECT
    empno,
    ename,
    deptno,
    DECODE(deptno, 
            10, 'ACCOUNTING',
            20, 'RESEARCH',
            30, 'SALES',
            'UNKNOWN') AS dept_name
            
→ deptno가 10이면 ACCOUNTING
→ deptno가 20이면 RESEARCH
→ deptno가 30이면 SALES
→ 그 외에는 UNKNOWN   
```

# 3. CASE WHEN 문법
* CASE WHEN은 조건식을 직접 작성할 수 있는 SQL 표준 문법.
```plantuml
기본 구조
CASE
    WHEN 조건1 THEN 결과1
    WHEN 조건2 THEN 결과2
    WHEN 조건3 THEN 결과3
    ELSE 기본값
END
```
### 예시(급여에 따라 등급 출력)
```plantuml
SELECT
    empno,
    ename,
    sal,
    CASE
        WHEN sal >= 3000 THEN '상'
        WHEN sal >= 3000 THEN '중'
        ELSE '하'
    END AS salary_grade
FROM emp;

→ sal이 3000 이상이면 '상'
→ sal이 2000 이상이면 '중'
→ 그 외에는 '하'    
```

# 4. DECODE와 CASE WHEN 차이
| 구분     | DECODE        | CASE WHEN       |
| ------ | ------------- | --------------- |
| 사용 DB  | Oracle 중심     | 대부분의 DB에서 사용 가능 |
| 비교 방식  | 주로 `값 = 값` 비교 | 다양한 조건 사용 가능    |
| 가독성    | 조건이 많아지면 헷갈림  | 비교적 읽기 쉬움       |
| 범위 조건  | 불편함           | 편함              |
| SQL 표준 | 아님            | 표준에 가까움         |
* DECODE는 기본적으로 같은지 비교할 때 편함.

# 💡 CASE WHEN에는 두 가지 형태가 있음
## 1.단순 CASE
* 특정 컬럼 값이 무엇인지 비교할 때 사용
```plantuml
전체 SQL로 보면 : 
SELECT
    empno,
    ename,
    deptno,
    CASE deptno
        WHEN 10 THEN 'ACCOUNTING'
        WHEN 20 THEN 'RESEARCH'
        WHEN 30 THEN 'SALES'
        ELSE 'UNKNOWN'
    END AS dept_name
FROM emp;

→ DECODE와 거의 비슷한 방식   
```

## 2.검색 CASE
* 조건식을 직접 작성할 때 사용.
```plantuml
전체 SQL로 보면 :
SELECT
    empno,
    ename,
    sal,
    CASE
        WHEN sal >= 3000 THEN '상'
        WHEN sal >= 2000 THEN '중'
        ELSE '하'
    END AS salary_grade
FROM emp;
```

### 📌 단순히 코드값을 이름으로 바꿀 때 : DECODE 또는 단순 CASE
### 📌 범위 조건, 복잡한 조건, 여러 조건 조합 : CASE WHEN