# SQL-FUNCTION
## FUNCTION
SQL에서 사용되는 함수에는 두 가지가 있다.
- 단일행 함수: 데이터를 한번에 하나씩만 처리하는 함수
- 복수행(그룹) 함수: 여러건의 데이터를 동시에 입력받아 결과값 하나를 만들어주는 함수

### DUAL TABLE
결과를 한 행으로 출력하기 위해 사용되는 더미 테이블이다. 산술 연산, 함수의 결과값 등을 출력하기 위해 많이 사용한다.
```sql
-- example
SELECT SYSDATE FROM DUAL:
```
DUAL TABLE은 더미컬럼 하나로 구성되어 있다.
## 단일행 함수
### 숫자함수
- `ABS(숫자)`: 절대값을 구하는 함수
- `FLOOR(숫자)`: 소수점 아래를 버리는 함수
- `ROUND(숫자)`, `ROUND(숫자, 자리수)`: 특정 자리수에서 반올림(자리수가 음수일 경우 일 > 십 > 백 > 천 ... 단위로)
- `TRUNC(숫자, 자리수)`: 특정 자릿수에서 버림연산하는 함수
- `MOD(피제수, 제수)`: 피제수를 제수로 나누었을 때 나머지를 구하는 함수 
### 문자처리 함수
- `UPPER(문자열)`: 대문자로 변환하는 함수
- `LOWER(문자열)`: 소문자로 변환하는 함수
- `INITCAP(문자열)`: 이니셜만 대문자로 변환하는 함수
- `LENGTH(문자열)`: 문자열 길이를 구하는 함수
- `LENGTHB(문자열)`: 문자열의 바이트 수를 알려주는 함수(DB CHARACTER SET 에 따라 다름)
- `INSTR(문자열, 문자)`: 문자열에서 특정 문자가 저장된 위치(INDEX)를 반환하는 함수(ORACLE의 인덱스는 0이 아니라 1부터임)
- `SUBSTR(문자열, 시작위치, 추출개수)`: 문자열에서 시작위치(INDEX)부터 추출개수 만큼의 문자열을 추출해내는 함수
- `SUBSTRB(문자열, 시작위치, 추출개수)`: `SUBSTR`과 동일하지만 바이트단위로 연산
- `LPAD(문자열, 길이, 채울문자)`: 길이만큼 자리를 마련하고 문자열을 오른쪽 정렬시킨 후 남는 자리에 채울문자를 채우는 함수
- `RPAD(문자열, 길이, 채울문자)`: 문자열을 왼쪽 정렬시킨후 `LPAD`와 동일
- `LTRIM(문자열)`: 문자열의 왼쪽 공백을 삭제하는 함수
- `RTRIM(문자열)`: 문자열의 오른쪽 공백을 삭제하는 함수
- `TRIM(문자 FROM 문자열)`: 문자열에서 특정 문자를 잘라내는 함수
    ```sql
    SELECT TRIM('a' FROM 'abcdefaa') FROM DUAL
    ```
### 날짜 함수
- 날짜 연산
    - 날짜 + 숫자: 그 날짜로부터 그 기간만큼 지난 날짜
    - 날짜 - 숫자: 그 날짜로부터 그 기간만큼 이전 날짜
    - 날짜 - 날짜: 두 날짜사이의 기간(기본값=일단위)

- `SYSDATE`: 현재 날짜를 반환하는 함수(초단위까지 반환)
    - `SYSDATE + 1`: 내일 날짜
    - `SYSDATE - 1`: 어제 날짜
- `MONTHS_BETWEEN(날짜1, 날짜2)`: 두 날짜 사이의 개월수를 구하는 함수(결과값이 소수로 나옴)
- `ADD_MONTHS(날짜, 개월수)`: 날짜에서 특정 개월수를 더한 날짜를 구하는 함수
- `NEXT_DAY(날짜, 요일)`: 해당 날짜에서 바로 다음 돌아오는 특정요일의 날짜를 구하는 함수
    ```sql
    SELECT NEXT_DAY(SYSDATE, '목요일') FROM DUAL
    ```
- `LAST_DAY(날짜)`: 해당 월의 마지막 날짜를 반환하는 함수
### 형 변환 함수
- `TO_CHAR(날짜, 출력형식)`: 날짜형이나 숫자형을 문자형으로 변환하는 함수
    ```sql
    SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD') FROM DUAL;
    ```
- `TO_DATE(문자열, 날짜형식)`: 숫자형이나 문자형을 날짜형으로 변환하는 함수
    ```sql
    SELECT TO_DATE('20200610', 'YYYYMMDD') FROM DUAL
    ```
- `TO_NUMBER(문자형, 숫자형식)`: 문자형을 숫자형으로 변환하는 함수
    ```sql
    SELECT TO_NUMBER('1230400', '9,999,999') FROM DUAL
    ```
### NULL 값을 다루는 함수
- `NVL(컬럼, 디폴트)`: 컬럼값이 NULL일 경우 디폴트 값으로 바꿔주는 함수 
### 선택을 위한 함수
- `DECODE(컬럼,조건1,값1,조건2,값2,...,아무조건도 아닐경우 값)`: 컬럼 값이 해당 조건과 같을 경우 해당하는 값을 출력하는 함수
    ```sql
    SELECT DECODE(ACCNT_NO, 10, '인사팀', 11, '개발팀', 12, '운영팀', '냐머지팀') AS DEPT FROM EMPLOYEES
    ```
### 조건에 따른 처리를 위한 함수
- `CASE WHEN 조건 THEN 값1 ... ELSE 값2 END`: 조건에 맞는 값을 뱉는 함수 
    ```sql
    SELECT CASE
           WHEN SCORE >= 90 THEN
           'A'
           WHEN SCORE >= 80 THEN
           'B'
           WHEN SCORE >= 70 THEN
           'C'
           ELSE
           'D'
           END              AS GRADE
           FROM STUDENTS
    ```

## 복수행(그룹) 함수
- 복수행(그룹) 함수의 특징 중 하나는 NULL값을 제외하고 연산한다는 것
- `SUM(컬럼)`: 해당 컬럼 값의 총합을 구하는 함수
    ```sql
    -- 모든 학생들의 점수 총합
    SELECT SUM(SCORE) FROM STUDENTS 
    ```
- `AVG(컬럼)`: 해당 컬럼 값의 평균을 구하는 함수
    ```sql
    -- 모든 학생들의 점수 평균
    SELECT AVG(SCORE) FROM STUDENTS
    ```
- `MAX(컬럼)`: 해당 컬럼 값 중 최대값
- `MIN(컬럼)`: 해당 컬럼 값 중 최소값
- `COUNT(컬럽)`: 해당 컬럼에 값이 있는 로우의 개수를 COUNT한다.
    ```sql
    -- 중복되지 않은 업무의 종류를 카운트
    SELECT COUNT(DISTINCT JOB) FROM EMP;
    ```
- `GROUP BY 컬럼1, 컬럼2,...`: 특정 컬럼을 기준으로 그룹화하여 테이블에 존재하는 행동을 그룹별로 구분하기 위해 사용. `GROUP BY`절을 사용할 떄는 그룹으로 묶은 컬럼 또는 그룹함수를 사용하여 `SELECT`하여야 한다. 그렇지 않을 경우 "GROUP BY 표현식이 아닙니다" 라는 오류를 뱉는다.
- `HAVING 조건`: `WHERE`절과 비슷하지만 `HAVING`은 `GROUP BY`절에 의해 생성된 결과 값 중 원하는 조건에 부합하는 자료만 보고싶을 때 사용한다. 그러므로 조건 또한 `SELECT`와 마찬가지로 `GROUP BY` 로 그룹지어진 컬럼 또는 그룹함수를 이용해 작성해야 한다.

- `ROLLUP`: 주어진 데이터들의 소계를 구해주는 함수
    ```sql
    SELECT DEPTNO, JOB, COUNT(*), SUM(SAL) FROM EMP GROUP BY ROLLUP(DEPTNO, JOB)
    ```

- `CUBE`: 주어진 데이터들의 소계와 총계를 구해주는 함수 
    ```sql
    SELECT DEPTNO, JOB, COUNT(*), SUM(SAL) FROM EMP GROUP BY CUBE(DEPTNO, JOB)
    ```