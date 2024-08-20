
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
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD') -- 정상 
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
