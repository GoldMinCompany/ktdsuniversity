
# 1. 날짜 조회하는 방법

* 현재 시간을 조회
  - DUAL은 row가 한개 이고 의미없는 TABLE
```sql
SELECT SYSDATE
  FROM DUAL
;
```

# 2. 날짜를 문자로 변경하는 방법

* 현재 날짜을 '연-월-일' 형식으로 변경

```sql
SELECT TO_CHAR(SYSDATE)
  FROM DUAL
;
```
* 위 SQL문으로 사용하는 경우, 실제 Web server에서 해당 SQL문은 오류를 발생시킨다. 아래의 코드 같이 사용한다.

```sql
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD') 
  FROM DUAL
;
```
* 현재 시간을 '시:분:초' 형식으로 변경
```sql
SELECT TO_CHAR(SYSDATE, 'HH:MI:SS') -- 12시 베이스
	 , TO_CHAR(SYSDATE, 'HH24:MI:SS') -- 24시 베이스
  FROM DUAL
;
```

# 3. 문자를 날짜로 변경하는 방법

* 문자를 날짜로 변환 TO_DATE(문자 타입의 컬럼 또는 값, '포멧')

```sql
SELECT SYSDATE 
     , TO_DATE('2005-09-01','YYYY-MM-DD')
  FROM DUAL
;
```

* 2005년 9월 1일부터 2005년 9월 30일 사이에 입사한 사람들의 정보를 조회
* BETWEEN A AND B는 기간 검색 시 주로 사용된다. 
```sql
SELECT *
  FROM EMPLOYEES
 WHERE HIRE_DATE BETWEEN TO_DATE('2005-09-01','YYYY-MM-DD') AND TO_DATE('2005-09-30','YYYY-MM-DD')
 ORDER BY HIRE_DATE
;
```

# 4. SELECT, FROM
* 하나 이상의 TABLE에서 여러 개의 row(행) 데이터를 조회

```sql
SELECT [COLUMN_NAME], [COLUMN_NAME] ... 
  FROM [TABLE_NAME]
```

# 5. 데이터를 정렬하는 방법

* ORDER BY COLUMN_NAME ASC -> 오름차순 정렬
* ORDER BY COLUMN_NAME DESC -> 내림차순 정렬
* 데이터의 수가 많은 TABLE에서 ORDER BY를 수행하는 경우 성능이 기하급수적으로 떨어진다. 실무에서는 가급적 사용하지 않고 필요할 때 정렬을 수행한다.

# 6. 데이터를 필터링하는 방법

* TABLE에서 특정 조건에 부합하는 데이터만 조회할 때, WHERE 절을 사용한다.
```sql
SELECT [COLUMN_NAME], [COLUMN_NAME] ...
  FROM [TABLE_NAME]
 WHERE [CONDITIONS]
```

# 7. 연산자의 사용 방법

* IN
  - IN에 들어갈 수 있는 값의 최대 갯수는 1000개이다. 비교할 값의 갯수가 명확할 때 사용한다.
  - IN에 들어간 값 중 NULL 연산이 불가능하다. NULL은 IS NULL 또는 IS NOT NULL로 비교 가능하다.
 
```sql
SELECT FIRST_NAME, LAST_NAME, EMPLOYEE_ID, SALARY, MANAGER_ID
  FROM EMPLOYEES 
 WHERE JOB_ID IN ('SA_MAN', 'SA_REP', 'IT_PROG')
   AND SALARY >= 10000
   AND MANAGER_ID = 100
;
```

* LIKE
  - 하나의 COLUMN에 "포함"된 값을 검색한다.
  - 시작, 포함, 끝 검색 시 %가 사용된다.
    - WHERE COLUMN LIKE 'D%' -> COLUMN의 값이 D로 시작한다.
    - WHERE COLUMN LIKE '%D' -> COLUMN의 값이 D로 끝난다.
    - WHERE COLUMN LIKE '%D%' -> COLUMN의 값이 D로 시작, 끝 또는 D가 포함된다.
  - LIKE를 사용하여 글자의 수를 체크할 수 있다. 와일드 카드 : _ (언더바)
    - COLUMN_NAME LIKE '__' -> COLUMN_NAME의 값이 두 글자인가?
    - COLUMN_NAME LIKE '_____' -> COLUMN_NAME의 값이 다섯 글자인가?
    - COLUMN_NAME LIKE 'A__%' -> COLUMN_NAME의 값이 A로 시작해서 두글자 이상이 더 있는가?

* NOT
  - WHERE COLUMN_NAME LIKE '%A' -> COLUMN_NAME의 값이 대문자 A가 포함되어 있는가?
  - WHERE COLUMN_NAME NOT LIKE '%A' -> COLUMN_NAME의 값이 대문자 A가 포함되어 있지 않은가?
  - WHERE COLUMN_NAME IN(10, 20, 30) -> COLUMN_NAME의 값이 10, 20, 30 중 하나인가?
  - WHERE COLUMN_NAME NOT IN(10, 20, 30) -> COLUMN_NAME의 값이 10, 20, 30 모두 아닌가?
  - WHERE COLUMN_NAME BETWEEN A AND B -> COLUMN_NAME의 값이 A와 B 사이인가?
  - WHERE COLUMN_NAME NOT BETWEEN A AND B -> COLUMN_NAME의 값이 A와 B 사이아닌가?
 
* IS NULL, IS NOT NULL
  - COLUMNS_NAME의 값이 NULL 값인지 조회할 때 IS NULL을 사용한다.
  - COLUMNS_NAME의 값이 NULL 값이 아닌지 조회할 때 IS NOT NULL을 사용한다.
 
* AND, OR의 연산순위
  - WHERE 절은 여러 개의 조건을 지원한다.
  - AND, OR를 사용하여 여러 개의 조건을 사용할 수 있다.
  - WHERE 절에 AND와 OR이 모두 있는 경우, AND가 우선순위를 갖기 때문에 우선 순위를 높히기 위해서 조건에 괄호를 사용한다.
 

# SQL 실습 (1 ~ 27)
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



-- 23. 직무 아이디가 'PU_CLERK'인 사원 중 연봉이 3000 이상인 사원들의 모든 정보를 조회한다.
-- 24. 평균 연봉보다 많이 받는 사원들의 사원번호, 이름, 성, 연봉을 조회한다.
-- 25. 평균 연봉보다 적게 받는 사원들의 사원번호, 연봉, 부서번호를 조회한다.
-- 26. 가장 많은 연봉을 받는 사원의 사원번호, 이름, 연봉을 조회한다.


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
```
