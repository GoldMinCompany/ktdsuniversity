# INLINE VIEW

* Sub query가 FROM 절에서 사용되는 경우, 해당 서브쿼리를 "INLINE VIEW"라 한다. 
FROM 절에서 사용된 서브쿼리의 결과가 하나의 테이블에 대한 뷰(VIEW)로 사용된다.
* INLINE VIEW 사용 이유
  - 서브쿼리를 통해 메인 쿼리로 올라갈 수록 쿼리의 길이가 점점 길어지고 가독성이 떨어진다. 이때, FROM절에서 사용하는 인라인뷰에 별칭(Alias)를 생성하여 간단하게 뷰(VIEW)를 만들 수 있다.
  - 전체 테이블을 비교하는 것보다 테이블 일부 데이터만 불러와 그 중에서 조건을 따지는 것이 비교하는 횟수가 적다. 

# LEFT OUTER JOIN

* 메인 테이블과 참조 테이블 두 종류의 테이블이 있다. 왼쪽에 있는 것이 메인 테이블이고 오른쪽에 있는 것이 참조 테이블이다.
* 왼쪽에 있는 메인 테이블의 값이 NULL 값이 있더라도 온전히 출력되고자 할 때, 참조 테이블의 연결되는 COLUMN을 연결하여 VIEW를 생성합니다.
* 중복되는 데이터가 필연적으로 발생할 수 있고 메인 테이블이 중복이 되거나 참조 테이블이 중복이 될 수 있고 NULL 값을 포함할 수 있다.
* 이 때, 사용하는 것이 LEFT OUTER JOIN 이다.
* 예를 들어, 쇼핑몰을 운영한다고 합시다. 어떤 사람이 가전기기를 파는데 냉장고도 팔 수 있고, 에어컨을 팔 수도 있고, TV 등 여러가지 가전제품을 팔 수가 있습니다. 쇼핑몰 사장이 가전기기가 얼마나 팔렸는지 궁금할 수 가 있습니다. 필수적으로 "주문"이라는 정보가 생기는데 상품이라는 데이터와 주문이라는 데이터를 INNER JOIN 하는 경우, 교집합인 부분만 출력이 되기 때문에 다른 상품에 대한 주문 정보를 알 수가 없다. 어떤 제품이 얼만큼 팔렸는지를 알 수 있기 위해서는 INNER JOIN이 아닌, LEFT OUTER JOIN을 사용해합니다.  

```sql
ppt 30 테이블 조인(inline view)
from에서 sub query를 쓸 수 있는 방법을 의미한다.
왜 사용하는가? join 만으로 풀리지 않는 문제들을 풀기 위해서 사용한다. 

컬럼 명을 바꾸고 싶을 때 AS 키워드 사용
EMPLOYEES TABLE에 HIRE_YEAR라는 컬럼이 없다.
임시 테이블을 만들어 조회를 한다.

-- 240823
-- 1.TODO
-- INLINE VIEW
-- 2007년에 입사한 모든 사원들의 모든 정보를 조회한다. 
SELECT *
  FROM EMPLOYEES
 WHERE HIRE_DATE BETWEEN TO_DATE('2007-01-01', 'YYYY-MM-DD') AND TO_DATE('2007-01-31', 'YYYY-MM-DD')
;

SELECT EMPLOYEE_ID
		      , FIRST_NAME
		      , LAST_NAME
		      , EMAIL
		      , PHONE_NUMBER
		      , HIRE_DATE
		      , JOB_ID
		      , SALARY
		      , COMMISSION_PCT
		      , MANAGER_ID
		      , DEPARTMENT_ID
  FROM (SELECT EMPLOYEE_ID
		     , FIRST_NAME
		     , LAST_NAME
		     , EMAIL
		     , PHONE_NUMBER
		     , HIRE_DATE
		     , TO_CHAR(HIRE_DATE, 'YYYY') AS HIRE_YEAR
		     , JOB_ID
		     , SALARY
		     , COMMISSION_PCT
		     , MANAGER_ID
		     , DEPARTMENT_ID
		  FROM EMPLOYEES) EMPLOYEES -- Alias
 WHERE HIRE_YEAR = '2007'
;

-- 2.TODD
-- INLINE VIEW
-- 성능 개선을 위해 집계와 정렬을 따로 수행한다. 
-- 88. 직무별 평균연봉을 평균연봉순으로 오름차순 정렬하여 조회한다.
SELECT JOB_TITLE
     , AVG_SALARY
  FROM (SELECT J.JOB_TITLE
    	     , AVG(E.SALARY) AS AVG_SALARY 
 		  FROM JOBS J
		 INNER JOIN EMPLOYEES E
   		    ON J.JOB_ID = E.JOB_ID 
 		 GROUP BY J.JOB_TITLE) JOB_AVG_SALARY
 ORDER BY AVG_SALARY
;

-- 3. TODO
-- INLINE VIEW
-- 89. 부서별 평균연봉을 내림차순 정렬하여 조회한다.

SELECT DEPARTMENT_NAME
     , AVG_SALARY
  FROM (SELECT D.DEPARTMENT_NAME
		     , AVG(E.SALARY) AS AVG_SALARY
  		  FROM EMPLOYEES E
 		 INNER JOIN DEPARTMENTS D
    		ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
 		 GROUP BY D.DEPARTMENT_NAME)		 
 ORDER BY AVG_SALARY
;

-- 4. TODO
-- INLINE VIEW, 다시해보기
-- 91. 입사연도별 최소연봉을 조회한다.

SELECT HIRE_YEAR
     , MIN(SALARY) AS MIN_SALARY
  FROM (SELECT TO_CHAR(HIRE_DATE,'YYYY') AS HIRE_YEAR
             , SALARY
  		  FROM EMPLOYEES) HIRE_YEAR_SALARY
 GROUP BY HIRE_YEAR
;

-- 5. TODO
-- 사원 테이블을 연봉 순으로 내림 차순
-- 11번 째 부터 20번째까지 조회
-- Oracle ROWNUM 패치가 완료된 이후에 조건을 걸어야 한다. 첫번 째인라인 뷰가 된다. 첫번째 인라뷰의 ROWNUM으로 검색을 해야되기 때문에 별칭을 만들어 접근한다. 
-- 84. 많은 연봉을 받는 사원 중 11번 째 부터 20번째를 조회한다.

SELECT ROWNUM -- 행의 번호를 알려줌 오라클 전용 
     , E.*
  FROM EMPLOYEES 
 ORDER BY SALARY DESC 
;

SELECT *
  FROM (SELECT ROWNUM AS R_NUM
   		     , EMP_1.*
  		  FROM (SELECT *
  		  		  FROM EMPLOYEES 
 		 	     ORDER BY SALARY DESC ) EMP_1
 	     WHERE ROWNUM <= 20)
 WHERE R_NUM >= 11
;

-- 6. TODO
-- 82. 많은 연봉을 받는 10명을 조회한다.

SELECT ROWNUM
     , EMP.FIRST_NAME
     , EMP.LAST_NAME
     , EMP.SALARY
  FROM (SELECT *
  		  FROM EMPLOYEES
 		 ORDER BY SALARY DESC ) EMP
 WHERE ROWNUM <= 10
;

-- 7. TODO
-- 85. 가장 적은 연봉을 받는 중 90번 째 부터 100번째를 조회한다.

SELECT R_NUM
     , E2.FIRST_NAME
     , E2.LAST_NAME
     , E2.SALARY
  FROM (SELECT ROWNUM AS R_NUM
     		 , E1.FIRST_NAME
     		 , E1.LAST_NAME
     		 , E1.SALARY
  		  FROM (SELECT *
  		  		  FROM EMPLOYEES
 		 		 ORDER BY SALARY ASC) E1
 		 WHERE ROWNUM <= 100) E2
 WHERE R_NUM >= 90
;

-- 8. TODO
-- 다시해보기 
-- 86. 'PU_CLERK' 직무인 2번째 부터 5번째 사원의 부서명, 직무명을 조회한다.

SELECT *
  FROM (SELECT *
  		  FROM (SELECT ROWNUM AS R_NUM
     				 , EMP_1.*
 		  		  FROM (SELECT *
 		  		  		  FROM EMPLOYEES E
 		 		 		 WHERE JOB_ID = 'PU_CLERK') EMP_1
 		 		 WHERE ROWNUM <= 5) EMP_2
		 WHERE R_NUM >= 2) EMP_3
 INNER JOIN JOBS J
    ON EMP_3.JOB_ID = J.JOB_ID
 INNER JOIN DEPARTMENTS D
    ON EMP_3.DEPARTMENT_ID = D.DEPARTMENT_ID
 
 ;

* 외부조인 

pk와 fk와 상관없이 컬럼에 따라 결정된다? 


레프트 아웃 조인

메인 테이블과 참조 테이블이 생기는데 왼쪽에 있는 것이 메인 테이블 오른쪽에 있는것이 참조테이블
왼쪽에 있는 테이블은 null 값이 있더라도 온전히 출력되고 참조 테이블의 연결되는 컬럼을 붙쳐서 뷰를 생성한다. 
중복되는 데이터가 필연적으로 생긴다. 메인테이블이 중복이 되거나 참조 테이블이 중복될 수 있다.
참조 테이블에도 null 값이 들어 갈 수 있다.  
언제 쓰는 것이 좋은가? 

쇼핑몰로 예를 들어보자. 어떤 사람이 가전기기를 판다. 냉장고도 팔 수 있고, 에어컨을 팔 수 있고, TV를 팔 수도 있다. 등등등
사람들이 와서 가전기기를 산다. 제품 사장은 가전기기가 얼마나 팔렸는지 궁금할 수 있다. 필수적으로 주문이라는 정보가 생긴다.
A라는 회원이 냉장고를 하나 샀다. A라는 회원이 에어컨을 하나 샀다. B라는 회원이 냉장고를 하나 샀다. B라는 회원이 에어컨을 두개 삿다
상품이라는 데이터와 주문이라는 테이블 이너 조인을 했을 경우 교집합인 부분만 출력이 되기 때문에 다른 상품에 대한 주문수량을 알 수가 없다.
뭐가 팔리지 않았는가에 대한 정보를 알 수 없다. 이 정보를 알기 위해서는 레프트 아웃터 조인을 사용해야 한다. 

가전기기 left outer join 주문
 on ~~~~ 

라이트 아웃 조인

메인테이블이 오른쪽 테이블이다. 중복된 값이 생길 수 있다. 방향성만 다르기 때문에 라이트 아웃조인은 잘 사용하지 않는다.

-- 9. TODO
-- 43. 상사사원번호를 중복없이 조회한다.

SELECT DISTINCT MANAGER_ID -- 하나 열?! 중복제거 실무에서 사용하지 않는다. 손실되는 데이터가 생긴다. 
  FROM EMPLOYEES
 WHERE MANAGER_ID IS NOT NULL --NULL 값 제거
;

-- 10.TODO
-- 43-1 부하직원이 없는 직원들만 조회한다. 
-- 상사사번 | 부하직원사번
-- 100    | 101
-- 100    | 102
-- 180    | NULL

SELECT *
  FROM EMPLOYEES MAN -- 상사 직원 
  LEFT OUTER JOIN EMPLOYEES EMP -- 부하 직원 
    ON MAN.EMPLOYEE_ID = EMP.MANAGER_ID
 WHERE EMP.EMPLOYEE_ID IS NULL
;

-- 11.TODO
-- 43-2 상사의 부하직원의 수를 조회
-- 사원 번호 | 부하직원의 수
-- 100     | 1
-- 101     | 2
-- 170     | 0

SELECT MAN.EMPLOYEE_ID
     , COUNT(EMP.EMPLOYEE_ID) AS EMP_COUNT -- 조인하는 경우 집계를 하는 방법이 달라진다. ROW가 존재하는가? INNER JOIN의 경우 교집합이기 때문에 상관없지만 명확히 컬럼 명을 입력해야한다. 
  FROM EMPLOYEES MAN -- 상사 직원
  LEFT OUTER JOIN EMPLOYEES EMP -- 부하 직원
    ON MAN.EMPLOYEE_ID = EMP.MANAGER_ID
 GROUP BY MAN.EMPLOYEE_ID
;

-- 12. TODO 
-- 177. 사원이 없는 부서의 부서명을 조회한다.

-- 13. TODO 
-- 178. 지역별 부서의 수를 조회한다. (부서가 없으면 부서의 수는 0으로 조회한다.)
-- 도시명    | 부서의 수 
-- Seattle  | 12
-- Seoul    | 0 
 
 
SELECT L.CITY 
     , COUNT(D.DEPARTMENT_ID) AS DEPT_COUNT
  FROM LOCATIONS L 
  LEFT OUTER JOIN DEPARTMENTS D
    ON L.LOCATION_ID = D.LOCATION_ID  
 GROUP BY L.CITY
;
 
-- 14. TODO
-- NULL VALUE : NVL(NULL 가능성이 있는 컬럼, 기본값) 오라클 전용 문법
-- 컬럼의 값이 NULL 일 때, 기본 값으로 조회시킨다. 
-- 179. 도시명별 사원의 평균연봉을 조회한다. (사원이 없으면 평균연봉은 0으로 조회한다.) -> LEFT OUTER JOIN 이용

SELECT L.CITY 
     , NVL(AVG(E.SALARY), 0) AS AVG_SALARY
  FROM LOCATIONS L
  LEFT OUTER JOIN DEPARTMENTS D
    ON L.LOCATION_ID = D.LOCATION_ID
  LEFT OUTER JOIN EMPLOYEES E
    ON D.DEPARTMENT_ID = E.DEPARTMENT_ID
 GROUP BY L.CITY 
;

-- 15. TODO
-- 38-1. 직무명별 사원의 수를 조회한다. 사원의 수가 없을 경우 0으로 조회한다. 

SELECT J.JOB_TITLE 
     , NVL(COUNT(E.EMPLOYEE_ID), 0) AS
  FROM JOBS J
  LEFT OUTER JOIN EMPLOYEES E
    ON J.JOB_ID = E.JOB_ID
 GROUP BY J.JOB_TITLE
;
-- 16. TODO
-- 38-2. 국가명별 사원의 최소연봉과 최대연봉을 조회한다. 사원의 수가 없을 경우 0으로 조회한다.  

SELECT C.COUNTRY_NAME
     , NVL(MIN(E.SALARY), 0) AS MIN_SALARY
     , NVL(MAX(E.SALARY), 0) AS MAX_SALARY
  FROM COUNTRIES C
  LEFT OUTER JOIN LOCATIONS L
    ON C.COUNTRY_ID = L.COUNTRY_ID
  LEFT OUTER JOIN DEPARTMENTS D
    ON L.LOCATION_ID = D.LOCATION_ID
    LEFT OUTER JOIN EMPLOYEES E
    ON D.DEPARTMENT_ID = E.DEPARTMENT_ID
 GROUP BY C.COUNTRY_NAME 
;


-- 16. TODO
-- 38-2. 국가명별 사원의 최소연봉과 최대연봉을 조회한다. 사원의 수가 없을 경우 0으로 조회한다.  
-- 강사님 풀이

SELECT C.COUNTRY_NAME
     , NVL(MIN(EMP_DEPT.SALARY), 0) AS MIN_SALARY
     , NVL(MAX(EMP_DEPT.SALARY), 0) AS MAX_SALARY
  FROM COUNTRIES C
  LEFT OUTER JOIN (SELECT * 
  					 FROM EMPLOYEES E
  					INNER JOIN DEPARTMENTS D
  					   ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
  					INNER JOIN LOCATIONS L
  					   ON D.LOCATION_ID = L.LOCATION_ID) EMP_DEPT
  			   ON C.COUNTRY_ID = EMP_DEPT.COUNTRY_ID
 GROUP BY C.COUNTRY_NAME 
;

-- 17. TODO
-- 47. 사원이 없는 부서명을 조회한다.
-- 사원 테이블 = DEPARTMENT_ID
-- 부서 테이블 = DEPARTMENT_ID
-- IN 연산은 NULL 비교가 되지 않는다. NULL을 제거한다. 
SELECT *
  FROM DEPARTMENTS
 WHERE DEPARTMENT_ID NOT IN (SELECT DEPARTMENT_ID
 			       FROM EMPLOYEES
 			      WHERE DEPARTMENT_ID IS NOT NULL) 
;

-- 18. TODO
-- 차집합..?
-- 49. 직무가 변경된적 없는 사원의 모든 정보를 조회한다.

SELECT *
  FROM EMPLOYEES
 WHERE EMPLOYEE_ID NOT IN (SELECT EMPLOYEE_ID
 			     FROM JOB_HISTORY)
;

-- 19.TODO
-- 55. 근무중인 사원이 없는 도시를 조회한다.
-- JOIN 과 NOT IN

SELECT *
  FROM LOCATIONS
 WHERE LOCATION_ID NOT IN (SELECT LOCATION_ID
 			     FROM DEPARTMENTS D
 			    INNER JOIN EMPLOYEES E
 			       ON D.DEPARTMENT_ID = E.DEPARTMENT_ID)
;

-- 20. TODO
-- 61. 부서가 없는 국가명을 조회한다.
-- 필요 테이블 : 부서, 지역, 국가
-- 먼저 알아야할 정보 : 부서가 있는 지역 
-- 국가에서 부서가 있는 지역을 빼준다. 
SELECT C.COUNTRY_NAME
  FROM COUNTRIES C
 WHERE COUNTRY_ID NOT IN (SELECT L.COUNTRY_ID
 			    FROM LOCATIONS L
 			   INNER JOIN DEPARTMENTS D
 			      ON L.LOCATION_ID = D.LOCATION_ID)
;

-- 21. TODO
-- 54. 근무중인 사원이 가장 많은 도시와 사원의 수를 조회한다.  
-- INLINE VIEW 사용
-- 1. 도시명별 사원의 수를 조회
-- 2. 사원의 수로 내림차순 정렬
-- 3. 정렬된 데이터에서 첫번째 ROW만 조회
-- ROWNUM = 1
SELECT *
  FROM (SELECT *
		  FROM (SELECT L.CITY
		     	     , COUNT(1) AS EMP_COUNT
		  		  FROM LOCATIONS L
		 		 INNER JOIN DEPARTMENTS D
		    	    ON L.LOCATION_ID = D.LOCATION_ID
		 		 INNER JOIN EMPLOYEES E
		    		ON D.DEPARTMENT_ID = E.DEPARTMENT_ID
		 		 GROUP BY L.CITY) CITY_EMP_COUNT
 		ORDER BY EMP_COUNT DESC) CITY_EMP_COUNT
WHERE ROWNUM = 1
;

-- 22. TODO
-- 75. 모든 사원들의 모든 정보를 조회한다. 
-- 단, 인센티브를 받는 사원은 "인센티브여부" 컬럼에 "Y"를, 아닌 경우 "N"으로 조회한다.
SELECT EMPLOYEE_ID
	 , FIRST_NAME
	 , LAST_NAME
	 , EMAIL
	 , PHONE_NUMBER
	 , HIRE_DATE
	 , JOB_ID
	 , SALARY
	 , COMMISSION_PCT
	 , MANAGER_ID
	 , DEPARTMENT_ID
	 , CASE  -- COMMISSION_PCT가 NULL 이라면 N 아니라면 Y
	     WHEN COMMISSION_PCT IS NULL THEN 'N'
	     ELSE 'Y'
		END AS 인센티브_여부
	 , CASE
		 WHEN SALARY < 2500 THEN '사원'
		 WHEN SALARY < 5000 THEN '대리'
		 WHEN SALARY < 7500 THEN '과장'
		 ELSE '차장'
		END AS 직급
	 , CASE EMPLOYEE_ID
	 	 WHEN 100 THEN '임원'
	 	 ELSE '직원'
	 END AS 임직원
  FROM EMPLOYEES
;

-- 23. TODO
-- 76. 모든 사원들의 이름을 10자리로 맞추어 조회한다.
-- LPAD(), RPAD()는 잘 사용하지 않는다. 
-- LPAD() 사원의 이름을 10자리로 맞춘다. "Abc" -> "_______Abc"
-- DATA INSERT시 매번 사용

SELECT FIRST_NAME
	 , LENGTH(FIRST_NAME)
	 , LPAD(FIRST_NAME, 10, '_')
	 , LENGTH(LPAD(FIRST_NAME, 10, '_'))
  FROM EMPLOYEES
;

-- 24. TODO
-- 오늘에서 하루를 더해라.
SELECT SYSDATE -- 오늘 
  	 , SYSDATE - 1 -- 하루 전
  	 , SYSDATE + 1 -- 하루 후
  	 , SYSDATE - 2 -- 이틀 전
  	 , SYSDATE + 2 -- 이틀 후
  	 , ADD_MONTHS(SYSDATE, -1) -- 한달 전
  	 , ADD_MONTHS(SYSDATE, 1) -- 한달 후
  	 , ADD_MONTHS(SYSDATE, -2)-- 두달 전
  	 , ADD_MONTHS(SYSDATE, 2)-- 두달 후
  	 , ADD_MONTHS(SYSDATE, -3 * 12)-- 3년 전 
  	 , ADD_MONTHS(SYSDATE, 3 * 12)-- 3년 후 
  	 , SYSDATE - 1 / 24 -- 1시간 전
  	 , SYSDATE + 1 / 24 -- 1시간 후 
  	 , SYSDATE - 3 / 24 -- 3시간 전
  	 , SYSDATE + 3 / 24 -- 3시간 후 
  	 , SYSDATE - 10 / 24 / 60 -- 10분 전
  	 , SYSDATE + 10 / 24 / 60 -- 10분 후 
  	 , SYSDATE - 30 / 24 / 60 / 60 -- 30초 전
  	 , SYSDATE + 30 / 24 / 60 / 60 -- 30초 후 
  FROM DUAL
;
```
