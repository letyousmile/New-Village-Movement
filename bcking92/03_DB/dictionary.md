# Oracle Dictionary
Data Dictionary는 대부분 읽기전용으로 제공되는 테이블 및 뷰들의 집합. 데이터베이스 전반에 대한 정보를 제공하는 일종의 메타데이터.

오라클 데이터베이스는 SQL이 실행될 때 마다 데이터 사전을 Acces한다.

DB작업동안 Oracle은 data dictionary를 읽어 객체(table, index 등)의 존재 여부와 사용자에게 적합한 Access권한등이 있는지를 확인 한다. 또한 data dictionary를 계속 갱신하여 DB구조, 감사, 사용자권한, 데이터등의 변경 사항을 반영한다.

Data dictionary에는
- 오라클의 사용자 정보
- 오라클 권한과 룰 정보
- 데이터베이스 스키마 객체(TABLE, VIEW, INDEX, CLUSTER, SYNONYM, SEQUENCE...)정보
- 무결성 제약조건에 관한 정보
- 데이터베이스 구조 정보
- 오라클 데이터베이스의 함수와 프로시저 및 트리거에 대한 정보
- 기타 일반적인 DATABASE 정보
등이 저장된다.

## 분류
- ALL_XXXX
    - ALL_ 로 시작하는 dictionary로, 한 특정 사용자가 조회 가능한 모든 dictionary를 의미한다. 자신이 조회하려는 객체의 주인이 아니더라도 접근권한이 있다면 ALL_XXXX 뷰를 통해 조회할 수 있다.

- USER_XXXX
    - USER_ 로 시작하는 dictionary. 특정 사용자에게 종속되어 있고 그 사용자가 조회 가능한 dictionary로 ALL_XXXX dictionary의 부분집합이다.

- DBA_XXXX
    - DBA 권한을 가진 사용자만이 조회할 수 있는 데이터 사전. 모든 오라클 데이터베이스 객체에 대한 정보를 볼 수 있다. SELECT ANY TABLE권한이 있는 사용자 또한 질의가 가능하고 다른 사용자가 질의하려면 앞에 SYS 접두어를 붙여야 한다.

- V$_XXXX
    - Dynamic Performance View라고도 하며, 현재 DB의 상태에 관한 정보로 주로 DBA에게만 엑세스가 허용되어 있는 dictionary이다.
    주로 DBA의 모니터링 작업용 정보를 제공하며, X$테이블을 베이스로 하는 뷰이다.

- X$_XXXX
    - X$ 뷰는 V$ 뷰가 보여주지 않는 정보를 보여준다.
    X$ 테이블은 오라클의 메모리정보를 볼 수 있는 SQL 인터페이스 뷰들로 ORACLE데이터베이스의 가장 숨겨진 영역 중 하나이다.
    