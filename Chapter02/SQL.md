# SECTION 02. 데이터베이스 구축    
## 2-4 SQL 문 작성하기     
```sql
SELECT * FROM memberTBL;   # 모든 데이터 조회

SELECT memberName, memberAddres, FROM memberTBL;   # 특정 열 조회

SELECT * FROM memberTBL WHERE memberName = '토마스';   # '토마스'에 대한 정보만 추출

CREATE TABLE `my testTBL` (id INT);   # 새로운 테이블 생성

DROP TABLE `my TestTBL`;   # 테이블 삭제
```
***

# SECTION 03. 데이터베이스 개체 활용

## 2-5 인덱스 사용하기
```sql
CREATE TABLE indexTBL (first_name varchar(14), last_name varchar(16), hire_date date);
INSERT INTO indexTBL
  SELECT first_name, last_name, hire_date
  FROM employees.employees
  LIMIT 500;
SELECT * FROM indexTBL;   # 적정량의 데이터가 있는 테이블 생성 

SELECT * FROM indexTBL WHERE first_name = 'Mary';   # 인덱스 없이 쿼리 작동 확인

CREATE INDEX idx_indexTBL_firstname ON indexTBL(first_name);   # 인덱스 생성
SELECT * FROM indexTBL WHERE first_name = 'Mary';   # 인덱스 생성 후 쿼리 작동 확인
```

## 2-6 기본적인 뷰 사용법 알아보기
```sql
CREATE VIEW uv_memberTBL
AS
  SELECT memberName, memberAddress FROM memberTBL;   # 뷰 생성하기
 
SELECT * FROM uv_memberTBL;   # 뷰 조회하기
```

## 2-7 간단한 스토어드 프로시저 만들기
```sql
SELECT * FROM memberTBL WHERE memberName = '토마스';
SELECT * FROM productTBL WHERE productName = '냉장고';

DELIMITER //
CREATE PROCEDURE myProc()
BEGIN
	SELECT * FROM memberTBL WHERE memberName = '토마스';
    SELECT * FROM productTBL WHERE productName = '냉장고';
END //
DELIMITER ;   # 2개의 쿼리를 하나의 스토어드 프로시저로 만들기

CALL myProc();   # 스토어드 프로시저 실행


```

## 2-8 가장 일바적으로 사용되는 트리거의 용도 알아보기
```sql
INSERT INTO memberTBL VALUES ('Soccer', '흥민', '서울시 서대문구 북가좌동');   # 데이터 삽입
SELECT * FROM memberTBL;

UPDATE memberTBL SET memberAddress = '서울 강남구 역삼동' WHERE memberName = '흥민';   # 데이터 수정
SELECT * FROM memberTBL;

DELETE FROM memberTBL WHERE memberName = '흥민';   # 데이터 삭제
SELECT * FROM memberTBL;

CREATE TABLE deletedMemberTBL
( memberID char(8),
  memberName char(5),
  memberAddress char(20),
  deletedDate date -- 삭제한 날짜
);   # 삭제된 데이터 보관할 테이블 생성

DELIMITER //
CREATE TRIGGER trg_deletedMemberTBL -- 트리거 이름
	AFTER DELETE -- 삭제 후에 작동하게 지정
    ON memberTBL -- 트리거를 부착할 테이블
    FOR EACH ROW -- 각 행마다 적용
BEGIN
	-- OLD 테이블의 내용을 백업 테이블에 삽입
    INSERT INTO deletedMemberTBL
		VALUES (OLD.memberID, OLD.memberName, OLD.memberAddress, CUREDATE());
END //
DELIMITER ;   # 삭제 작업 일어나면 백업 테이블에 삭제된 데이터 기록되는 트리거 생성

SELECT * FROM memberTBL;   # 회원 테이블에 데이터 4건 있는지 확인

INSERT INTO memberTBL VALUES ('Soccer', '흥민', '서울시 서대문구 북가좌동');
DELETE FROM memberTBL WHERE memberName = '흥민';   # '흥민' 삽입 후 바로 삭제

SELECT * FROM memberTBL;   # 삭제 확인

SELECT * FROM deletedMemberTBL;   # 백업 테이블 확인
```
