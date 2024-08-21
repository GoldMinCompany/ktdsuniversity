# 집계함수

* 개수 세기 COUNT(COLUMN_NAME)
  - 행의 수
```sql
SELECT COUNT(EMPLOYEE_ID)
  FROM EMPLOYEES
;
```

위 코드는 실무에서 사용하지 않는다. Primary Key로 개수를  세는 방법은 사용하지 않는다. 

```sql
SELECT COUNT(1)
  FROM EMPLOYEES
;
```
1은 특별한 의미를 갖지 않고 의미 없는 숫자를 사용하여 숫자를 센다.

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
