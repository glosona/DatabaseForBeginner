# SECTION 02. 데이터베이스 구축
## 2-4 SQL 문 작성하기
```python
SELECT * FROM memberTBL;   # 모든 데이터 조회

SELECT memberName, memberAddres, FROM memberTBL;   // 특정 열 조회

SELECT * FROM memberTBL WHERE memberName = '토마스';   // '토마스'에 대한 정보만 추출

CREATE TABLE `my testTBL` (id INT);   // 새로운 테이블 생성

DROP TABLE `my TestTBL`;   // 테이블 삭제
```

# SECTION 03. 데이터베이스 개체 활용

## 2-5 인덱스 사용하기
```python
CREATE TABLE indexTBL (first_name varchar(14), last_name varchar(16), hire_date date);
INSERT INTO indexTBL
  SELECT first_name, last_name, hire_date
  FROM employees.employees
  LIMIT 500;
SELECT * FROM indexTBL;   // 적정량의 데이터가 있는 테이블 생성 

SELECT * FROM indexTBL WHERE first_name = 'Mary';   // 인덱스 없이 쿼리 작동 확인

CREATE INDEX idx_indexTBL_firstname ON indexTBL(first_name);   // 인덱스 생성
SELECT * FROM indexTBL WHERE first_name = 'Mary';   // 인덱스 생성 후 쿼리 작동 확인
```

## 2-6 기본적인 뷰 사용법 알아보기
```python
CREATE VIEW uv_memberTBL
AS
  SELECT memberName, memberAddress FROM memberTBL;   // 뷰 생성하기
 
SELECT * FROM uv_memberTBL;   // 뷰 조회하기
```

## 2-7 간단한 스토어드 프로시저 만들기
```python
SELECT * FROM memberTBL WHERE memberName = '토마스';
SELECT * FROM productTBL WHERE productName = '냉장고';



```
