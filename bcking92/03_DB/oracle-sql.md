# SQL(Structured Query language)
- 데이터베이스에서 자료를 검색하고 수정하고 삭제하는 등을 위한 데이터베이스 언어
- 관계 데이터베이스(RDB)를 처리하기 위해 고안된 언어
- 독자적인 문법을 갖는 DB표준 언어(ISO에 의해 지정)
## SQL 종류
- DQL(Data Query Language, 데이터 질의어)
    - SELECT
- DML(Data Manipulation Language, 데이터 조작어)
    - INSERT, UPDATE, DELETE
- DDL(Data Definition Language, 데이터 정의어)
    - CREATE, ALTER, DROP, RENAME, TRUNCATE
- TCL(Transaction Control Language, 트렌잭션 처리어)
    - COMMIT, ROLLBACK, SAVEPOINT
- DCL(Data Control Language, 데이터 제어어)
    - GRANT, REVOKE


## SELECT
- `DESC`: 테이블의 구조를 확인하기 위한 명령어
    ```sql
    DESC TABLE_NAME
    ``` 
- `SELECT`: 테이블에 저장된 데이터를 조회하기 위한 명령어
    ```sql
    SELECT * FROM TABLE_NAME
    ```
- `WHERE`: SELECT문에서 원하는 조건의 레코드를 검색하고자 할 때 사용한다.
    ```sql
    SELECT * FROM TABLE_NAME WHERE AGE > 10
    ```
- 산술/비교/논리 연산자
    - 산술
        - `+`, `-`, `*`, `/`
    - 비교
        - `=`: 같다
        - `>`: 크다
        - `<`: 작다
        - `>=`: 크거나 같다
        - `<=`: 작거나 같다
        - `<>`, `!=`, `^=`: 다르다
    - 논리
        - `AND`: 두 조건을 모두 만족
        - `OR`: 두 조건 중에서 하나라도 만족
        - `NOT`: 조건에 맞지 않는 것만
- `BETWEEN AND`: 하나의 컬럼의 값이 범위내에 속하는지를 알아보는 연산자. 숫자형, 문자형, 날짜형에 사용가능
    ```sql
    SELECT * FROM TABLE_NAME WHERE DATE BETWEEN '20200601' AND '20200630'
    ```
- `IN`: 하나의 컬럼의 값이 특정 값들에 속하는지 알아보는 연산자.
    ```sql
    SELECT * FROM TABLE_NAME WHERE AGE IN ('10', '15', '20')
    ```

- `LIKE`: 컬럼에 저장된 데이터의 시작 위치에 데이터가 일치하면 조회가 가능한 연산자. 검색하고자 하는 값을 정확하게 모를 때는 와일드카드(`%`, `_`)와 함께 사용할 수 있다. %, _ 문자를 검색하고 싶다면 ESCAPE(`\`)를 사용하여 검색한다. `NOT` 연사자와 함께 `NOT LIKE`의 형태로도 사용할 수 있다.
    ```sql
    SELECT * FROM TABLE_NAME WHERE U_NAME LIKE '%병%' 
    ```
    ```sql
    SELECT * FROM TABLE_NAME WHERE U_NAME LIKE '_병_'
    ```

- `NULL`: `NULL`은 값이 아니므로 `COL = NULL`과 같이 표기할 수 없다. `NULL`을 구분할 때는 `IS NULL` 또는 `IS NOT NULL` 명령어를 사용한다.

- `ORDER BY`: 데이터를 정렬하는 기준을 표시하는 명령어 `ASC` 오름차순과, `DESC` 내림차순이 있다. 디폴트는 ASC   
    ```sql
    SELECT * FROM TABLE_NAME ORDER BY PHONE_NUMBER DESC
    ```

- `DISTINCT`: 동일한 데이터 값들이 중복되어 출력되지 않도록 할 때 사용한다.
    ```sql
    SELECT DISTINCT AGE FROM STUDENT
    ```

- 연결연산자(`||`, CONCAT): 문자열을 연결하여 출력할 때 사용한다.
    ```sql
    SELECT U_NAME || 'is a' || U_AGE  AS ST_AGE FROM STUDENT
    ```
