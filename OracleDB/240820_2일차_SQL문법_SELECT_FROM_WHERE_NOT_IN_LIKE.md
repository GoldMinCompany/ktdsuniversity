
# 1. 날짜 조회하는 방법

* 현재 시간을 조회
  - DUAL은 row가 한개 이고 의미없는 TABLE
```sql
SELECT SYSDATE
  FROM DUAL
;
```

# 2. 날짜를 문자로 변경하는 방법

* 현재 시간을 '연-월-일' 형식으로 변경

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
    - COLUMN_NAME LIKE 'A__%' -> COLUMN_NAME의 값이 다섯 글자인가?

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
