# SQLD 정리

SQLD 공부 내용을 과목과 개념 단위로 나누어 정리한 폴더

## 파일 구성

[1. 데이터 모델링의 이해](./01_Data_Modeling_Basics.md)
   - 데이터 모델링의 특징
   - 데이터 모델링 3단계
   - 데이터 모델이 제공하는 기능

[02. 스키마와 데이터 독립성](./02_Schema_Data_Independence.md)
   - 외부 스키마
   - 개념 스키마
   - 내부 스키마
   - 논리적/물리적 데이터 독립성

[03. 관계형 모델과 SQL 개요](./03_Relational_Model_SQL_Overview.md)
   - 릴레이션, 속성, 튜플, 도메인
   - DDL, DML, DCL, TCL
   - 관계 대수와 SQL 실행 순서

[04. 엔터티, 속성, 식별자](./04_Entity_Attribute_Identifier.md)
   - 엔터티
   - 속성
   - 식별자
   - 식별자 관계와 비식별자 관계

[05. 정규화와 반정규화](./05_Normalization_Denormalization.md)
   - 정규화
   - 이상 현상
   - 반정규화

[06. 트랜잭션, NULL, VIEW](./06_Transaction_NULL_View.md)
   - 트랜잭션
   - ACID
   - NULL
   - 뷰

[07. SELECT문과 함수](./07_SQL_Basic_SELECT_Functions.md)
   - SELECT 문
   - SQL 함수
   - GROUP BY / HAVING / ORDER BY

[08. JOIN](./08_JOIN.md)
   - INNER JOIN
   - OUTER JOIN
   - CROSS JOIN
   - NATURAL JOIN / USING
   - 셀프 조인

[09. 서브쿼리와 집합 연산자](./09_Subquery_Set_Operators.md)
   - 서브쿼리
   - 집합 연산자

[10. 그룹 함수, 윈도우 함수, Top N, 계층형 질의, PIVOT](./10_Group_Window_TopN_Hierarchical_Pivot.md)
    - ROLLUP, CUBE, GROUPING SETS
    - 윈도우 함수
    - Top N 쿼리
    - 계층형 질의
    - PIVOT / UNPIVOT

[11. DML, DDL, 제어 관련 문법](./11_DML_DDL_Control.md)
    - DELETE / TRUNCATE
    - MERGE
    - ALTER TABLE
    - DROP TABLE
    - 참조 무결성 옵션



## 추가 개념 정리 예시

- [엔터티 분류](./Entity_Classifications.md)
- [GROUPING / CASE / DECODE](./GROUPING_CASE_DECODE.md)
