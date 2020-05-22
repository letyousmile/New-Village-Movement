# ORACLE SQL

- COMMENT 조회
```sql
SELECT *
FROM ALL_COL_COMMENTS
WHERE TABLE_NAME = 'TABLE_NAME';
```

- `rownum`
```sql
SELECT *
FROM 'TABLE'
WHERE rownum < 10;
```
TABLE의 데이터를 10개만 조회

- `validation-query`

DB를 사용하기 전에 해당 DB와 Connection이 정상인지 확인하기 위해 사용하는 query

- `ORACLE RCA`

서버 부하를 막기위해 DB는 하나 서버입구를 두개로둠