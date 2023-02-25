# SQL

## SQL이란

SQL은 Structured Query Language(구조적 질의 언어)의 줄임말로 RDBMS(관계형 데이터베이스 시스템)을 제어하는 컴퓨터 언어를 뜻합니다. 저희가 잘 아는 파이썬, 자바와 같은 프로그래밍 언어보다는 그 명령문이 짧고 간결합니다.

RDBMS에는 여러 가지 종류가 있습니다.

* Oracle DB : 가장 오래되었고 신뢰도가 높습니다. 대규모 시스템에서 많이 쓰입니다.
* MySQL : 오픈 소스로 저희들이 이용할 RDBMS입니다.
* Maria DB : MySQL 5.5를 기반으로 만들어져 MySQL과 호환성이 뛰어납니다.
* PostgreSQL : 오픈 소스 ORDBMS(객체형 데이터베이스 관리 시스템) 입니다.

### 1. RDB 구조

관계형 데이터베이스는 키(key)와 값(value)의 관계를 2차원 테이블의 형식으로 나타낸 데이터베이스로, 하나의 데이터베이스 안에는 여러개의 테이블이 존재할 수 있습니다.

테이블은 행(row)와 열(column)으로 이루어져 있습니다. 테이블의 각 행은 레코드(record)라고 부릅니다. 각각의 데이터 한 건을 의미합니다. 테이블의 열(column)은 데이터프레임의 열이라고 생각하면 됩니다.

\[데이터베이스 - 테이블 - 열, 행]

### 2. SQL 명령어 분류

* DDL : 데이터베이스 스키마와 설명을 정의하는 언어입니다. 데이터베이스와 테이블을 생성하는 작업에 쓰입니다.
* DML : 데이터 검색, 삽입, 변경, 삭제를 수행하는 언어입니다.
* DCL : 데이터에 접근할 수 있는 권한을 관리하는 언어입니다.
* TCL : Transaction을 다루는 언어입니다.

### 3. 데이터 타입

가장 자주 쓰이는 데이터 4개만을 정리했습니다.

* VARCHAR() : 최대 255자의 문자를 저장합니다.
* INT() : 표준 Integer 값
* DATE : 날짜. ‘YYYY-MM-DD’ 형식
* TIME : 시간. ‘HH:MM:SS’ 형식

### 4. SQL 실습

1. Database 생성

```sql
CREATE DATBASE 데이터베이스명;
```

1. Database 접속

```sql
USE 데이터베이스명;
```

1. Database 삭제

```sql
DROP DATABASE 데이터베이스명;
```

1. Table 생성

```sql
CREATE TABLE Table1(
    id INT(20) NOT NULL AUTO_INCREMENT, 
    PRIMARY KEY(id),
		detail TEXT NULL
);

# NOT NULL : 필수 입력 사항. 공백 허용 하지 않겠다는 옵션
# NULL : 공백 허용 옵션

# AUTO_INCREMENT : 자료형이 INT(정수형)일때 적용 가능, 데이터가 늘어날 때마다 1씩 자동 증가

# PRIMARY KEY(컬럼명) : 중복 값 허용 안하는 컬럼 1개 선택. 테이블의 주요 축이 되는 컬럼/
```

1. Table 데이터 추가

```sql
INSERT INTO 테이블명 (컬럼1,컬럼2 ...) VALUES (값1, 값2 ...)
```

1. Table 데이터 확인

```sql
# 모든 데이터 확인
SELECT * FROM 테이블명;

# 해당하는 컬럼 데이터만 확인
SELECT (컬럼명) FROM 테이블명;
```

1. Table 데이터 정렬 (row 정렬)

```sql
# DESC : 내림차순 정렬
SELECT * FROM 테이블명 ORDER BY 컬럼명 DESC; 

# ASC : 오름차순 정렬
SELECT * FROM 테이블명 ORDER BY 컬럼명 ASC;
```

1. Table 데이터 수정

```sql
UPDATE 테이블명 SET detail='내용' WHERE id=1;
```

1. Table 이름 변경

```sql
RENAME TABLE 테이블명 TO 새로운테이블명;
```

1. Table 데이터 삭제

```sql
DELETE FROM 테이블명 WHERE id=1;
```

1. Table 삭제

```sql
DROP TABLE 테이블명;
```
