# INDEX

인데스는 데이터베이스 테이블에 있는 데이터를 빨리 찾기 위한 용도의 데이터베이스 객체이며 일종의 색인기술이다. 테이블에 index를 생성하게 되면 index table을 생성해 관리한다. 인덱스는 테이블에 있는 하나 이상의 컬럼으로 만들 수 있다. 가장 일반적인 B-tree(BALANCED TREE)인덱스는 인덱스 키(인덱스로 만들 테이블의 컬럼 값)와 이 키에 해당하는 컬럼 값을 가진 테이블의 로우가 저장된 주소값으로 구성된다.

## 생성
```sql
CREATE INDEX [INDEX_NAME] ON [TABLE](COL1, COL2, ...)
```

```SQL
CREATE INDEX EX_INDEX ON CUSTOMERS(UNAME, UADDRESS);

CREATE[UNIQUE] INDEX EX_INDEX ON CUSTOMERS(UNAME, UADDRESS);
```
`UNIQUE` 키워드를 붙이면 컬럼에 중복을 허용하지 않는다는 뜻이다.

## 조회
```sql
SELECT * FROM USER_INDEXES WHERE TABLE_NAME = 'CUSTOMERS';
```
위에서 만들었던 EX_INDEX 인덱스를 조회할 수 있다.

## 삭제

```sql
DROP INDEX [인덱스 명]
```

```sql
DROP INDEX EX_INDEX;
```
인덱스는 조회성능을 극대화하기 위해 만든 객체인데 너무 많이 만들면 INSERT, DELETE, UPDATE시에 부하가 밣생해 전체적인 데이터베이스 성능을 저하한다.
- 안쓰는 인덱스는 삭제시키는 것이 좋다.
- 즉, 인덱스는 INSERT, DELETE, UPDATE의 성능을 희생하고 SELECT의 성능을 높이는 것이다.
- 인덱스를 하나 더 추가할지 말지는 데이터의 저장속도를 어디까지 희생할 수 있느냐? 그에 비해 읽기속도를 얼마나 개선할 수 있느냐? 의 타협점에서 생각해봐야 하는 것이다.
- 인덱스가 추가될 수록 데이터의 저장속도가 느려진다는 것은 당연히 CPU의 계산량이 많아진다는 것이 아니라 I/O 작업의 양이 많아진다는 것이다.
    - 그러므로 좋은 CPU와 RAM이 인덱스가 많아짐으로 인한 속도문제를 해결해 줄 수 있는것은 아니다.


## 인덱스 리빌드

인덱스 파일은 생성 후 INSERT, DELETE, UPDATE 등을 반복하다 보면 성능이 저하된다. 생성된 인덱스는 트리구조를 가지는데 삽입, 수정, 삭제등이 오랫동안 일어나다 보면 트리의 한쪽이 무거워져 전체적으로 트리의 깊이가 깊어진다. 트리가 치우치면 인덱스 검색속도가 느려지므로 주기적으로 리빌딩 작업을 거치는 것이 좋다.

```sql
ALTER INDEX [인덱스명] REBUILD;
```

```sql
ALTER INDEX EX_INDEX REBUILD;
```

## 공부하면 좋겠다
- https://12bme.tistory.com/138
