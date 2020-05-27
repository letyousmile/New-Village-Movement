# SP(Stored Procedure, 저장 프로시저)
- Transact-SQL 문장의 집합, 특정한 로직을 처리하고 결과 값을 반환하지 않는다.(함수와 결과값을 반환한다는 점이 다르다)

## 생성

```sql
CREATE OR REPLACE PROCEDURE MY_PROCEDURE
    ( P_STUDENT_ID    IN STUDENTS.STUDENT_ID%TYPE,
      P_STUDENT_NAME  IN STUDENTS.STUDENT_NAME%TYPE,
      P_STUDENT_MAJOR IN STUDENTS.STUDENT_MAJOR%TYPE)
IS
    st_score VARCHAR2;
BEGIN

    SELECT STUDENT_SCORE
    INTO st_score
    FROM STUDENTS
    WHERE STUDENT_ID = P_STUDENT_ID;

    IF st_score > 90 THEN
        UPDATE STUDENTS
        SET STUDENTS_GRADE = 'A',
            UPDDATE = SYSDATE
        WHERE STUDENT_ID = P_STUDENT_ID;
    
    ELSIF st_score > 80 THEN
        UPDATE STUDENTS
        SET STUDENTS_GRADE = 'B',
            UPDDATE = SYSDATE
        WHERE STUDENT_ID = P_STUDENT_ID;

    ELSIF st_score > 70 THEN
        UPDATE STUDENTS
        SET STUDENTS_GRADE = 'C',
            UPDDATE = SYSDATE
        WHERE STUDENT_ID = P_STUDENT_ID;

    ELSE
        UPDATE STUDENTS
        SET STUDENTS_GRADE = 'F',
            UPDDATE = SYSDATE
        WHERE STUDENT_ID = P_STUDENT_ID;
    END IF;

END;
```

## 실행

```sql
EXEC MY_PROCEDURE('102003', '김병철', '산업공학');
```

## 매개변수
### 입력값 매핑
```sql
EXEC MY_PROCEDURE(P_STUDENT_ID    => '102003',
                  P_STUDENT_NAME  => '김병철',
                  P_STUDENT_MAJOR => '산업공학' );
```

### 디폴트 값 설정
```sql
CREATE OR REPLACE PROCEDURE MY_PROCEDURE
    ( P_STUDENT_ID    IN STUDENTS.STUDENT_ID%TYPE := '102003',
      P_STUDENT_NAME  IN STUDENTS.STUDENT_NAME%TYPE := '김병철',
      P_STUDENT_MAJOR IN STUDENTS.STUDENT_MAJOR%TYPE := '산업공학')
--... 
```

### 매개변수의 종류

1. IN
    - 입력만가능, 변수에 입력받은 값을 할당하고 프로시저 내에서만 사용
    ```sql
    CREATE OR REPLACE PROCEDURE MY_PROCEDURE
    ( P_STUDENT_ID    IN STUDENTS.STUDENT_ID%TYPE,
      P_STUDENT_NAME  IN STUDENTS.STUDENT_NAME%TYPE,
      P_STUDENT_MAJOR IN STUDENTS.STUDENT_MAJOR%TYPE)
    ```
2. OUT
    - 출력만 가능, 변수에 입력받은 값을 할당하지 않고 프로시저를 빠져나온 후에도 해당 변수 사용가능
    - 예를들어, A 프로시저내에 B 프로시저가 있다고 치자. A프로시저에는 var라는 변수가 있고 B프로시저내에서 var 변수를 OUT 매개변수로 사용하고 var의 값을 변화시킨다면 B프로시저가 끝나는 순간 var변수의 값은 A프로시저 내에서도 변하게 된다.
    ```sql
    CREATE OR REPLACE PROCEDURE MY_PROCEDURE
    ( P_STUDENT_ID    OUT STUDENTS.STUDENT_ID%TYPE,
      P_STUDENT_NAME  OUT STUDENTS.STUDENT_NAME%TYPE,
      P_STUDENT_MAJOR OUT STUDENTS.STUDENT_MAJOR%TYPE)
    ```
3. IN OUT
    - 입, 출력 다 가능, 변수에 입력받은 값을 할당할 뿐 아니라 결과값을 외부에서도 사용할 수 있다.
    ```sql
    CREATE OR REPLACE PROCEDURE MY_PROCEDURE
    ( P_STUDENT_ID    IN OUT STUDENTS.STUDENT_ID%TYPE,
      P_STUDENT_NAME  IN OUT STUDENTS.STUDENT_NAME%TYPE,
      P_STUDENT_MAJOR IN OUT STUDENTS.STUDENT_MAJOR%TYPE)
    ```

## RETURN
- 함수와 다르게 프로시저에서는 RETURN 구문은 값을 반환하는 것이 아니라 프로시저를 빠져나가는 역할을 한다.
```sql
-- ...
IF st_score > 90 THEN
        UPDATE STUDENTS
        SET STUDENTS_GRADE = 'A',
            UPDDATE = SYSDATE
        WHERE STUDENT_ID = P_STUDENT_ID;
ELSE
    RETURN;
--...
```

## 장단점
### 장점
- 보안
    - 단위 실행 권한부여 가능
- 추상화
    - 로직을 숨기고 기능을 추상화함
- 네트워크 시간 절감
    - 서버에서 여러번 쿼리를 호출하는 것 보다 DB에서 호출하면 시간이 많이 단축됨
- 절차적 기능
    - IF, WHILE 같은 제어문 사용 가능
- 개발업무 구분
    - 개발과 DB를 구분지어서 진행할 수 있음
- 쿼리수정시 session을 날리지 않아도 됨(서버에서 session을 사용한다면)
    - db만 수정하면 되니까 재기동 할 필요가 없으므로
### 단점
- 낮은 처리성능
    - 숫자, 문자 연산을 많이 사용하면 오히려 java, c보다 느려짐
- 유지보수 어려움
    - 수정, 버전 관리등이 어려움(너무 보기 힘듬 ㅜ)
    - 너무 큰 로직이 하나의 프로시저에 들어가있다보니 리팩토링도 매우 힘들어보임(DBA가 붙어주지 않는다면)
- 재사용성 매우떨어짐
    - 큰 프로시저들은 비즈니스 로직을 안에 넣어놔서 재사용 할 수가 없음
    - 어떻게 개선할 수 있을까 // ORM을 써야하나?