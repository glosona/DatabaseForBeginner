# SECTION  01. SELECT ··· FROM 문
## 5-1. 개체의 이름을 정확히 모를 때 데이터 검색하기
[시작]-[mYSQL 8.0 Command Line Client]
```sql
SHOW DATABASES;   # 현재 서버에 데이터 베이스 있는지 조회

USE employees;   # 데이터 베이스 지정

SHOW TABLES;   # 테이블 정보 조회

DESCRIBE employees;   # 열 조회

SELECT fist_name, gender FROM employees LIMIT 10;   # 원하는 열 조회
```
***
# SECTION  02. SELECT ··· FROM ··· WHERE 문
## 5-2. cookDB 생성하고 저장하기
```sql
DROP DATABASE IF EXISTS cookDB; -- 만약 cookDB가 존재하면 우선 삭제한다
CREATE DATABASE cookDB;   # cookDB 생성

# 회원 테이블과 구매 테이블 생성
USE cookDB;
CREATE TABLE userTBL -- 회원 테이블
( userID CHAR(8) NOT NULL PRIMARY KEY, -- 사용자 아이디(PK)
  userName VARCHAR(10) NOT NULL, -- 이름
  birthYear INT NOT NULL, -- 출생 연도
  addr CHAR(2) NOT NULL, -- 지역(경기, 서울, 경남 식으로 2글자만 입력)
  mobile1 CHAR(3), -- 휴대폰의 국번(011, 016, 017, 018, 019, 010 등)
  mobile2 CHAR(8), -- 휴대폰의 나머지 번호(하이픈 제외)
  height SMALLINT, -- 키
  mDate DATE -- 회원 가입일
);
CREATE TABLE buyTBL -- 구매 테이블
( num INT AUTO_INCREMENT NOT NULL PRIMARY KEY, -- 순번(PK)
  userID CHAR(8) NOT NULL, -- 아이디(FK)
  prodName CHAR(6) NOT NULL, -- 물품
  groupName CHAR(4), -- 분류
  price INT NOT NULL, -- 단가
  amount SMALLINT NOT NULL, -- 수량
  FOREIGN KEY (userID) REFERENCES userTBL (userID)
);

# 데이터 삽입
INSERT INTO userTBL VALUES('YJS', '유재석', 1972, '서울', '010', '11111111', 178, '2008-8-8');
INSERT INTO userTBL VALUES('KHD', '강호동', 1970, '경북', '011', '22222222', 182, '2007-7-7');
INSERT INTO userTBL VALUES('KKJ', '김국진', 1965, '서울', '019', '33333333', 171, '2009-9-9');
INSERT INTO userTBL VALUES('KYM', '김용만', 1967, '서울', '010', '44444444', 177, '2015-5-5');
INSERT INTO userTBL VALUES('KJD', '김제동', 1974, '경남', NULL , NULL      , 173, '2013-3-3');
INSERT INTO userTBL VALUES('NHS', '남희석', 1971, '충남', '016', '66666666', 180, '2017-4-4');
INSERT INTO userTBL VALUES('SDY', '신동엽', 1971, '경기', NULL , NULL      , 176, '2008-10-10');
INSERT INTO userTBL VALUES('LHJ', '이휘재', 1972, '경기', '011', '88888888', 180, '2006-4-4');
INSERT INTO userTBL VALUES('LKK', '이경규', 1960, '경남', '018', '99999999', 170, '2004-12-12');
INSERT INTO userTBL VALUES('PSH', '박수홍', 1970, '서울', '010', '00000000', 183, '2012-5-5');

INSERT INTO buyTBL VALUES(NULL, 'KHD', '운동화', NULL   , 30,   2);
INSERT INTO buyTBL VALUES(NULL, 'KHD', '노트북', '전자', 1000, 1);
INSERT INTO buyTBL VALUES(NULL, 'KYM', '모니터', '전자', 200,  1);
INSERT INTO buyTBL VALUES(NULL, 'PSH', '모니터', '전자', 200,  5);
INSERT INTO buyTBL VALUES(NULL, 'KHD', '청바지', '의류', 50,   3);
INSERT INTO buyTBL VALUES(NULL, 'PSH', '메모리', '전자', 80,  10);
INSERT INTO buyTBL VALUES(NULL, 'KJD', '책'    , '서적', 15,   5);
INSERT INTO buyTBL VALUES(NULL, 'LHJ', '책'    , '서적', 15,   2);
INSERT INTO buyTBL VALUES(NULL, 'LHJ', '청바지', '의류', 50,   1);
INSERT INTO buyTBL VALUES(NULL, 'PSH', '운동화', NULL   , 30,   2);
INSERT INTO buyTBL VALUES(NULL, 'LHJ', '책'    , '서적', 15,   1);
INSERT INTO buyTBL VALUES(NULL, 'PSH', '운동화', NULL   , 30,   2);

# 삽입된 데이터 확인
SELECT * FROM userTBL;
SELECT * FROM buyTBL;
```
### WHERE 절
```sql
SELECT 열이름 FROM 테이블이름 WHERE 조건식;
```
### 조건 연산자와 관계 연산자
* WEHRE 절에 (=, <, >, <=, >=, <>, !=), (NOT, AND, OR) 사용 가능
```sql
SELECT userID, userName FROM userTBL WHERE birthYear >= 1970 AND height >= 182;
```
### BETWEEN ··· AND, IN(), LIKE 연산자
* 숫자일 경우 BETWEEN ··· AND 연산자 사용 가능
```sql
SELECT userName, height FROM userTBL WHERE height >= 180 AND height <= 182;

SELECT userName, height FROM userTBL WHERE height BETWEEN 180 AND 182;
```
* 이산적인 값 조회 시에는 IN() 연산자 사용
```sql
SELECT userName, addr FROM userTBL WHERE addr='경남' OR addr='충남' OR addr='경북';

SELECT userName, addr FROM userTBL WHERE addr IN ('경남', '충남', '경북');
```
* 문자열 내용 검색 시 LIKE 연산자 사용
```sql
SELECT userName, height FROM userTBL WHERE userName LIKE '김%';   # 김씨 회원 조회

SELECT userName, height FROM userTBL WHERE userName LIKE '_경규';   # 첫 글자는 아무거나 

SELECT userName, height FROM userTBL WHERE userName LIKE '_경%';   # 첫 글자는 아무거나, 경 다음 글자는 수도 상관 x
```
  1. % -> 무엇이든 허용
  2. 한 글자만 매치하려면 '_ ' 사용
 
### 서브쿼리와 ANY, ALL, SOME 연산자
* 쿼리문 안에 또 쿼리문이 들어있는 것이 서브쿼리
```sql
SELECT userName, height FROM userTBL WHERE height > 177;   # 김용만의 키를 직접 넣은 것

SELECT userName, height FROM userTBL
  WHERE height > (SELECT height FROM userTBL WHERE userName = '김용만');   # 서브쿼리 활용
```
* 반환 값 2개 중 아무거나 쓸 때는 ANY(=SOME) 사용
```sql
SELECT userName, height FROM userTBL
	WHERE height >= ANY (SELECT height FROM userTBL WHERE addr = '경기');
```
* 반환 값 모두 사용할 때는 ALL 사용
```sql
SELECT userName, height FROM userTBL
	WHERE height >= ALL (SELECT height FROM userTBL WHERE addr = '경기');
```
* '= ANY (서브쿼리)' == 'IN (서브쿼리)'
```sql
SELECT userName, height FROM userTBL
	WHERE height = ANY (SELECT height FROM userTBL WHERE addr = '경기');
  
SELECT userName, height FROM userTBL
  WHERE height IN (SELECT height FROM userTBL WHERE addr = '경기');
```

### ORDER BY 절
WEHRE 절과 같이 사용해도 됨. 항상 뒤에 와야 함.
* ORDER BY: 오름차순
```sql
SELECT userName, mDate FROM userTBL ORDER BY mDate;
```
* 열 이름 뒤에 DESC: 내림차순
```sql
SELECT userName, mDate FROM userTBL ORDER BY mDate;
```
* 키 순대로 정렬, 같을 경우 이름 순으로 정렬
```sql
SELECT userName, mDate FROM userTBL ORDER BY mDate DESC;
```

### DISTINCT 키워드
* 중복이 하나만 출력됨
```sql
SELECT addr FROM userTBL ORDER BY addr;  # 중복

SELECT DISTINCT addr FROM userTBL;   # 하나만
```

### LIMIT 절
