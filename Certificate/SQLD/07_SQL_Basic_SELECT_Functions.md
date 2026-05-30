# SQL 기본 SELECT 문과 함수

## 1. SELECT 문

SELECT 문은 테이블에서 데이터를 조회할 때 사용한다.

```sql
SELECT 컬럼명
FROM 테이블명
WHERE 조건;
```

## 2. DISTINCT

DISTINCT는 중복된 데이터를 제거하고 한 건으로 출력한다.

```sql
SELECT DISTINCT DEPTNO
FROM EMP;
```

## 3. SQL 실행 순서

```text
FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY
```

암기 키워드:

```text
FWGHSO
```

## 4. 단일행 함수와 다중행 함수

### 1) 단일행 함수

각 행에 대해 하나의 결과값을 반환한다.

* 문자형 함수
* 숫자형 함수
* 날짜형 함수
* 변환형 함수
* NULL 관련 함수

### 2) 다중행 함수

여러 행의 데이터를 집계하여 하나의 결과값을 반환한다.

* 집계 함수
* 그룹 함수
* 윈도우 함수

## 5. 문자형 함수

### 1) LOWER

문자열을 소문자로 변환한다.

```sql
SELECT LOWER('HELLO WORLD')
FROM DUAL;
```

### 2) UPPER

문자열을 대문자로 변환한다.

```sql
SELECT UPPER('hello world')
FROM DUAL;
```

### 3) SUBSTR

문자열의 일부분을 추출한다.

```sql
SELECT SUBSTR('Hello World', 1, 5)
FROM DUAL;
```

### 4) LENGTH

문자열의 길이를 반환한다.

```sql
SELECT LENGTH('Hello World')
FROM DUAL;
```

### 5) TRIM / LTRIM / RTRIM

* TRIM : 양쪽 공백 제거
* LTRIM : 왼쪽 공백 제거
* RTRIM : 오른쪽 공백 제거

### 6) CONCAT

두 문자열을 연결한다.

```sql
SELECT CONCAT('Hello', 'SQL')
FROM DUAL;
```

Oracle에서는 `||` 연산자를 사용하여 문자열을 연결할 수 있다.

```sql
SELECT FIRST_NAME || LAST_NAME AS FULL_NAME
FROM EMP;
```

## 6. 숫자형 함수

### 1) ABS

절댓값을 반환한다.

### 2) SIGN

숫자의 부호를 반환한다.

### 3) ROUND

숫자를 반올림한다.

```sql
SELECT ROUND(3.14159, 2)
FROM DUAL;
```

### 4) TRUNC

숫자를 절삭한다.

```sql
SELECT TRUNC(3.14159, 2)
FROM DUAL;
```

### 5) CEIL

숫자보다 크거나 같은 최소 정수를 반환한다.

```sql
SELECT CEIL(4.3)
FROM DUAL;
```

### 6) FLOOR

숫자보다 작거나 같은 최대 정수를 반환한다.

```sql
SELECT FLOOR(4.7)
FROM DUAL;
```

### 7) MOD

나눗셈 후 나머지를 반환한다.

```sql
SELECT MOD(17, 5)
FROM DUAL;
```

### 8) POWER

거듭제곱을 반환한다.

```sql
SELECT POWER(2, 3)
FROM DUAL;
```

### 9) SQRT

제곱근을 반환한다.

```sql
SELECT SQRT(9)
FROM DUAL;
```

## 7. 날짜형 함수

### 1) SYSDATE

현재 날짜와 시간을 반환한다.

### 2) ADD_MONTHS

지정한 개월 수만큼 날짜를 더한다.

### 3) MONTHS_BETWEEN

두 날짜 사이의 개월 수를 계산한다.

### 4) NEXT_DAY

지정한 날짜 이후의 특정 요일 날짜를 반환한다.

### 5) LAST_DAY

해당 월의 마지막 날짜를 반환한다.

### 6) EXTRACT

날짜에서 특정 부분을 추출한다.

## 8. 변환형 함수

### 1) TO_CHAR

날짜나 숫자를 문자열로 변환한다.

```sql
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD')
FROM DUAL;
```

### 2) TO_DATE

문자열을 날짜로 변환한다.

```sql
SELECT TO_DATE('2024-07-01', 'YYYY-MM-DD')
FROM DUAL;
```

### 3) TO_NUMBER

문자열을 숫자로 변환한다.

### 4) CAST

데이터 타입을 변환한다.

```sql
SELECT CAST('12345' AS NUMBER)
FROM DUAL;
```

## 9. GROUP BY

GROUP BY는 특정 컬럼을 기준으로 데이터를 그룹화한다.

```sql
SELECT DEPTNO, AVG(SAL)
FROM EMP
GROUP BY DEPTNO;
```

주의할 점:

* GROUP BY 절에서는 SELECT 절의 별칭을 사용할 수 없다.
* SELECT 절에는 GROUP BY에 사용한 컬럼이나 집계 함수만 사용할 수 있다.

## 10. HAVING

HAVING은 그룹화 이후 그룹에 조건을 적용할 때 사용한다.

```sql
SELECT DEPTNO, AVG(SAL)
FROM EMP
GROUP BY DEPTNO
HAVING AVG(SAL) >= 2000;
```

## 11. WHERE와 HAVING의 차이

* WHERE : 그룹화 전 개별 행을 필터링한다.
* HAVING : 그룹화 후 그룹을 필터링한다.
* WHERE 절에는 집계 함수를 사용할 수 없다.
* HAVING 절에는 집계 함수를 사용할 수 있다.

## 12. ORDER BY

ORDER BY는 결과를 정렬한다.

```sql
SELECT ENAME, SAL
FROM EMP
ORDER BY SAL DESC;
```

## 13. 핵심 정리

* SELECT 문은 데이터를 조회하는 문장이다.
* DISTINCT는 중복을 제거한다.
* WHERE는 그룹화 전 조건, HAVING은 그룹화 후 조건이다.
* GROUP BY를 사용할 때 SELECT 절에는 그룹 기준 컬럼 또는 집계 함수만 올 수 있다.
