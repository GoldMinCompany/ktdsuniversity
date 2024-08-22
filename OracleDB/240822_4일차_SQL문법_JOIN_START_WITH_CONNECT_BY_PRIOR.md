```sql
-- TODO
-- 1. 서브쿼리 조인문제
-- 부서장의 사원번호가 100인 부서에서 근무하는 사원의 모든 정보를 조회한다.
-- 모르는 것 : 부서장의 사원번호 100인 부서의 번호
-- 부서와 사원의 관계는 근무한다는 관계로 연결되어 있다. 
-- 하나의 부서에서는 여러명의 사원이 근무 중이다.

SELECT *
  FROM EMPLOYEES
 WHERE DEPARTMENT_ID = (SELECT DEPARTMENT_ID
 			  FROM DEPARTMENTS
 			 WHERE MANAGER_ID = 100)
;

-- TODO
-- 2. 서브쿼리 조인문제.
-- 도시명이 Seattle이고 부서의 모든 정보를 조회한다. 
-- 모르는 것 : 도시명이 Seattle인 부서의 번호

SELECT *
  FROM DEPARTMENTS
 WHERE LOCATION_ID = (SELECT LOCATION_ID
			FROM LOCATIONS
 		       WHERE CITY = 'Seattle'
)
;

-- TODO
-- 3. 'Sales Manager' 직무를 담당하고 있는 사원들의 모든 정보를 조회
-- 모르는 것 : 'Sale Manager'의 직무 번호

SELECT *
  FROM EMPLOYEES
 WHERE JOB_ID = (SELECT JOB_ID 
		   FROM JOBS
 		  WHERE JOB_TITLE = 'Sales Manager')

;

-- 4. TODO
-- 'Americas' 대륙에 존재하는 국가들의 모든 정보를 조회

SELECT *
  FROM COUNTRIES
 WHERE REGION_ID = (SELECT REGION_ID
  		      FROM REGIONS
 		     WHERE REGION_NAME = 'Americas')

;

-- 5. TODO
-- 'United States of America' 국가에 존재하는 모든 도시를 조회

SELECT *
  FROM LOCATIONS
 WHERE COUNTRY_ID = (SELECT COUNTRY_ID
		       FROM COUNTRIES
		      WHERE COUNTRY_NAME = 'United States of America')
;
 
-- 6. TODO
-- 'Seattle' 도시에서 근무하는 모든 사원들의 정보를 조회
-- 'Seattle'의 도시 번호가 몇 번인가?
-- 'Seattle'에 있는 부서가 몇 번인가?
-- = 의 값은 하나의 값만 비교 할 수 있기 때문에 2개 이상의 행을 리턴할 수 없다.
-- sub query의 row가 2개 이상일 경우 = 은 사용할 수 없고 IN을 사용한다. 
SELECT *
  FROM EMPLOYEES
 WHERE DEPARTMENT_ID IN (SELECT DEPARTMENT_ID
 			   FROM DEPARTMENTS
 			  WHERE LOCATION_ID = (SELECT LOCATION_ID
					    	 FROM LOCATIONS
					   	WHERE CITY = 'Seattle'))
;

-- 7. TODO
-- 'Sales Manager' 직무를 담당하는 사원들의 부서명을 조회
-- 1. 'Sales Manager'의 직무 번호를 모른다. 사원 검색을 할 수 있기위해서는 직무 번호를 알아야 한다.
-- 2. 'Sales Manager'를 담당하는 사원의 부서번호를 모른다. 부서 번호를 알아야 부서명을 조회할 수 있다. 

SELECT DEPARTMENT_NAME
  FROM DEPARTMENTS
 WHERE DEPARTMENT_ID IN (SELECT DEPARTMENT_ID
			   FROM EMPLOYEES
			  WHERE JOB_ID = (SELECT JOB_ID 
					    FROM JOBS
					   WHERE JOB_TITLE = 'Sales Manager'))
;


* 테이블 조인 : 두개 이상의 테이블을 모두 조회하고자 할 때, JOIN을 사용한다. 
INNER JOIN 교집함
ON 키워드를 통해 두 테이블 간의 관계를 명시해준다. 


-- 8. TODO
-- INNER JOIN 실습
-- 모든 사원들의 사원 정보와 모든 부서 정보를 조회한다. 

SELECT *
  FROM EMPLOYEES E
 INNER JOIN DEPARTMENTS D
    ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
;

-- 9. TODO , JOIN 이용
-- 41. 사원이 근무하는 부서명, 이름, 성을 조회한다.

SELECT E.FIRST_NAME
     , E.LAST_NAME
     , D.DEPARTMENT_NAME
  FROM EMPLOYEES E
 INNER JOIN DEPARTMENTS D
    ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
;

-- 10. TODO
-- 45. 부서명별 사원의 수를 조회한다.
-- 집계함수
-- GROUP BY
-- INNER JOIN

SELECT D.DEPARTMENT_NAME
     , COUNT(1)
  FROM EMPLOYEES E
 INNER JOIN DEPARTMENTS D
    ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
 GROUP BY D.DEPARTMENT_NAME
;

-- 11. TODO
-- 44. 50번 부서의 부서장의 이름, 성, 연봉, 부서명을 조회한다.

SELECT E.FIRST_NAME
     , E.LAST_NAME
     , E.SALARY
     , E.EMPLOYEE_ID
     , D.DEPARTMENT_ID
     , D.DEPARTMENT_NAME
  FROM EMPLOYEES E
 INNER JOIN DEPARTMENTS D 
    ON E.EMPLOYEE_ID = D.MANAGER_ID
 WHERE D.DEPARTMENT_ID = 50   
;

-- 12.TODO
-- 42. 가장 적은 연봉을 받는 사원의 부서명, 이름, 성, 연봉, 부서장사원번호를 조회한다.
-- SUB QUERY, INNER JOIN
SELECT D.DEPARTMENT_NAME
     , E.FIRST_NAME
     , E.LAST_NAME
     , E.SALARY
     , D.MANAGER_ID
  FROM EMPLOYEES E
 INNER JOIN DEPARTMENTS D
    ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
 WHERE E.SALARY = (SELECT MIN(SALARY)
 					           FROM EMPLOYEES)
;

-- 13.TODO
-- 52. 'Seattle' 에서 근무중인 사원의 이름, 성, 연봉, 부서명 을 조회한다.
-- 'Seattle'의 지역 번호를 알지 못한다. sub query가 필요하다.
-- INNDER JOIN

SELECT E.FIRST_NAME 
     , E.LAST_NAME
     , E.SALARY
     , D.DEPARTMENT_NAME
  FROM EMPLOYEES E
 INNER JOIN DEPARTMENTS D
    ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
 WHERE LOCATION_ID = (SELECT LOCATION_ID
  					           	FROM LOCATIONS
 					             WHERE CITY = 'Seattle')


-- 14. TODO
-- 56. 연봉이 7000 에서 12000 사이인 사원이 근무중인 도시를 조회한다.
-- sub query로만 이유
-- 사원 연봉 = 사원 테이블
-- 도시 정보 = 지역 테이블
-- 사원과 지역을 연결하기 위한 부서 테이블
-- 모르는 것 2가지
-- 1. 연봉 7000 ~ 12000 사원의 부서번호
-- 2. 연봉 7000 ~ 12000 사원이 근무중인 지역의 번호
 					   
SELECT *
  FROM LOCATIONS
 WHERE LOCATION_ID IN (SELECT LOCATION_ID
 						 FROM DEPARTMENTS
 						WHERE DEPARTMENT_ID IN (SELECT DEPARTMENT_ID 
        						  						    FROM EMPLOYEES
					  						             WHERE SALARY BETWEEN 7000 AND 12000))
-- 15. TODO
-- 72. 모든 사원들의 이름, 성, 연봉, 부서명을 부서번호로 내림차순 정렬하여 조회한다.

SELECT E.FIRST_NAME
     , E.LAST_NAME
     , E.SALARY
     , D.DEPARTMENT_NAME
  FROM EMPLOYEES E
 INNER JOIN DEPARTMENTS D
    ON E.DEPARTMENT_ID = E.DEPARTMENT_ID
 ORDER BY D.DEPARTMENT_NAME DESC
;

-- 16. TODO
-- 73. 부서명별 연봉의 합을 부서명으로 내림차순 정렬하여 조회한다.

SELECT SUM(SALARY)
     , D.DEPARTMENT_NAME 
  FROM EMPLOYEES E
 INNER JOIN DEPARTMENTS D
    ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
 GROUP BY D.DEPARTMENT_NAME
 ORDER BY D.DEPARTMENT_NAME DESC 
;

-- 17. TODO
-- 74. 직무명별 사원의 수를 직무명으로 오름차순 정렬하여 조회한다.
 
 SELECT J.JOB_TITLE 
      , COUNT(1)
   FROM EMPLOYEES E
  INNER JOIN JOBS J
     ON J.JOB_ID = E.JOB_ID
  GROUP BY J.JOB_TITLE
  ORDER BY JOB_TITLE ASC
;

* 재귀 조인 RECURSIVE JOIN

한 테이블에 PK가 있고, 해당 PK를 참조하는 FK가 동시에 있는 경우 조인

-- 18.TODO
-- 32. 자신의 상사보다 더 많은 연봉을 받는 사원의 모든 정보를 조회한다.
-- 부하직원의 상사 사원번호는 상사정보의 사원번호가 같다. 
SELECT *
  FROM EMPLOYEES EMP -- 부하직원 
 WHERE MANAGER_ID = (SELECT MAN.EMPLOYEE_ID
 					   FROM EMPLOYEES MAN -- 상사직원
 					  WHERE MAN.EMPLOYEE_ID = EMP.MANAGER_ID
 					    AND MAN.SALARY < EMP.SALARY) -- 상사의 번호가 100번 
;

SELECT *
  FROM EMPLOYEES EMP
 WHERE EMP.SALARY > (SELECT SALARY
 					   FROM EMPLOYEES MAN
 					  WHERE MAN.EMPLOYEE_ID = EMP.MANAGER_ID)

;
-- JOIN을 이용

SELECT EMP.*
     , MAN.SALARY
  FROM EMPLOYEES EMP -- 부하직원
 INNER JOIN EMPLOYEES MAN -- 상사직원
    ON EMP.MANAGER_ID = MAN.EMPLOYEE_ID
 WHERE EMP.SALARY > MAN.SALARY 
;

-- 19.TODO
-- 33. 자신의 상사보다 더 일찍 입사한 사원의 모든 정보를 조회한다.
SELECT EMP.HIRE_DATE
     , MAN.HIRE_DATE
  FROM EMPLOYEES EMP
 INNER JOIN EMPLOYEES MAN
    ON EMP.MANAGER_ID = MAN.EMPLOYEE_ID
 WHERE EMP.HIRE_DATE < MAN.HIRE_DATE
;


*계층 조회
나의 사원 번호는 누군가의 상사번호이다. 

-- 20. TODO
-- 실습
-- 계층 실습
-- 100번 직원의 모든 부하직원들을 조회한다.
 SELECT LEVEL -- 계층의 깊이
      , E.EMPLOYEE_ID
      , E.MANAGER_ID
   FROM EMPLOYEES E
  START WITH EMPLOYEE_ID = 100
CONNECT BY PRIOR EMPLOYEE_ID = MANAGER_ID -- 100번의 사원번호는 누군가의 상사번호다.
;

-- START WITH를 사용하면 WHERE을 사용하지 못한다. 

-- 21.TODO
-- 206번 직원의 모든 상사들을 조회한다.
-- FK는 PK와 같다.
 SELECT LEVEL
      , E.*
   FROM EMPLOYEES E
  START WITH EMPLOYEE_ID = 206
CONNECT BY PRIOR MANAGER_ID = EMPLOYEE_ID
  ORDER BY LEVEL DESC
;

-- 22. TODO 						  						 
-- 67. 101번 사원의 모든 부하직원 들의 이름, 성, 상사사원번호을 조회한다.

 SELECT E.EMPLOYEE_ID
      , E.FIRST_NAME
      , E.LAST_NAME
      , E.MANAGER_ID
   FROM EMPLOYEES E
  START WITH EMPLOYEE_ID = 101
CONNECT BY PRIOR EMPLOYEE_ID = MANAGER_ID
;
 						  						 

-- 23. TODO
-- 68. 114번 직원의 모든 상사들의 이름, 성, 상사사원번호을 조회한다.

 SELECT E.FIRST_NAME
      , E.LAST_NAME
      , E.MANAGER_ID
   FROM EMPLOYEES E
  START WITH EMPLOYEE_ID = 114
CONNECT BY PRIOR MANAGER_ID = EMPLOYEE_ID
;

-- 24. TODO
-- 69. 114번 직원의 모든 상사들의 이름, 성, 상사사원번호  단, 역순으로 조회한다.

 SELECT E.FIRST_NAME
      , E.LAST_NAME
      , E.MANAGER_ID
   FROM EMPLOYEES E
  START WITH EMPLOYEE_ID = 114
CONNECT BY PRIOR MANAGER_ID = EMPLOYEE_ID
  ORDER BY LEVEL DESC 
;
```
