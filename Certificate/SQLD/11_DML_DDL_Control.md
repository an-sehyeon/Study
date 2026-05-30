# DML, DDL, 제어 구문

## 1. DELETE와 TRUNCATE

DELETE와 TRUNCATE는 모두 데이터를 삭제할 때 사용하지만 동작 방식이 다르다.

## 2. DELETE

DELETE는 행 단위로 데이터를 삭제한다.

특징:

* WHERE 절을 사용하여 특정 조건의 데이터만 삭제할 수 있다.
* 각 행을 개별적으로 삭제하므로 상대적으로 느릴 수 있다.
* 트랜잭션 로그를 기록한다.
* 트랜잭션 내에서 ROLLBACK이 가능하다.
* 인덱스를 유지한다.

```sql
DELETE FROM EMP
WHERE DEPTNO = 10;
```

## 3. TRUNCATE

TRUNCATE는 테이블 구조는 유지하고 전체 데이터를 한 번에 삭제한다.

특징:

* WHERE 절을 사용할 수 없다.
* 항상 전체 테이블을 비운다.
* DELETE보다 일반적으로 빠르다.
* 최소한의 로그만 생성한다.
* 일반적으로 ROLLBACK이 불가능하다.
* 인덱스를 재설정한다.

```sql
TRUNCATE TABLE EMP;
```

## 4. MERGE

MERGE는 INSERT, UPDATE, DELETE 작업을 하나의 문장으로 처리할 수 있는 명령어이다.

데이터가 이미 있으면 수정하고, 없으면 삽입하는 방식으로 많이 사용한다.

```sql
MERGE INTO EMP_MNG M
USING EMP E
ON (M.EMPNO = E.EMPNO)
WHEN MATCHED THEN
    UPDATE SET
        M.SAL = E.SAL,
        M.COMM = E.COMM
WHEN NOT MATCHED THEN
    INSERT (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
    VALUES (E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE, E.SAL, E.COMM, E.DEPTNO);
```

## 5. 참조 무결성 옵션

외래 키를 사용할 때 부모 테이블의 데이터가 삭제될 경우 자식 테이블을 어떻게 처리할지 정할 수 있다.

### 1) ON DELETE CASCADE

부모 테이블의 행이 삭제되면 자식 테이블의 관련 행도 함께 삭제한다.

### 2) ON DELETE SET NULL

부모 테이블의 행이 삭제되면 자식 테이블의 외래 키 값을 NULL로 설정한다.

### 3) ON DELETE SET DEFAULT

부모 테이블의 행이 삭제되면 자식 테이블의 외래 키 값을 기본값으로 설정한다.

### 4) ON DELETE NO ACTION

자식 테이블에 참조하는 행이 있으면 부모 테이블의 행을 삭제할 수 없다.

## 6. 참조 무결성 예시

```sql
CREATE TABLE CHILD_TABLE_CASCADE (
    CHILD_ID NUMBER PRIMARY KEY,
    PARENT_ID NUMBER,
    CONSTRAINT FK_PARENT_CASCADE FOREIGN KEY (PARENT_ID)
        REFERENCES PARENT_TABLE (PARENT_ID) ON DELETE CASCADE
);
```

## 7. ALTER TABLE

ALTER TABLE은 테이블 구조를 변경할 때 사용한다.

## 8. 컬럼 추가

```sql
ALTER TABLE EMP ADD PHONE_NUMBER VARCHAR2(15) NOT NULL;
```

## 9. 컬럼 삭제

```sql
ALTER TABLE EMP DROP COLUMN PHONE_NUMBER;
```

## 10. 컬럼명 변경

```sql
ALTER TABLE EMP RENAME COLUMN NAME TO FULL_NAME;
```

## 11. 데이터 타입 변경

```sql
ALTER TABLE EMP MODIFY SALARY NUMBER(10, 2);
```

## 12. 제약 조건 추가

```sql
ALTER TABLE EMP ADD CONSTRAINT CHK_SALARY CHECK (SALARY > 0);
```

## 13. 제약 조건 삭제

```sql
ALTER TABLE EMP DROP CONSTRAINT CHK_SALARY;
```

## 14. 기본값 설정

```sql
ALTER TABLE EMP MODIFY JOIN_DATE DATE DEFAULT SYSDATE;
```

## 15. DROP TABLE과 TRUNCATE TABLE

### 1) DROP TABLE

DROP TABLE은 테이블의 구조와 데이터를 모두 삭제한다.

특징:

* 테이블 자체가 삭제된다.
* 인덱스, 트리거, 제약 조건도 함께 삭제된다.
* 일반적으로 복구가 어렵다.
* 테이블 공간을 반환한다.

```sql
DROP TABLE EMP;
```

### 2) TRUNCATE TABLE

TRUNCATE TABLE은 테이블 구조는 유지하고 데이터만 삭제한다.

특징:

* 테이블 구조는 남아 있다.
* 인덱스, 트리거, 제약 조건은 유지된다.
* 일반적으로 복구가 어렵다.
* 테이블 공간을 초기화한다.

```sql
TRUNCATE TABLE EMP;
```

## 16. 데이터 사전

데이터 사전은 시스템 카탈로그라고도 한다.

DBMS가 자동으로 생성하고 유지하는 데이터베이스 구조 및 통계 정보이다.

포함되는 정보:

* 테이블
* 뷰
* 인덱스
* 패키지
* 접근 권한
* 통계 정보

일반 사용자는 데이터 사전을 조회할 수 있지만 직접 갱신할 수 없다.

## 17. 핵심 정리

* DELETE는 조건 삭제가 가능하고 ROLLBACK이 가능하다.
* TRUNCATE는 전체 삭제만 가능하고 일반적으로 ROLLBACK이 불가능하다.
* DROP은 테이블 자체를 삭제한다.
* ALTER TABLE은 테이블 구조를 변경한다.
* MERGE는 있으면 UPDATE, 없으면 INSERT하는 방식으로 사용한다.
* 참조 무결성 옵션은 부모 데이터 삭제 시 자식 데이터를 어떻게 처리할지 정한다.
