# 집계함수

* 개수 세기 COUNT(COLUMN_NAME)

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
