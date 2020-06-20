# JOIN

## CARTESIAN PRODUCT
- CARTESIAN PRODUCT 는 두개 이상의 테이블이 조인될 때 WHERE절에 공동되는 컬럼에 의한 조인이 발생되지 않아 모든 데이터가 결과로 나타나는 경우를 말한다. 

이런 상황이 발생되지 않도록 WHERE절에 올바른 조인 조건을 지정해 주어야 한다.

## EQUI JOIN(등가 조인)
조인 대상이 되는 두 테이블에서 공통으로 존재하는 컬럼의 값이 일치되는 행을 연결하여 조인하는 기법

```sql
SELECT * FROM A, B WHERE A.id = B.id;
```

## NON-EQUI JOIN(비등가 조인)
동일 컬럼이 없이 다른 조건을 사용하여 조인하는 기법

```sql
SELECT A.ENAME, A.SAL, A.YMD, A.GRADE FROM EMP, SALGRAGE WHERE SAL BETWEEN LOSAL AND HISAL
```

## SELF JOIN
- 자기 자신과 조인하는 기법
- **(사용 상황)** 어떤 사원의 이름과 소속되 부서의 팀장을 같이 추출하고 싶을 때 팀장의 이름 또한 사원 테이블에서 가져와야 하므로 셀프조인을 한다.

## OUTER JOIN
- 조인 조건에 만족하지 않는 행도 나타내는 조인 기법

```sql
SELECT EMPLOYEE.ENAME||'의 매니저는 '|| MANAGER.ENAME||'입니다.' FROM EMP EMPLOYEE, EMP MANAGER WHERE EMPLOYEE.MGR = MANAGER.EMPNO(+);
```
위의 쿼리처럼 (+)를 사용하여 OUTER JOIN을 나타낸다 매니저가 일치하면 그 대상을 조인하고 그렇지 않더라도 해당 테이블((+)가 걸려있는)의 row를 가져오게 된다. 