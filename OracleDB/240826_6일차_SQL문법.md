```sql
240826 수업 내용 정리

-- 1. TODO
-- 124. 부서별 사원의 수를 조인을 이용해 다음과 같이 조회한다."부서명 (사원의 수)"
-- 모름
-- concat, || 문자열 연결
SELECT '부서명 (' || 10 || ')'
  FROM DUAL
;

SELECT D.DEPARTMENT_NAME || '(' || COUNT(E.EMPLOYEE_ID) || ')' AS 부서별_사원_수 
  FROM DEPARTMENTS D
  LEFT OUTER JOIN EMPLOYEES E
    ON D.DEPARTMENT_ID = E.DEPARTMENT_ID
 GROUP BY D.DEPARTMENT_NAME
;

-- 2. TODO
-- 125. 부서별 사원의 수를 스칼라쿼리를 이용해 다음과 같이 조회한다. "부서명 (사원의 수)"
-- 모름
-- 스칼라쿼리 문법, 스칼라 쿼리는 한개의 컬럼만 반환하고 한개의 ROW만 반환 해야한다. 
-- 스칼라쿼리는 밖의 테이블을 사용할 수 있다. 
-- 실무에서는 스칼라 쿼리를 사용 하지 않는다.
SELECT D.NOW
     , (SELECT D.NOW + 1
     	  FROM DUAL)
  FROM (SELECT SYSDATE AS NOW
  		  FROM DUAL) D
;


SELECT D.DEPARTMENT_NAME || '(' || (SELECT COUNT(1)
          			      FROM EMPLOYEES E
         			     WHERE D.DEPARTMENT_ID = E.DEPARTMENT_ID) || ')' AS 부서별사원수
  FROM DEPARTMENTS D
;  


-- 3.TODO
-- 127. 사원의 정보를 스칼라쿼리를 이용해 다음과 같이 조회한다. "사원번호 번 사원의 상사명은 상사명 입니다."
-- 모름

SELECT E.EMPLOYEE_ID || '번 ' || (SELECT MAN.FIRST_NAME || ' ' || MAN.LAST_NAME
				   FROM EMPLOYEES MAN
				  WHERE E.MANAGER_ID = MAN.EMPLOYEE_ID) 
  FROM EMPLOYEES E
  
;


-- 4.TODO
-- 129. 사원의 정보를 스칼라쿼리를 이용해 다음과 같이 조회한다. "사원명 (직무명)"
-- 모름

SELECT E.FIRST_NAME || '(' || (SELECT J.JOB_TITLE || ')'
				 FROM JOBS J
				WHERE E.JOB_ID = J.JOB_ID) 
  FROM EMPLOYEES E
;


-- 138. 국가 아이디의 첫 글자와 국가명의 첫 글자가 다른 모든 국가를 조회한다.
-- 모름
-- SUBSTR()
-- 자주 사용되는 함수가 아니다. 


-- 139. 사원 모든 정보 중 이메일만 모두 소문자로 변경하여 조회한다.
-- 모름
-- LOWER(COLUMN_NAME)
-- 자주 사용하는 함수가 아니다. 



-- 140. 사원의 연봉을 TRUNC(소수점 버림) 함수를 사용해 100 단위는 버린채 다음과 같이 조회한다. 예> 3700 -> 3000, 12700 -> 12000
```sql
-- 모름
-- 자주 사용하는 함수가 아니다. 

-- 5.TODO
-- 150. 사원번호가 100번인 사원의 사원번호, 이름과 사원번호로 내림차순 정렬된 사원의 사원번호, 이름 조회한다.
-- 집합을 두개를 만들어야 한다. 
-- 하나의 테이블에서 두개의 집합이 나온다.
-- UNION 두 개이상의 집합을 하나로 만들 때 사용된다.
-- UNION ALL 
-- 타입과 타입의 순서도 동일하게 맞춰야 한다.
-- 치명적인 단점 UNION ALL을 통해 두 개 이상의 테이블을 하나로 합칠 수 있는데 각각의 테이블을 조회할 때의 속도가 증가하므로 네트워크 지연 시간을 초과할 수 가 있어 치명적인 단점을 갖고 있다.
SELECT EMPLOYEE_ID
     , FIRST_NAME
  FROM EMPLOYEES
 WHERE EMPLOYEE_ID = 100
 UNION ALL
 SELECT *
   FROM (SELECT EMPLOYEE_ID
     	      , FIRST_NAME 
  	   FROM EMPLOYEES
	  WHERE EMPLOYEE_ID != 100
 	  ORDER BY EMPLOYEE_ID DESC)

;


-- 데이터 모델링
-- 정규화를 반드시 알 필요가 있다. 
-- "데이터를 어떤식으로 구성할 것인가?"
-- 1차 정규화
-- 2차 정규화
-- 3차 정규화
-- BCNF
-- 4차 정규화
-- 5차 정규화 -> 실제 실무에서 사용되는 정규화 과정. 모든 데이터를 중복없이 유일한 값으로 정리로 한다. 
-- 1. Multi value 를 제거해야 한다. -> PK가 중복되는 값이 생길 수 있다. 멀티 벨류를 제거하면 반드시 생기는 현상이다. pk의 중복을 피하기 위해서는 장르, 출연진의 데이터가 별도의 테이블로 분리한다. 관계를 생성한다는 것을 의미한다. 하나의 영화는 여러개의 장르를 가질 수 있고 하나의 장르는 여러개의 영화를 가질 수 있다. 다:대:다 다대다 인경우 PK가 중복되는 경우가 발생하기 때문에 별도의 테이블이 필요하다. 별도의 테이블을 통해 1대다 1대다의 관계를 성립하도록 한다. 
-- 2. Duplicate PK 제거한다. 
-- 각각의 테이블들이 고유값을 갖는 것을 완전 정규화라 한다.

``` 
