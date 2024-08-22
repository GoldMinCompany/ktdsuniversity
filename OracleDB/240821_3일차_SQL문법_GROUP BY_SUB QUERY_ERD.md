# 집계함수

* 개수 세기 COUNT(COLUMN_NAME)
  - 행의 수
```sql
SELECT COUNT(EMPLOYEE_ID)
  FROM EMPLOYEES
;
```

위 코드는 실무에서 사용하지 않는다. Primary Key로 개수를 세는 방법은 사용하지 않는다. 

```sql
SELECT COUNT(1)
  FROM EMPLOYEES
;
```
1은 특별한 의미를 갖지 않고 의미 없는 숫자를 사용하여 특정 열의 행의 개수를 센다.

* MAX
  - Column의 값 중 최댓값을 리턴하는 집계함수이다.
 
* MIN
  - Column의 값 중 최솟값을 리턴하는 집계함수이다.
 
* AVG
  - Column의 값의 평균을 리턴하는 집계함수이다.
  - AVG 집계함수는 Number type만 연산이 가능하다.
 
* SUM
  - Column의 값의 총합을 리턴하는 집계함수이다.
  - AVG 집계함수와 마찬가지로 Number type만 연산이 가능하다.

# GROUP BY

* GROUP BY가 포함된 Query 문에서는 '*'를 사용할 수 없다.
* GROUP BY는 하나의 정보를 축약 해 놓은 것이 집계이기 때문에 집계함수와 Column은 동시에 사용할 수 없다
* 반드시 GROUP BY를 입력해야 한다.
* GROUP BY는 Table을 Column의 기준에 따라서 임시 Table로 그룹화한 것이다.
* Table에 굉장히 많은 Row가 있을 때, GROUP BY를 사용할 경우 성능이 현저히 낮아지기 때문에 GROUP BY는 ORDER BY와 더불어 사용하지 않는다. 

```sql
SELECT MAX(SALARY)
     , MIN(SALARY)
     , DEPARTMENT_ID
  FROM EMPLOYEES
 GROUP BY DEPARTMENT_ID
 ORDER BY DEPARTMENT_ID ASC
 ;
```

# Sub Query
* SELECT 문 안의 SELECT 문
* 조회하려는 대상을 정확히 알기 못하거나 조회하려는 대상이 하나 이상일 경우 사용한다.
* SELECT (SELECT ... FROM) -> Scala query
* FROM (SELECT ... FROM) -> Inline view
* WHERE COLUMNS_NAME = (SELECT ... FROM) -> Sub query
```sql
SELECT *
  FROM TABLE
 WHERE COLUMNS_NAME = (SELECT COLUMNS_NAME
                         FROM TABLE
                        WHERE CONDITION)
```

```sql
SELECT *
  FROM EMPLOYEES
 WHERE COMMISSION_PCT = (SELECT MAX(COMMISSION_PCT)
                           FROM EMPLOYEES)
;
```
```sql
SELECT SALARY
     , COMMISSION_PCT
  FROM EMPLOYEES
 WHERE COMMISSION_PCT = (SELECT MIN(COMMISSION_PCT)
                           FROM EMPLOYEES)
;
```

# HAVING
* GROUP BY된 결과를 집계함수로 Filtering 할 수 있다.
* HAVING을 통해 GROUP BY 통해 그룹화된 Data를 Filtering 할 수 있다.

```sql
SELECT MANAGER_ID
     , COUNT(1)
  FROM EMPLOYEES
 GROUP BY MANAGER_ID
HAVING COUNT(1) >= 3
;
```
# Table join
* Table 마다 Primary key가 있고 PK를 참조하는 키를 Foreign key라 한다.
* Parimary key와 Foreign key 연결하여 관련된 정보를 조회한다. 
* 예를들어 DEPARTMENT Table의 PK인 DEPARTMENT_ID는 EMPLOYEES Table의 FK인 DEPARTMENT_ID와 같다.
* 이러한 연결상태를 JOIN이라 하고 연결 관계를 생성할 수 있다.

# JOIN 관계에 있는 데이터는 여러가지 형태의 관계가 형성된다.
* 1:1
  - 한 테이블의 Primary key가 다른 테이블의 Foreign key에 하나만 존재하는 관계
  - 만들어서는 안되는 관계이다. 단, 컬럼의 수가 40개 이상 넘어갈 경우 테이블을 분리하여 1:1관계를 맺는다.

* 1:N
  - 한 테이블의 Primary key가 다른 테이블의 Foreign key에 여러 개가 존재하는 관계
  - 가장 흔하게 만들게 되는 관계

* N:N
  - 물리적으로 존재할 수 없는 관계
 
# PK, FK 제약조건의 역할과 위배되는 케이스
* Foreign key로 다른 테이블의 Primary key를 참조할 때, Primary key값에 정의되어 있는 값만 사용가능하다. 
* Primary key와 Foreign key는 테이블의 필수 요소로 테이블은 이 둘 중 하나 이상은 반드시 포함해야한다.

# ER-D(Entity Relationship Diagram, 객체 관계도)
* 식별 관계
  - Table A의 Primary key가 다른 테이블의 Foreign key로 참조가 되고 1:N 관계가 성립하며 Foreign key가 Primary key가 된다. PFK 표현하고 실선으로 표시한다.
* 비식별 관계
  - Table A의 Primary key가 다른 테이블의 Foreign key로 참조된다. 점섬으로 표시된다.


# 멀티키
* Primary key가 두개인 경우 멀티키라고 한다.
* 멀티키의 특징은 데이터 쌍 전체가 하나의 Primary key가 된다. 

# SQL 실습
```sql
-- 1. 현재 시간을 조회한다.
SELECT SYSDATE
  FROM DUAL -- DUMMY TABLE (row가 하나밖에 없는 의미 없는 TABLE)
;
-- 2. 현재 시간을 "연-월-일" 포멧으로 조회한다.
SELECT TO_CHAR(SYSDATE) --BUG (이렇게 사용하면 안됨 - 실제 웹서버에서는 해당 SQL은 에러를 일으킨다.)
     , TO_CHAR(SYSDATE, 'YYYY-MM-DD') -- 정상 
  FROM DUAL
;
-- 3. 한 시간 전 시간을 "시:분:초" 포멧으로 조회한다.

SELECT TO_CHAR(SYSDATE, 'HH:MI:SS') -- 문자열은 작은 따옴표를 이용한다. 12시 베이스
	 , TO_CHAR(SYSDATE, 'HH24:MI:SS') -- 24시 베이스
  FROM DUAL
;
-- 4. EMPLOYEES 테이블의 모든 정보를 조회한다.
SELECT *
  FROM EMPLOYEES
;
-- 4.1 EMPLOYEES TABLE에서 FRIST_NAME으로 오름 차순 정렬하기.
SELECT *
  FROM EMPLOYEES
 ORDER BY FIRST_NAME ASC
;
-- 4.2 EMPLOYEES TALBE에서 LAST_NAME으로 내림 차순 정렬하기.
SELECT *
  FROM EMPLOYEES
 ORDER BY LAST_NAME DESC 
;
-- 4.3 EMPLOYEES TALBE에서 FIRST_NAME으로 오름 차순 정렬하고
-- LAST_NAME으로 내림 차순 정리
-- FIRST_NAME으로 오름 차순 정렬된 상태에서, LAST_NAME만 내림차순 정렬 수행
SELECT *
  FROM EMPLOYEES
 ORDER BY FIRST_NAME ASC -- 1차 정렬
     , LAST_NAME DESC -- 2차 정렬
;
-- ORDER BY 주의점
-- 데이터의 수가 굉장히 많은 테이블에서 ORDER BY를 수행하면 성능이 기하급수적으로 느려진다.
-- 실무에서는 가급적이면 정렬을 사용하지 않고 정말 필요할 때 정렬을 수행한다. 


-- 5. DEPARTMENTS 테이블의 모든 정보를 조회한다.
SELECT *
  FROM DEPARTMENTS
;
-- 6. JOBS 테이블의 모든 정보를 조회한다.
SELECT *
  FROM JOBS
;
-- 7. LOCATIONS 테이블의 모든 정보를 조회한다.
SELECT *
  FROM LOCATIONS
;
-- 8. COUNTRIES 테이블의 모든 정보를 조회한다.
SELECT *
  FROM COUNTRIES
;
-- 9. REGIONS 테이블의 모든 정보를 조회한다.
SELECT *
  FROM REGIONS
;
-- 10. JOB_HISTORY 테이블의 모든 정보를 조회한다.
SELECT *
  FROM JOB_HISTORY
;
-- 11. 90번 부서에서 근무하는 사원들의 모든 정보를 조회한다.
-- 11. EMPLOYEES TABLE에서 DEPARTMENT_ID가 90인 사원의 모든 컬럼을 조회한다. 
SELECT *
  FROM EMPLOYEES
 WHERE DEPARTMENT_ID = 90
;
-- 12. 90번, 100번 부서에서 근무하는 사원들의 모든 정보를 조회한다.
-- 12. EMPLOYEES TABLE에서 DEPARTMENT_ID가 90, 100인 사원의 모든 컬럼을 조회한다. 
SELECT *
 FROM EMPLOYEES
WHERE DEPARTMENT_ID = 90
   OR DEPARTMENT_ID = 100
;
-- 13. 100번 상사의 직속 부하직원의 모든 정보를 조회한다.
-- 13. EMPLOYEES TALBE에서 MANAGER_ID가 100인 모든 컬럼을 조회
SELECT *
  FROM EMPLOYEES
 WHERE MANAGER_ID = 100
;

-- 14. 직무 아이디가 AD_VP 인 사원의 모든 정보를 조회한다.
-- 14. EMPLOYEES TALBE에서 JOB_ID가 'AD_VP'인 모든 컬럼을 조회
SELECT *
  FROM EMPLOYEES
 WHERE JOB_ID = 'AD_VP'
;

-- 15. 연봉이 7000 이상인 사원의 모든 정보를 조회한다.
-- 15. EMPLOYEES TALBE에서 SALARY가 7000보다 큰 모든 컬럼을 조회 
SELECT *
  FROM EMPLOYEES
 WHERE SALARY >= 7000
;

-- 15-1 
-- EMPLOYEES TABLE에서 SALARY가 7000이상이고 JOB_ID가 'IT_PROG'이며
-- DEPARTMENT_ID가 60인 모든 컬럼 조회한다.
-- SALARY를 기준으로 내림차순 정렬한다.
-- ORDER BY는 SQL의 반드시 마지막에 작성한다. 

SELECT *
  FROM EMPLOYEES
 WHERE SALARY >= 7000
   AND JOB_ID = 'IT_PROG'
   AND DEPARTMENT_ID = 60
 ORDER BY SALARY DESC
;
-- 15-2
-- DEPARTMENTS(부서정보) TABLE에서 DEPARTMENT_ID(부서번호)가 60인
-- DEPARTMENT_NAME만 조회한다.

SELECT DEPARTMENT_NAME 
  FROM DEPARTMENTS
 WHERE DEPARTMENT_ID = 60
;
-- 16. 2005년 09월에 입사한 사원들의 모든 정보를 조회한다.
/*
 * 숫자 타입의 컬럼 : 숫자 비교
 * 문자 타입의 컬럼 : 문자 비교
 * 날짜 타입의 컬럼 : 날짜 비교
 * 
 * 날짜를 문자로 변환 TO_CHAR(날짜 타입의 컬럼, '포멧')
 * 문자를 날짜로 변환 TO_DATE(문자 타입의 컬럼 또는 값, '포멧')
 * 
 * 2005년 9월 1일 부터 2005년 9월 30일 사이에 입사한 사람들의 정보를 조회
 * BETWEEN A AND B
 * BETWEEN TO_DATE() AND TO_DATE()
 */

SELECT SYSDATE 
     , TO_DATE('2005-09-01','YYYY-MM-DD')
  FROM DUAL
;

SELECT *
  FROM EMPLOYEES
 WHERE HIRE_DATE BETWEEN TO_DATE('2005/09/01','YYYY-MM-DD') AND TO_DATE('2005-09-30','YYYY-MM-DD')
 ORDER BY HIRE_DATE
 ;

-- 17. 111번 사원의 모든 정보를 조회한다.
SELECT *
  FROM EMPLOYEES
 WHERE EMPLOYEE_ID = 111
;

-- 18. 인센티브를 안받는 사원들의 모든 정보를 조회한다.
-- 18. EMPLOYEES TALBE에서 COMMISSION_PCT값이 NULL 모든 컬럼을 조회한다. 
SELECT *
  FROM EMPLOYEES
 WHERE COMMISSION_PCT IS NULL
;

/*
 * 실습 문제 
 * 1. employees table에서 job_id가 SA_MAN 이거나, SA_REP 이거나, IT_PROG인 데이터 중
 *    salary가 10,000 이상인 data 중
 *    management_id가 100인 data의 first_name, last_name, employees_id 조회 
 */

SELECT FIRST_NAME, LAST_NAME, EMPLOYEE_ID, SALARY, MANAGER_ID
  FROM EMPLOYEES 
 WHERE (JOB_ID = 'SA_MAN'
    OR JOB_ID = 'SA_REP'
    OR JOB_ID = 'IT_PROG')
   AND SALARY >= 10000
   AND MANAGER_ID = 100
;
-- IN 연산자 주의점
-- 1. IN에 들어간 값의 최대 개수는 1000개이다. 비교할 값의 개수가 명확할 때 사용한다.
-- 2. IN에 들어간 값 중 NULL은 연산이 불가능하다. NULL 은 IS NULL 또는 IS NOT NULL로만 비교가 가능하다.
SELECT FIRST_NAME, LAST_NAME, EMPLOYEE_ID, SALARY, MANAGER_ID
  FROM EMPLOYEES 
 WHERE JOB_ID IN ('SA_MAN', 'SA_REP', 'IT_PROG')
   AND SALARY >= 10000
   AND MANAGER_ID = 100
;
/**
 * 2. employees table에서 department_id가 10~60 사이인 데이터 중
 *    salary가 10,000 미만 데이터를 salary 기준으로 내림차순 정렬하여 모든 컬럼 조회
 */
SELECT *
  FROM EMPLOYEES
 WHERE DEPARTMENT_ID >= 10
   AND DEPARTMENT_ID <= 60
   AND SALARY < 10000
 ORDER BY SALARY DESC
 ;

SELECT *
  FROM EMPLOYEES
 WHERE DEPARTMENT_ID IN (10, 20, 30, 40, 50, 60)
   AND SALARY < 10000
 ORDER BY SALARY DESC
 ;

SELECT *
  FROM EMPLOYEES
 WHERE DEPARTMENT_ID BETWEEN 10 AND 60 -- BETWEEN 기간 검색할 때 주로 사용한다.
   AND SALARY < 10000
 ORDER BY SALARY DESC
 ;

/*
 * 3. employees table에서 salary가 11,000 이상인 데이터 중에서 
 *    commission_pct가 0.2이상인 데이터를 commission_pct를 기준으로 오름차순 정렬
 *    salary를 기준으로 내림차순 정렬하여 모든 컬럼 조회 
 */

SELECT *
  FROM EMPLOYEES
 WHERE SALARY >= 11000
   AND COMMISSION_PCT >= 0.2
 ORDER BY COMMISSION_PCT ASC
     , SALARY DESC
;
-- ORDER BY의 정렬 방법이 ASC이라면, 생략가능하다. 


-- 19. 인센티브를 받는 사원들의 모든 정보를 조회한다.
-- 19. EMPLOYEES TABLE에서 COMMISSION_PCT값이 NULL 아닌 모든 컬럼을 조회한다. 
SELECT *
  FROM EMPLOYEES
 WHERE COMMISSION_PCT IS NOT NULL
;



-- 20. 이름의 첫 글자가 'D' 인 사원들의 모든 정보를 조회한다.
-- 20. EMPLOYEES TABLE에서 FIRST_NAME의 값이 D로 시작하는 데이터의 모든 컬럼을 조회한다. 
/*
 * 포함, 시작, 끝 검색 -> % 사용
 * COLUMN LIKE 'D%' --> 컬럼의 값이 D로 시작합니다. 
 * COLUMN LIKE '%D' --> 컬럼의 값이 D로 끝납니다. 
 * COLUMN LIKE '%D%' --> 컬럼의 값이 D로 시작하거나, D로 끝나거나, 중간에 D가 존재한다. 
 */
SELECT *
  FROM EMPLOYEES
 WHERE FIRST_NAME LIKE 'D%'
;

-- 21. 성의 마지막 글자가 'a' 인 사원들의 모든 정보를 조회한다.
-- 21. EMPLOYEES TABLE에서 LAST_NAME의 값이 a로 끝나는 모든 컬럼을 조회한다.

SELECT *
  FROM EMPLOYEES
 WHERE LAST_NAME LIKE '%a'
;

-- 22. 전화번호에 '.124.'이 포함된 사원들의 모든 정보를 조회한다.
-- 22. EMPLOYEES TABLE에서 PHONE_NUMBER의 값에 .124.이 포함된 모든 컬럼을 조회한다. 

SELECT *
  FROM EMPLOYEES
 WHERE PHONE_NUMBER LIKE '%.124.%'
;

/*
 * 실습문제 
 * 4. EMPLOYEES TABLE에서 FIRST_NAME 또는 LAST_NAME에 'e' 또는 'E'가 포함된 데이터의 모든 컬럼을 조회한다.
 * COLUMN NAME LIKE '%A' -> COLUMN에 대문자 A가 포함되어 있는가?
 * COLUMN NAME NOT LIKE '%A' -> COLUMN에 대문자 A가 포함되어 있지 않은가?
 * COLUMN NAME IN(10, 20, 30) -> COLUMN값이 10, 20, 30 중 하나인가?
 * COLUMN NAME NOT IN(10, 20, 30) -> COLUMN값이 10, 20, 30 모두 아닌가?
 * COLUMN NAME BETWEEN A AND B -> COLUMN값이 A와 B 사이인가?
 * COLUMN NAME NOT BETWEEN A AND B -> COLUMN값이 A와 B 사이아닌가?
 *
 **/
SELECT *
  FROM EMPLOYEES
 WHERE FIRST_NAME LIKE '%e%'
    OR FIRST_NAME LIKE '%E%'
    OR LAST_NAME LIKE '%e%'
    OR LAST_NAME LIKE '%E%'
;


 -- 20240821
 -- 집계함수 실습
 -- 1. 갯수 세기 - count(column) employees table에 몇개의 row가 있는가? 우리 회사의 사원이 몇명인가? 

SELECT COUNT(EMPLOYEE_ID) -- 실무에서 정말 이렇게 갯수를 세는가? PK로 갯수를 세는 방법은 사용하지 않는다. 
  FROM EMPLOYEES
;

SELECT *
  FROM DUAL
;

SELECT SYSDATE -- TABLE의 ROW 갯수만큼 결과가 나온다.
  FROM EMPLOYEES
;

SELECT COUNT(1) -- 1은 특별한 의미를 갖지 않는다. 의미 없는 숫자를 이용해서 숫자를 센다 
  FROM EMPLOYEES
;

-- EMPLOYEES TABLE에서 DEPARTMENT_ID가 90인것은 몇 개 있는가?

SELECT COUNT(1)
  FROM EMPLOYEES
 WHERE DEPARTMENT_ID = 90
;


SELECT COUNT(EMPLOYEE_ID)
  FROM EMPLOYEES
 WHERE DEPARTMENT_ID = 90
;

 -- 2. 최대 연봉
SELECT MAX(SALARY)
  FROM EMPLOYEES
;

 -- 3. 가장 늦게 입사한 날짜
SELECT MAX(HIRE_DATE)
  FROM EMPLOYEES
 ;

 -- 4. 최소 연봉

SELECT MIN(SALARY)
  FROM EMPLOYEES
;

 -- 5. 가장 일찍 입사한 날짜

SELECT MIN(HIRE_DATE)
  FROM EMPLOYEES
;

 -- 6. 평균 연봉 
SELECT AVG(SALARY) -- AVG는 NUMBER TYPE만 연산 가능!
  FROM EMPLOYEES
;

 -- 7. 연봉 총합

SELECT SUM(SALARY) -- AVG마찬가지로 SUM 함수도 NUMBER TYPE만 연산 가능하다. 
  FROM EMPLOYEES
;

-- 8. 갯수, 최대연봉, 최소연봉, 가장늦게 입사한날짜, 가장 일찍 입사한날짜, 평균연봉, 연봉 총합
 
SELECT COUNT(1)
     , MAX(SALARY)
     , MIN(SALARY)
     , MAX(HIRE_DATE)
     , MIN(HIRE_DATE)
     , AVG(SALARY)
     , SUM(SALARY)
  FROM EMPLOYEES
;

-- EMPLOYEES TALBE에서 DEPARTMENT_ID 별로 최대 연봉과 최저 연봉을 조회한다. 
-- 조회하는 컬럼
-- 1. DEPARTMENT_ID
-- 2. 최대 연봉
-- 3. 최저 연봉
-- GROUP BY가 포함된 QUERY에서는 *를 쓸 수 없다.
-- 집계 함수와 COLUMN은 동시에 쓸 수 없다.
-- 하나의 정보를 축약 해 놓은 것이 집계이기 때문에 집계 함수와 COLUMN은 동시에 쓸 수 없다
-- 반드시 GROUP BY를 입력해야한다. 
SELECT MAX(SALARY)
     , MIN(SALARY)
     , DEPARTMENT_ID
  FROM EMPLOYEES
 GROUP BY DEPARTMENT_ID
 ORDER BY DEPARTMENT_ID ASC
 ;

-- EMPLOYEES TABLE에서 DEPARTMENT_ID + JOB_ID 별로 평균 연봉을 알고 싶다
-- 조회해야 하는 컬럼
-- 1. DEPARTMENT_ID
-- 2. JOB_ID
-- 3. 평균 연봉

SELECT JOB_ID
     , DEPARTMENT_ID
     , AVG(SALARY)
  FROM EMPLOYEES
 GROUP BY DEPARTMENT_ID
     , JOB_ID
 ORDER BY DEPARTMENT_ID ASC
 
 -- GROUP BY 주의점
 /**
  * GROUP BY는 TABLE을 기준을 따라서 임시 TABLE로 그룹화 
  * TABLE에 굉장히 많은 ROW가 있을 때, GROUP BY를 사용할 경우 성능이 무지막지 하게 낮아진다.
  * GROUP BY는 사용하지 않는다.  
  */


-- 23. 직무 아이디가 'PU_CLERK'인 사원 중 연봉이 3000 이상인 사원들의 모든 정보를 조회한다.

-- 24. 평균 연봉보다 많이 받는 사원들의 사원번호, 이름, 성, 연봉을 조회한다.
-- EMPLOYESS TABLE에서 EMPLOYEE_ID, FIRST_NAME, LAST_NAME 조회
-- 조건 : salary > 평균연봉
-- 평균연봉이 얼마인지 모른다.
-- 먼저 할것 -> 얼마인지 모르는 평균연봉을 먼저 구하자. 

SELECT AVG(SALARY)
  FROM EMPLOYEES
;

SELECT FIRST_NAME
     , LAST_NAME
     , SALARY
  FROM EMPLOYEES
 WHERE SALARY > (SELECT AVG(SALARY)
                   FROM EMPLOYEES)
-- 25. 평균 연봉보다 적게 받는 사원들의 사원번호, 연봉, 부서번호를 조회한다.
                   
SELECT EMPLOYEE_ID
     , SALARY
     , DEPARTMENT_ID
  FROM EMPLOYEES
 WHERE SALARY < (SELECT AVG(SALARY)
                   FROM EMPLOYEES)
;
-- 26. 가장 많은 연봉을 받는 사원의 사원번호, 이름, 연봉을 조회한다.

SELECT EMPLOYEE_ID
     , FIRST_NAME
     , LAST_NAME
     , SALARY
  FROM EMPLOYEES
 WHERE SALARY = (SELECT MAX(SALARY)
                   FROM EMPLOYEES)
;



-- 27. 이름이 4글자인 사원의 모든 정보를 조회한다.
-- EMPLOYEES TABLE에서 FIRST_NAME의 길이가 4글자인 데이터의 모든 컬럼을 조회한다. 
-- LIKE 이용 글자의 수를 체크하는 와일드 카드 : _ (언더바)
/**
 * COL LIKE '__' -> COL의 값이 두 글자인가?
 * COL LIKE '_____' -> COL의 값이 다섯 글자인가?
 * 
 * COL LIKE 'A__%' -> COL의 값이 A로 시작해서 두글자 이상이 더 있는가 ?
 */

SELECT *
  FROM EMPLOYEES
 WHERE FIRST_NAME LIKE '____'
;


-- 28. 'SA_REP' 직무인 직원 중 가장 높은 연봉과 가장 낮은 연봉을 조회한다.
-- 28. 직무아이디 == JOB_ID

SELECT MAX(SALARY)
     , MIN(SALARY)
  FROM EMPLOYEES
 WHERE JOB_ID = 'SA_REP'
 ;

-- 29. 직원의 입사일자를 '연-월-일' 형태로 조회한다.
-- 29. HIRE_DATE date type을 문자 type으로 !

SELECT TO_CHAR(HIRE_DATE, 'YYYY-MM-DD')
  FROM EMPLOYEES
;

-- 30. 가장 늦게 입사한 사원의 모든 정보를 조회한다.
SELECT *
  FROM EMPLOYEES
 WHERE HIRE_DATE = (SELECT MAX(HIRE_DATE)
                      FROM EMPLOYEES)
;
-- 31. 가장 일찍 입사한 사원의 모든 정보를 조회한다.
SELECT *
  FROM EMPLOYEES
 WHERE HIRE_DATE = (SELECT MIN(HIRE_DATE)
                      FROM EMPLOYEES)
;
                      
-- 32. 자신의 상사보다 더 많은 연봉을 받는 사원의 모든 정보를 조회한다.
-- 33. 자신의 상사보다 더 일찍 입사한 사원의 모든 정보를 조회한다.

-- 34. 부서아이디별 평균 연봉을 조회한다.
-- EMPLOYEES TABLE에서 DEPARTMENT_ID 별로 평균 연봉을 조회한다. 

SELECT DEPARTMENT_ID
     , AVG(SALARY)
  FROM EMPLOYEES
 GROUP BY DEPARTMENT_ID
 ORDER BY DEPARTMENT_ID ASC
;

-- 35. 직무아이디별 평균 연봉, 최고연봉, 최저연봉을 조회한다.
-- EMPLOYEES TABLE에서 JOB_ID 별로 평균 연봉, 최고 연봉, 최저 연봉을 조회한다. 

SELECT JOB_ID
     , AVG(SALARY)
     , MAX(SALARY)
     , MIN(SALARY)
  FROM EMPLOYEES
 GROUP BY JOB_ID 
;

-- 36. 가장 많은 인센티브를 받는 사원의 모든 정보를 조회한다.
SELECT *
  FROM EMPLOYEES
 WHERE COMMISSION_PCT = (SELECT MAX(COMMISSION_PCT)
                           FROM EMPLOYEES)
;
-- 37. 가장 적은 인센티브를 받는 사원의 연봉과 인센티브를 조회한다.
SELECT SALARY
     , COMMISSION_PCT
  FROM EMPLOYEES
 WHERE COMMISSION_PCT = (SELECT MIN(COMMISSION_PCT)
                           FROM EMPLOYEES)
;

-- 38. 직무아이디별 사원의 수를 조회한다.
-- EMPLOYEES TABLE에서 JOB_ID 별로 ROW의 수를 조회한다. 

SELECT JOB_ID 
     , COUNT(1)
  FROM EMPLOYEES
 GROUP BY JOB_ID
;

-- 39. 상사아이디별 부하직원의 수를 조회한다. 단, 부하직원이 2명 이하인 경우는 제외한다.
-- EMPLOYEES TABLE에서 MANAGER_ID로 ROW의 수를 조회한다. 이 때, ROW의 수가 2이하인 경우는 제외시킨다.
-- HAVING 그룹바이를 통해 쪼개진 데이터를 필터링해주는 쿼리문이다. 

SELECT MANAGER_ID
     , COUNT(1)
  FROM EMPLOYEES
 GROUP BY MANAGER_ID
HAVING COUNT(1) >= 3
;

-- 40. 사원이 속한 부서의 평균연봉보다 적게 받는 사원의 모든 정보를 조회한다.
-- 41. 사원이 근무하는 부서명, 이름, 성을 조회한다.
-- 42. 가장 적은 연봉을 받는 사원의 부서명, 이름, 성, 연봉, 부서장사원번호를 조회한다.
-- 43. 상사사원번호를 중복없이 조회한다.







-- 44. 50번 부서의 부서장의 이름, 성, 연봉을 조회한다.
-- 45. 부서명별 사원의 수를 조회한다.
-- 46. 사원의 수가 가장 많은 부서명, 사원의 수를 조회한다.
-- 47. 사원이 없는 부서명을 조회한다.
-- 48. 직무가 변경된 사원의 모든 정보를 조회한다.
-- 49. 직무가 변경된적 없는 사원의 모든 정보를 조회한다.
-- 50. 직무가 변경된 사원의 과거 직무명과 현재 직무명을 조회한다.
-- 51. 직무가 가장 많이 변경된 부서의 이름을 조회한다.
-- 52. 'Seattle' 에서 근무중인 사원의 이름, 성, 연봉, 부서명 을 조회한다.
-- 53. 'Seattle' 에서 근무하지 않는 모든 사원의 이름, 성, 연봉, 부서명, 도시를 조회한다.
-- 54. 근무중인 사원이 가장 많은 도시와 사원의 수를 조회한다.  
-- 55. 근무중인 사원이 없는 도시를 조회한다.
-- 56. 연봉이 7000 에서 12000 사이인 사원이 근무중인 도시를 조회한다.
-- 57. 'Seattle' 에서 근무중인 사원의 직무명을 중복없이 조회한다.
-- 58. 사내의 최고연봉과 최저연봉의 차이를 조회한다.
-- 59. 이름이 'Renske' 인 사원의 연봉과 같은 연봉을 받는 사원의 모든 정보를 조회한다. 단, 'Renske' 사원은 조회에서 제외한다.
-- 60. 회사 전체의 평균 연봉보다 많이 받는 사원들 중 이름에 'u' 가 포함된 사원과 동일한 부서에서 근무중인 사원들의 모든 정보를 조회한다.
-- 61. 부서가 없는 국가명을 조회한다.
-- 62. 'Europe' 에서 근무중인 사원들의 모든 정보를 조회한다.
-- 63. 'Europe' 에서 가장 많은 사원들이 있는 부서명을 조회한다.
-- 64. 대륙 별 사원의 수를 조회한다.
-- 65. 연봉이 2500, 3500, 7000 이 아니며 직업이 SA_REP 이나 ST_CLERK 인 사람들을 조회한다.
-- 66. 사원의 사원번호, 이름, 성, 상사의 사원번호, 상사의 이름, 상사의 성을 조회한다.
-- 67. 101번 사원의 모든 부하직원 들의 이름, 성, 상사사원번호, 상사사원명을 조회한다.
-- 68. 114번 직원의 모든 상사들의 이름, 성, 상사사원번호, 상사사원명을 조회한다.
-- 69. 114번 직원의 모든 상사들의 이름, 성, 상사사원번호, 상사사원명을 조회한다. 단, 역순으로 조회한다.

-- 70. 모든 사원들을 연봉 오름차순 정렬하여 조회한다. 연봉 SALARY

SELECT *
  FROM EMPLOYEES
 ORDER BY SALARY ASC
;
-- 71. 모든 사원들을 이름 내림차순 정렬하여 조회한다. 이름 FIRST_NAME
 
 SELECT *
   FROM EMPLOYEES
  ORDER BY FIRST_NAME DESC
;

-- 72. 모든 사원들의 이름, 성, 연봉, 부서명을 부서번호로 내림차순 정렬하여 조회한다.
-- 73. 부서명별 연봉의 합을 내림차순 정렬하여 조회한다.
-- 74. 직무명별 사원의 수를 오름차순 정렬하여 조회한다.
-- 75. 모든 사원들의 모든 정보를 조회한다. 단, 인센티브를 받는 사원은 "인센티브여부" 컬럼에 "Y"를, 아닌 경우 "N"으로 조회한다.
-- 76. 모든 사원들의 이름을 10자리로 맞추어 조회한다.
-- 77. 2007년에 직무가 변경된 사원들의 현재 직무명, 부서명, 사원번호, 이름, 성을 조회한다.
-- 78. 직무별 최대연봉보다 더 많은 연봉을 받는 사원의 모든 정보를 조회한다


-- 79. 사원들의 입사일자 중 이름, 성, 연도만 조회한다. 입사일짜를 연도 변경 이름 FIRST_NAME 성 LAST_NAME

SELECT FIRST_NAME
     , LAST_NAME
     , TO_CHAR(HIRE_DATE, 'YYYY') 
  FROM EMPLOYEES
;
  
-- 80. 사원들의 입사일자 중 이름, 성, 연도, 월 만 조회한다. 입사일짜 연도 월

SELECT FIRST_NAME
     , LAST_NAME
     , TO_CHAR(HIRE_DATE, 'YYYY-MM') 
  FROM EMPLOYEES
;

-- 81. 100번 사원의 모든 부하직원을 계층조회한다. 단, LEVEL이 4인 사원은 제외한다.
-- 82. 많은 연봉을 받는 10명을 조회한다.
-- 83. 가장 적은 연봉을 받는 사원의 상사명, 부서명을 조회한다.
-- 84. 많은 연봉을 받는 사원 중 11번 째 부터 20번째를 조회한다.
-- 85. 가장 적은 연봉을 받는 중 90번 째 부터 100번째를 조회한다.
-- 86. 'PU_CLERK' 직무인 2번째 부터 5번째 사원의 부서명, 직무명을 조회한다.

-- 87. 모든 사원의 정보를 직무 오름차순, 연봉 내림차순으로 조회한다. 직무 JOB_ID 연봉 SALARY

SELECT *
  FROM EMPLOYEES
 ORDER BY JOB_ID ASC 
     , SALARY DESC 
;



-- 88. 직무별 평균연봉을 평균연봉순으로 오름차순 정렬하여 조회한다.
-- 89. 부서별 평균연봉을 내림차순 정렬하여 조회한다.
-- 90. 이름의 첫 번째 글자별 평균연봉을 조회한다.
-- 91. 입사연도별 최소연봉을 조회한다.
-- 92. 월별 최대연봉 중 2번째 부터 4번째 데이터만 조회한다.
-- 93. 직무명별 최소연봉을 조회한다.
-- 94. 부서명별 최대연봉을 조회한다.
-- 95. 직무명, 부서명 별 사원 수, 평균연봉을 조회한다.
-- 96. 도시별 사원 수를 조회한다.
-- 97. 국가별 사원 수, 최대연봉, 최소연봉을 조회한다.
-- 98. 대륙별 사원 수를 대륙명으로 오름차순 정렬하여 조회한다.
-- 99. 이름이나 성에 'A' 혹은 'a' 가 포함된 사원의 모든 정보를 조회한다.
-- 100. 국가별로 연봉이 5000 이상인 사원의 수를 조회한다.
-- 101. 인센티브를 안받는 사원이 근무하는 도시를 조회한다.
-- 102. 인센티브를 포함한 연봉이 10000 이상인 사원의 모든 정보를 조회한다.
-- 103. 가장 많은 부서가 있는 도시를 조회한다.
-- 104. 가장 많은 사원이 있는 부서의 국가명을 조회한다.
-- 105. 우편번호가 5자리인 도시에서 근무하는 사원명, 부서명, 도시명, 우편번호를 조회한다.
-- 106. 우편번호에 공백이 없는 도시에서 근무하는 사원의 이름, 부서명, 우편번호를 조회한다.
-- 107. "주"가 없는 도시에서 근무하는 사원의 이름, 도시를 조회한다.
-- 108. 국가명이 6자리인 국가의 모든 정보를 조회한다.
-- 109. 사원의 이름과 성을 이용해 EMAIL과 같은 값으로 만들어 조회한다.
-- 110. 모든 사원들의 이름을 10자리로 변환해 조회한다. 예> 이름 => "        이름"
-- 111. 모든 사원들의 성을 10자리로 변환해 조회한다. 예> 성 => "성         "
-- 112. 109번 사원의 입사일 부터 1년 내에 입사한 사원의 모든 정보를 조회한다.
-- 113. 가장 먼저 입사한 사원의 입사일로부터 2년 내에 입사한사원의 모든 정보를 조회한다.
-- 114. 가장 늦게 입사한 사원의 입사일 보다 1년 앞서 입사한 사원의 모든 정보를 조회한다.
-- 115. 도시명에 띄어쓰기 " " 가 포함된 도시에서 근무중인 사원들의 부서명, 도시명, 사원명을 조회한다.
-- 116. MOD 함수를 통해 사원번호가 홀수면 남자, 짝수면 여자 로 구분해 조회한다. MOD(값, 나눌값)
-- 117. '20230222' 문자 데이터를 날짜로 변환해 조회한다.(DUAL)
-- 118. '20230222' 문자 데이터를 'YYYY-MM' 으로 변환해 조회한다.(DUAL)
-- 119. '20230222130140' 문자 데이터를 'YYYY-MM-DD HH24:MI:SS' 으로 변환해 조회한다. (DUAL)
-- 120. '20230222' 날짜의 열흘 후의 날짜를 'YYYY-MM-DD' 으로 변환해 조회한다. (DUAL)
-- 121. 사원 이름의 글자수 별 사원의 수를 조회한다.
-- 122. 사원 성의 글자수 별 사원의 수를 조회한다.
-- 123. 사원의 연봉이 5000 이하이면 "사원", 7000 이하이면 "대리", 9000 이하이면 "과장", 그 외에는 임원 으로 조회한다.
-- 124. 부서별 사원의 수를 조인을 이용해 다음과 같이 조회한다."부서명 (사원의 수)"
-- 125. 부서별 사원의 수를 스칼라쿼리를 이용해 다음과 같이 조회한다. "부서명 (사원의 수)"
-- 126. 사원의 정보를 다음과 같이 조회한다. "사원번호 번 사원의 이름은 성이름 입니다."
-- 127. 사원의 정보를 스칼라쿼리를 이용해 다음과 같이 조회한다. "사원번호 번 사원의 상사명은 상사명 입니다."
-- 128. 사원의 정보를 조인을 이용해 다음고 같이 조회한다. "사원명 (직무명)"
-- 129. 사원의 정보를 스칼라쿼리를 이용해 다음과 같이 조회한다. "사원명 (직무명)"
-- 130. 부서별 연봉 차이(최고연봉 - 최저연봉)가 가장 큰 부서명을 조회한다.
-- 131. 부서별 연봉 차이(최고연봉 - 최저연봉)가 가장 큰 부서에서 근무하는 사원들의 직무명을 중복없이 조회한다.
-- 132. 부서장이 없는 부서명 중 첫 글자가 'C' 로 시작하는 부서명을 조회한다.
-- 133. 부서장이 있는 부서명 중 첫 글자가 'S' 로 시작하는 부서에서 근무중인 사원의 이름과 직무명, 부서명을 조회한다.
-- 134. 지역변호가 1000 ~ 1999 사이인 지역내 부서의 모든 정보를 조회한다.
-- 135. 90, 60, 100번 부서에서 근무중인 사원의 이름, 성, 부서명을 조회한다.
-- 136. 부서명이 5글자 미만인 부서에서 근무중인 사원의 이름, 부서명을 조회한다.
-- 137. 국가 아이디가 'C'로 시작하는 국가의 지역을 모두 조회한다.
-- 138. 국가 아이디의 첫 글자와 국가명의 첫 글자가 다른 모든 국가를 조회한다.
-- 139. 사원 모든 정보 중 이메일만 모두 소문자로 변경하여 조회한다.
-- 140. 사원의 연봉을 TRUNC(소수점 버림) 함수를 사용해 100 단위는 버린채 다음과 같이 조회한다. 예> 3700 -> 3000, 12700 -> 12000
-- 141. 100단위를 버린 사원의 연봉 별 사원의 수를 조회한다.
-- 142. 현재 시간으로부터 20년 전 보다 일찍 입사한 사원의 모든 정보를 조회한다.
-- 143. 부서번호별 현재 시간으로부터 15년 전 보다 일찍 입사한 사원의 수를 조회한다.
-- 144. 부서명, 직무명 별 평균 연봉을 조회한다.
-- 145. 도시명, 지역명 별 사원의 수를 조회한다.
-- 146. 부서명, 직무명 별 평균 연봉 중 가장 작은 평균연봉을 받는 부서명, 직무명을 조회한다.
-- 147. 102번 직원의 모든 부하직원의 수를 조회한다.
-- 148. 113번 직원의 모든 부하직원의 수를 조회한다.
-- 149. 부하직원이 없는 사원의 모든 정보를 조회한다.
-- 150. 사원번호가 100번인 사원의 사원번호, 이름과 사원번호로 내림차순 정렬된 사원의 사원번호, 이름 조회한다.
/*조회 예
--------------------
100    Steven
206    William
205    Shelley
204    Hermann
203    Susan
202    Pat
201    Michael
200    Jennifer
199    Douglas
198    Donald
197    Kevin
196    Alana
...
*/


-- 151. 모든 사원의 모든 정보를 조회한다.
-- 152. 부서가 없는 사원의 모든 정보를 조회한다.
-- 153. 직무가 없는 사원의 모든 정보를 조회한다.
-- 154. 부서와 직무가 모두 있는 사원의 모든 정보를 조회한다.
-- 155. 부서장이 없는 모든 부서의 모든 정보를 조회한다.
-- 156. 부서장이 있는 모든 부서의 모든 정보를 조회한다.
-- 157. 부서장의 모든 사원 정보를 조회한다.
-- 158. 사원의 이름만 조회한다.
-- 159. 사원의 이름이 7자리인 사원의 모든 정보를 조회한다.
-- 160. 사원의 이메일이 6자리인 사원의 모든 정보를 조회한다.
-- 161. 모든 지역의 모든 정보를 조회한다.
-- 162. 지역이 없는 모든 부서의 정보를 조회한다.
-- 163. 지역이 있는 모든 부서의 정보와 도시 정보를 조회한다.
-- 164. 모든 사원의 모든 정보와 부서명을 조회한다.
-- 165. 111번 사원의 모든 정보와 부서명을 조회한다.
-- 166. 115번의 사원의 모든 정보와 부서명, 직무명을 조회한다.
-- 167. 100번 사원의 모든 정보와 부서명, 직무명, 도시명을 조회한다.
-- 168. 부서아이디별 사원의 평균연봉을 조회한다.
-- 169. 직무아이디별 사원의 최고연봉을 조회한다.
-- 170. 부서명별 사원의 수를 조회한다.
-- 171. 직무명별 사원의 평균연봉을 조회한다.
-- 172. 부서명, 직무명별 사원의 수와 평균연봉을 조회한다.
-- 173. 인센티브를 안받는 사원의 모든 정보를 조회한다.
-- 174. 인센티브를 받는 사원의 부서아이디를 중복없이 조회한다.
-- 175. 인센티브를 받는 사원의 직무아이디를 중복없이 조회한다.
-- 176. 사원이 있는 부서의 지역아이디를 조회한다.
-- 177. 사원이 없는 부서의 부서명을 조회한다.
-- 178. 지역별 부서의 수를 조회한다. (부서가 없으면 부서의 수는 0으로 조회한다.)
-- 179. 지역별 사원의 평균연봉을 조회한다. (사원이 없으면 평균연봉은 0으로 조회한다.)
-- 180. Seattle의 부서 아이디를 조회한다.
-- 181. Seattle에서 근무중인 사원의 모든 직무명을 중복없이 조회한다.
-- 182. 사원이 한명도 없는 도시를 조회한다.
-- 183. 사원이 한명이라도 있는 도시를 조회한다.
-- 184. 모든 사원의 정보를 연봉으로 오름차순 정렬하여 조회한다.
-- 185. 부서명별 평균연봉을 부서명으로 내림차순 정렬하여 조회한다.
-- 186. 부서명별 최고연봉을 최고연봉으로 오름차순 정렬하여 조회한다.
-- 187. 부서명이 가장 긴 부서에서 근무중인 사원의 모든 정보를 조회한다.
-- 188. 도시명 별 사원의 수를 도시명으로 오름차순 정렬하여 조회한다.
-- 189. 모든 사원의 사원번호, 이름, 성, 연봉, 인센티브를 포함한 연봉 정보를 조회한다.
-- 190. 매년 10%의 상여금을 받는다고 했을 때, 사원별로 현재까지 받은 상여금의 합과 사원번호, 연봉을 조회한다.
-- 191. 직무가 변경되었던 사원들의 모든 정보를 조회한다.
-- 192. 모든 사원들의 현재 직무명과 과거의 직무명을 조회한다. 만약 직무가 한번도 변경되지 않았다면, 과거의 직무명은 '없음' 으로 조회한다.
-- 193. 직무가 변경될 때마다 연봉이 15% 감소한다고 했을 때, 직무가 변경된 사원들의 감소된 연봉을 조회한다.
-- 194. 2003년에 입사한 사원은 몇 명인지 조회한다.
-- 195. 2002년부터 2006년까지 입사한 사원은 몇명인지 연도별로 조회한다.
-- 196. 113번 사원의 상사의 모든 정보를 조회한다.
-- 197. 100번 사원의 모든 부하직원을 계층조회한다.
-- 198. 113번 사원의 모든 상사를 계증조회한다.
-- 199. IT 부서장의 모든 부하직원을 계층조회한다.
-- 200. 모든 부서의 부서장들의 부하직원을 계층조회한다.
```
