# 디자인 패턴

### 1)생성 패턴
       (1) Singleton : 인스턴스를 하나만 생성
       (2) Factory Method : 객체 생성을 하위 클래스에 위임
       (3) Abstract Factory : 관련 객체군을 묶어 생성
       (4) Builder : 복잡한 객체를 단계적으로 생성
       (5) Prototype : 기존 객체를 복제하여 생성

       하나의 인스턴스 → Singleton
       객체 생성 인터페이스, 하위 클래스 결정 → Factory Method
       관련된 제품군 생성 → Abstract Factory
       복잡한 객체를 단계별 생성 → Builder
       복제 → Prototype

### 2) 구조 패턴
       (1) Adapter : 호환되지 않는 인터페이스를 변환
       (2) Bridge : 추상부와 구현부를 분리
       (3) Composite : 부분과 전체를 트리 구조로 동일하게 처리
       (4) Decorator : 객체에 기능을 동적으로 추가
       (5) Facade : 복잡한 서브시스템에 단순한 통합 인터페이스 제공
       (6) Flyweight : 공유를 통해 메모리 절약
       (7) Proxy : 대리 객체가 접근 제어

       어댑터  → 변환
       브리지  → 추상과 구현 분리
       컴포지트 → 트리·부분 전체
       데코레이터 → 장식·기능 추가
       퍼사드 → 단순한 창구
       플라이웨이트 → 공유
       프록시 → 대리인

### 3) 행위 패턴
       (1) Interpreter : 문법과 해석 방법 정의
       (2) Iterator : 내부 구조 노출 없이 순차 접근
       (3) Mediator : 객체 간 통신을 중재자에게 집중
       (4) Observer : 상태 변경을 여러 객체에 통보
       (5) Visitor : 데이터 구조와 처리 연산 분리

       혼동 주의
        - Observer : 한 객체 변화가 여러 객체에 통보
        - Mediator : 여러 객체의 통신을 중재자 한 곳에 집중
