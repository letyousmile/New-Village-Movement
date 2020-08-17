# Table Space

테이블 스페이스는 테이블 및 인덱스를 저장해 놓은 오라클의 논리적인 공간이다. 실제 물리적인 공간은 데이터파일(.DBF)이다. 테이블 스페이스의 관리는 관리자만 가능하다.

## 생성

```sql
CREATE TABLESPACE 테이블스페이스명
DATAFILE
'저장될경로/파일명.dbf'size 30m --size는 mb, kb만 가능
EXTENT MANAGEMENT LOCAL
SEGMENT SPACE MANAGEMENT AUTO;
```

```sql
-- 오토 익스텐드
CREATE TABLESPACE 테이블스페이스명
DATAFILE
'저장될경로/파일명.dbf'size 30m
AUTOEXTEND ON
MAXSIZE 90M
EXTENT MANAGEMENT LOCAL
SEGMENT SPACE MANAGEMENT AUTO;
```

## 조회

```sql
SELECT * FROM DBA_TABLESPACES;
```

```sql
SELECT * FROM DBA_DATA_FILES;
```

## 사용하기

```sql
CREATE TABLE TABLE_NAME (
    PKEY VARCHAR2(10)
    , UNAME VARCHAR2(10)
) TABLESPACE TABLESPACE_NAME;
```