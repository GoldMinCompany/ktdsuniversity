# Oracle DataBase 설치
1. https://www.oracle.com/database/technologies/xe-downloads.html

위 링크에서 다운로드 후 zip 압축해제 후, setup.exe 실행시킨다. 설치가 안되는 경우 setup.exe 종료 후 재설치 한다. 

다운로드 완료 후 패스워드 입력을 입력한다. 패스워드 1234

# DBeaver 설치
2. https://dbeaver.io/download/
- DBeaver.io / DBeaver Community (Installer) download / password : 1234

DataBase 접속 프로그램으로 많이 사용되는 DBeaver를 다운로드한다. 구성요소 모두 다운로드.

Sample DataBase는 생성하지 않는다. 

# Setting

DBeaver 왼쪽 위 아이콘 클릭하고 port 번호는 1521, Database는 XE(교육용)으로 변경한다. 

Test Connect를 최초로 실행하고 설정을 끝마친다. 

생성된 localhost를 선택하고 F2를 눌러 oracle21cXE - system으로 이름을 변경한다. 

> Oracle HR schema 생성 후 왼쪽 후 아이콘을 클릭하여 "oracle21cXE - hr"을 생성(이름 변경)한다.

# DataBase

* DBMS는 대량의 data를 처리하기 위한 시스템이고 DB의 집합이다. 다수의 data가 관계를 맺어 관계형데이터베이스(RDBMS)라 칭한다. 
* DB는 대량의 data를 처리하기 위한 공간이고 DB는 다수의 Table로 구성되어있다.
* Table은 실제 data가 보관된 정형화된 구조의 저장소이다.


* column
  - 하나의 table은 하나 이상의 column으로 구성된다.
  - data를 저장할 수 있는 공간이다.
  - 각 column은 data type을 갖고 있다.
 
* rows
  - table의 data는 하나의 "행"으로 표현된다.
  - 각 행은 여러 개의 column으로 구성된다. 


* Keys
  - Primary Key(PK)
    - 하나의 테이블에서 절대 중복이 되지 않는 값
    - 하나의 row를 대표하는 값
    - table은 하나 이상의 Primary Key를 반드시 갖는다.
  - Foreign Key(FK)
    - Primary Key를 참조하는 키
    - 자신의 table 또는 다른 table의 Primary Key로 관계를 형성할 때 사용된다.
  - Unique Key
    - 하나의 table에서 절대 중복이 되지 않는 값이다.
    - PK와 다른 성격을 띄고 있고 PK는 row를 대표하는 값이다.
    - Unique Key는 한 table에 중복된 data를 생성할 수 없다.

* SQL(Structured Query Language)
  - database가 관리하는 data를 조작하는 언어
  - database, table, column 등을 생성(Create), 수정(Alter), 삭제(Drop)할 수 있다.
  - table 내의 row를 추가(C), 조회(R), 수정(U), 삭제(D)할 수 있다.
  - 위 나열된 항목들을 관리할 수 있도록 구조화된 언어체계를 갖고 있다. 
    
