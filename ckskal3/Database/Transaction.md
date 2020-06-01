## Transaction

> 트랜잭션이란 데이터베이스의 상태를 변화시키기 위해 수행하는 작업들의 묶음
>
> 트랜잭션은 이론상 ACID원칙을 보장해야 한다. ACID 란 Atomicity(원자성), Consistency(일관성), Isolation(독립성), Durability(영구성)를 뜻한다.
> 
> #### ACID
>
> * Atomicity
> 
>   * 모든 트랜잭션은 완전히 실행되거나, 아예 실행하지 않아야 한다. 즉 도중에 중단된 상태로 존재하는 트랜잭션은 존재할 수 없다.
>
> * Consistency
>
>   * 같은 DB에 트랜잭션을 실행한 결과는 언제난 같아야 한다.
>
> * Isolation
>
>   * 모든 트랜잭션은 다른 트랜잭션에 의해 영향을 받으면 안된다. 
>
> * Durability
>
>   * 성공한 트랜잭션의 결과는 보존이 되어야한다. 즉 오류로 인해 서버를 복구할떄 성공했던 트랜잭션의 결과는 보존되어야 한다.
>
>  
> ### Transaction Isolation
>
> 동시에 여러 트랜잭션을 처리할때, 어느 한 트랜잭션이 다른 트랜잭션에서 조회, 수정을 가능하게 허용하는 수준, 트랜잭션 고립화 수준은 4가지로 나눌 수 있다. 위에서 아래로 갈수록 수준이 높아지며 동시성이 떨어진다.
>
> #### InnoDB의 Lock
>
> [여기](https://suhwan.dev/2019/06/09/transaction-isolation-level-and-lock/)를 더욱 자세히 공부 할 수 있다.
>
> * s_lock
>   * read 작업에 대해 lock을 검
> * x_lock
>   * 데이터를 변경하는 작업에 대해 lock을 검
>
> #### Transaction Isolation Level
> 
> * READ-UNCOMMITTED
>   * 정합성에 문제가 있기에 거의 사용하지 않는다.
>   * 한 트랜잭션에서 insert, delete, update 에 대해 commit 또는 rollback을 하지 않은 상태에서도 다른 트랜잭션이 변경된 값을 볼 수 있다.
>   * Dirty read, Non-repeatable read, Phantom read 발생한다.
> * READ-COMMITED
>   * Oracle의 기본 고립수준
>   * 데이터를 변경한 트랜잭션에서 commit 전에는 UNDO 영역에서 데이터를 백업해두고 값을 가져온다.
>   * 데이터를 변경한 트랜잭션에서 commit을 하면 변경된 값을 보여준다.
>   * Non-repeatable read, Phantom read 발생한다.
>
> * REPEATABLE-READ
>   * MySQL의 기본 고립수준
>   * UNDO 영역에서 데이터를 백업해두고 값을 가져온다.
>   * UNDO 영역에 데이터가 쌓이면 서버의 처리 성능이 떨어지기에 불필요한 데이터는 주기적으로 삭제한다.
>   * Phantom Read 발생한다.
> * SERIALIZABLE
>   * 가장 높은 고립수준으로 동시 처리 성능이 낮다.
>   * Phantom Read 가 발생하지 않지만 DB에서 거의 사용하지 않는다.
>
> #### 고립수준에서 발생하는 문제 3가지
> 
> * DIrty Read
>   * 커밋되지 않은 수정 중인 데이터가 다른 트랜잭션에 읽히는 경우 
> * Non-Repeatable Read
>   * 한 트랜잭션에서 조회를 두번 했을때 그 사이에 다른 트랜잭션에 의해 수정/삭제 되어 값이 다르게 나오는 경우
> * Phantom Read
>   * 한 트랜잭션에서 조회를 두번 했을때, 첫 번째 조회때는 없었던 데이터가 두번째 조회떄는 나타나는 경우
>   * 트랜잭션 도중 삽입을 허용하면서 생기는 문제
>   * 고립화 수준을 3단계까지 올려야 하지만 동시성이 너무 떨어지게 된다.
> 
> #### Mysql에서 transaction isolation level 확인 방법
> 
> ```mysql
> show variables like 'transaction_isolation'; // MYSQL8
> show variables like 'tx_isolation';
> ```
> 
> #### transaction isolation level 변경 방법
> 
> ````mysql
> set transaction_isolation='read-committed';
> set tx_isolation='read-committed';
> 
> commit; 반드시 한번 해줄것
> ````
> 
> MySQL의 스토리지 엔진인 MyISAM 과 InnoDB도 공부해보면 좋을 것 같다.