# V$ACTIVE_SESSION_HISTORY

오라클은 현재 접속해서 활동중인 ACTIVE SESSION 정보를 1초에 한번씩 샘플링 해서 ASH 버퍼에 저장한다.

버퍼에 저장된 히스토리는 V$ACTIVE_SESSION_HISTORY 뷰를 통해서 조회할 수 있다. 이 뷰를 통해 정보가 찾아지지 않는다면 이미 AWR에 쓰여진 것이므로 DAB_HINT_ACTIVE_SESS_HISTORY 뷰를 조회한면 된다.

## 주요 이벤트 조회 방법

select
1. sample_id,sample_time,
    - 샘플링이 일어난 시간과 샘플 ID
2. session_id,session_serial#,user_id,xid,
    - 세션정보, User명, 트랜잭션 ID
3. sql_id,sql_child_number,sql_plan_hash_value,
    - 수행중 SQL정보
4. session_state,
    - 현재 세션의 상태 정보, 'ON CPU' 또는 'WAITING'
5. qc_instance_id, qc_session_id,
    - 병렬 Slave 세션일때 쿼리 코디네이터 정보를 찾을 수 있게 함
6. blocking_session, blocking_session_serial# blocking_session_status,
    - 현재 세션의 진행을 막고 있는 블로킹 세션 정보
7. event,event#,seq#,wait_class,wait_time,time_waited,
    - 현재 발생 중인 대기 이벤트 정보
8. p1text,p1,p2text,p2,p3text,p3,
    - 현재 발생 중인 대기 이벤트의 파라미터 정보
9. current_obj#,current_file#,current_block#,
    - 해당 세션이 현재 참조하고 있는 오브젝트 정보. v$session뷰에 있는 row-wait_obj#,row_wait_file#,row_wait_block#컬럼을 가져온 것임
10. program,module,action,client_id
    - 애플리케이션 정보

from v$active_session_history;


 