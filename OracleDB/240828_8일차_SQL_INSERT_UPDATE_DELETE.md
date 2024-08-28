```sql
/*
INSERT INTO TABLE (COLUMN, COLUMN, COLUMN)
VALUES (VALUE, VALUE, VALUE)
;
*/

INSERT INTO AGE_LMT 
 (AGE_LMT_ID
, AGE_LMT_NM)
VALUES 
 ('00002'
 ,'7세 이상 관람가')
;


INSERT INTO AGE_LMT 
 (AGE_LMT_ID
, AGE_LMT_NM)
VALUES 
 ('00003'
 ,'12세 이상 관람가')
;

INSERT INTO AGE_LMT 
 (AGE_LMT_ID
, AGE_LMT_NM)
VALUES 
 ('00004'
 ,'15세 이상 관람가')
;

INSERT INTO AGE_LMT 
 (AGE_LMT_ID
, AGE_LMT_NM)
VALUES 
 ('00005'
 ,'19세 이상 관람가')
;


SELECT MV_INFO_PK_SEQ.NEXTVAL
  FROM DUAL
;

INSERT INTO MV_INFO
 (MV_ID
, MV_TTL
, OPL_DT
, SCRNG_TM
, AGE_LMT_ID
, PSTR
, PLT)
VALUES
 (MV_INFO_PK_SEQ.NEXTVAL
, '매트릭스3'
, TO_DATE('1999-05-15', 'YYYY-MM-DD')
, 136
, '00003'
, 'https://media.themoviedb.org/t/p/w300_and_h450_bestv2/yI9r0iz2XvlevxUzxvdoQmv3yce.jpg'
, ' ')
;




COMMIT; -- 데이터의 수정을 확정 짓는다. 

ROLLBACK; -- 데이터의 수정을 취소한다. 

SELECT M.MV_ID 
     , M.MV_TTL
     , A.AGE_LMT_ID 
     , A.AGE_LMT_NM
  FROM MV_INFO M
 INNER JOIN AGE_LMT A
    ON M.AGE_LMT_ID = A.AGE_LMT_ID
;

-- 'MA-20240827-000001'
-- 접두어 - 년월일 - 시퀀스

SELECT 'MA' || '-' || TO_CHAR(SYSDATE, 'YYYY-MM-DD') || '-' || LPAD(MV_ACTR_PK_SEQ.NEXTVAL, 6, '0')
  FROM DUAL
;

-- 배우 3명 추가
-- INSERT INTO JOIN 불가
-- UPDATE JOIN 불가
-- DELETE JOIN 불가
INSERT INTO ACTR 
 (ACTR_ID
 ,ACTR_NM
 ,ACTR_ENG_NM
 ,ACTR_BRTH_DT
 ,LCTNS_ID
 ,ACTR_PRFL_PHT_URL)
VALUES
 (LPAD(ACTR_PK_SEQ.NEXTVAL, 5, '0')
 ,'아치르노'
 ,'Archie Renaux'
 , NULL
 , NULL
 , 'https://cf.lottecinema.co.kr//Media/MovieFile/PersonImg/121000/120416_107_4.jpg')
;

COMMIT;

INSERT INTO LCTNS
 (LCTNS_ID
, LCTNS_NAME)
VALUES
 (LPAD(LCTNS_PK_SEQ.NEXTVAL, 5, '0')
, '영국')

COMMIT;

SELECT * 
  FROM ACTR
;

-- 출연진 추가

INSERT INTO MV_ACTR
 (MV_ACTR_ID
, ACTR_ID
, MV_ID)
VALUES
 ('MA-' || TO_CHAR(SYSDATE, 'YYYY-MM-DD') || '-' || LPAD(MV_ACTR_PK_SEQ.NEXTVAL, 6, '0')
, '00003'
, 1)
;

COMMIT;

-- 에일리언 로물루스에 출연한 모든 배우들의 이름과 영화 이름, 출생지을 조회한다.

SELECT ACTR_NM 
     , MV_TTL
     , LCTNS_NAME
  FROM MV_INFO M
 INNER JOIN MV_ACTR MA
    ON M.MV_ID = MA.MV_ID
 INNER JOIN ACTR A
    ON A.ACTR_ID = MA.ACTR_ID
 LEFT OUTER JOIN LCTNS L
    ON A.LCTNS_ID = L.LCTNS_ID
 WHERE M.MV_TTL = '에일리언 로물루스'
;

SELECT ACTR_NM 
     , MV_TTL
     , LCTNS_NAME
  FROM MV_INFO M
 INNER JOIN MV_ACTR MA
    ON M.MV_ID = MA.MV_ID
   AND M.MV_TTL = '에일리언 로물루스'
 INNER JOIN ACTR A
    ON A.ACTR_ID = MA.ACTR_ID
 LEFT OUTER JOIN LCTNS L
    ON A.LCTNS_ID = L.LCTNS_ID
 WHERE M.MV_TTL = '에일리언 로물루스'
;

-- 데이터 수정
UPDATE ACTR 
   SET LCTNS_ID = '00003'
     , ACTR_BRTH_DT = TO_DATE('2000-12-01', 'YYYY-MM-DD')
 WHERE ACTR_ID = '00003'
;

COMMIT;

INSERT INTO GNR
 (GNR_ID
, GNR_NM)
VALUES
 (LPAD(GNR_PK_SEQ.NEXTVAL, 5, '0')
, '액션')
;
COMMIT;

INSERT INTO MV_GNR
 (MV_GNR_ID
, GNR_ID
, MV_ID)
VALUES
 (LPAD(MV_GNR_PK_SEQ.NEXTVAL, 5, '0')
, LPAD(MV_GNR_PK_SEQ.NEXTVAL, 5, '0')
, 3)
;
SELECT M.MV_TTL 
     , G.GNR_NM
  FROM MV_INFO M
 INNER JOIN MV_GNR MG
    ON M.MV_ID = MG.MV_ID
 INNER JOIN GNR G
    ON MG.GNR_ID = G.GNR_ID 
;
COMMIT;

-- ROW 삭제

DELETE
  FROM MV_GNR
 WHERE MV_GNR_ID = '00002'
;

ROLLBACK;


SELECT M.MV_ID 
     , M.MV_TTL
     , TO_CHAR(M.OPL_DT, 'YYYY-MM-DD') AS OPN_DT
     , M.SCRNG_TM
     , M.PLT 
     , G.GNR_NM
  FROM MV_INFO M
 INNER JOIN MV_GNR MG
    ON M.MV_ID = MG.MV_ID
 INNER JOIN GNR G
    ON MG.GNR_ID = G.GNR_ID
 INNER JOIN AGE_LMT AL
    ON M.AGE_LMT_ID = AL.AGE_LMT_ID
;
```

```

TRANSACTION
DCL -> ROW 수정, 삽입, 삭제 확정 처리

DML -> SELECT, INSERT, UPDATE, DELETE -> transation 

DDL -> CREATE TABLE, DROP TABLE, ALTER TABLE, ALTER COLUMN, ALTER USER

LOCAL 개발 및 테스트, 개발된 소스를 형상관리 툴에 업로드(GIT, SVN)

DEV서버(개발서버) WAS로 불리는 것 하나와 DB 서버가 들어가 있다. TEST에 문제 생길 시 LOCAL 개발과 TEST를 통해 해결한다. 
DEV서버에서 문제가 발생되지 않는 경우, Stage서버가 있다. 준상용 서버(약 천만건). 성능을 확인하는 것이다. 


ORACLE DB가 있다. 

은행이 있다. A와 B라는 사람이 있다. A는 통장계좌에 약 1억원의 돈이 있고 B는 통장계좌에 0원이 있다.
출금, 입금, 이체 기능이 있다. A가 B에게 5천만원을 이체한다고 가정한다. A의 통장에서 5천만은 빼주고 B의 통장에 5천만원을 더한다.
AUTO 지정하는 경우 입금 시 에러가 발생하면 A의 5천만원은 증발하게 된다. 이를 막기 위해 try ~ catch 문을 사용하여
예외 발생시 rollback을 정상 작동 시 commit을 한다.  


JDBC API
CONNECTION
STATEMENT
RESULT SET 
```
