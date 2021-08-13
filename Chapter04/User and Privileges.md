# SECTION 02. 데이터베이스 사용자 관리
## 4-4. MySQL 사용자 생성하고 권한 부여하기
* director, ceo, staff 생성하고 각각 권한 부여하기
![db에 권한 부여](https://user-images.githubusercontent.com/80742177/129306523-11d07841-1f79-4f38-aa78-0986800e0b14.PNG)     

* director로 접속해서 권한 확인하기     
![director 권한 확인](https://user-images.githubusercontent.com/80742177/129306916-1c0db1f8-1fa1-41a3-b5f7-9daeb4efc796.PNG)     
```sql
CREATE DATABASE sampleDB;
DROP DATABASE sampleDB;   # 실행함
```

* ceo로 접속해서 권한 확인하기
![ceo 권한 확인](https://user-images.githubusercontent.com/80742177/129306945-e3438787-3846-4245-8898-e76ed07ed99b.PNG)
```sql
USE ShopDB;
SELECT * FROM membertbl;   # 실행함

DELETE FROM membertbl;   # 실행 안함 
```


* staff로 접속해서 권한 확인하기      
1       
![staff 권한 확인](https://user-images.githubusercontent.com/80742177/129308440-b31407a4-4ef0-4c6a-825f-e1105f67740e.PNG)      
2      
![staff 권한 확인2](https://user-images.githubusercontent.com/80742177/129308445-34a6fc55-e18e-4092-a123-76e0ba161d89.PNG)       
3       
![staff 권한 확인3](https://user-images.githubusercontent.com/80742177/129308446-d75c5a13-3242-4f70-9558-6a216a534076.PNG)     
```sql
USE ShopDB;
SELECT * FROM memberTBL;
DELETE FROM memberTBL WHERE memberID = 'Gorden';   # 실행함

DROP TABLE memberTBL;   # 실행 안함

USE employees;
SELECT * FROM employees;   # 실행함

```
