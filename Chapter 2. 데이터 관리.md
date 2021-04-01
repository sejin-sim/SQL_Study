# Chapter 2. 데이터 관리

## 데이터베이스 생성하기
```mysql
CREATE DATABASE IF NOT EXISTS course_rating;
USE course_rating;
```

## 컬럼의 데이터 타입
### Numeric types(숫자형 타입)
- 정수형 타입 
	* TINYINT SIGNED : -128 ~ 127 
	* TINYINT UNSIGNED : 0 ~ 255
	* SMALLINT SIGNED : -32768 ~ 32767 
	* SMALLINT UNSIGNED : 0 ~ 65535
	* MEDIUMINT SIGNED : -8388608 ~ 8388607
	* MEDIUMINT UNSIGNED : 0 ~ 16777215
	* INT SIGNED : -2147483648 ~ 2147483647
	* INT UNSIGNED : 0 ~ 4294967295
	* BIGINT SIGNED : -9223372036854775808 ~ 9223372036854775807
	* BIGINT UNSIGNED : 0 ~ 18446744073709551615

- 실수형 타입
	* DECIMAL(M, D) : M은 최대로 쓸 수 있는 전체 숫자의 자리수, D는 최대로 쓸 수 있는 소수점 뒤에 있는 자리의 수   
	 ㄴ ex) DECIMAL(5, 2) : -999.99 부터 999.99 까지의 실수. M은 최대 65, D는 30까지의 값을 가질 수 있다. 
	* FLOAT : -3.402823466E+38 ~ -1.175494351E-38, 0, 1.175494351E-38 ~ 3.402823466E+38
	* DOUBLE : -1.7976931348623157E+308 ~ -2.2250738585072014E-308, 0, 2.2250738585072014E-308 ~ 1.7976931348623157E+308

### 날짜 및 시간 타입(Date and Time Types)
* DATE : ’2020-03-26’ 연, 월, 일 순
* DATETIME : ’2020-03-26 09:30:27’ 연, 월, 일, 시, 분, 초
* TIMESTAMP : ’2020-03-26 09:30:27’ 연, 월, 일, 시, 분, 초   
 ㄴ DATETIME와 다른 점 : 타임 존(time_zone) 정보도 함께 저장
* TIME : ’09:27:31’ 시:분:초

### 문자열 타입(String type) 
* CHAR : 0 ~ 255자
* VARCHAR : 0 ~ 65,535자, 가변형으로 값에 따라 저장 용량이 달라짐
* TEXT : 0 ~ 65,535자, CHAR형과는 내부 구현일부 차이 有

## 테이블 생성 및 row 추가하기
```mysql
CREATE TABLE `animal_info` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `type` VARCHAR(30) NOT NULL,
  `name` VARCHAR(10) NOT NULL,
  `age` TINYINT NOT NULL,
  `sex` CHAR(1) NOT NULL,
  `weight` DOUBLE NOT NULL,
  `feature` VARCHAR(500) NULL,
  `entry_date` DATE NOT NULL,
  PRIMARY KEY (`id`));

INSERT INTO animal_info (type, name, age, sex, weight, feature, entry_date) 
  VALUES ('사자', '리오', 8, 'm', 170.5, '상당히 날렵하고 성격이 유순한 편임', '2015-03-21');
INSERT INTO animal_info (type, name, age, sex, weight, feature, entry_date) 
  VALUES ('코끼리', '조이', 15, 'f', 3000, '새끼 때 무리에서 떨어져 길을 잃고 방황하다가 동물원에 들어와서 적응을 잘 마침', '2007-07-16');
INSERT INTO animal_info (type, name, age, sex, weight, feature, entry_date) 
  VALUES ('치타', '매튜', 20, 'm', 62, '나이가 노령이라 최근 활동량이 현저히 줄어든 모습이 보임', '2003-11-20');
```

## row 
### 갱신
```mysql
UPDATE student SET major = '멀티미디어학과', name = '차소원' WHERE id = 2;
```
### 삭제
```mysql
DELETE FROM student WHERE id=4;
```

## coulmn 
### 추가 
```mysql
ALTER TABLE Student ADD gender CHAR(1) NULL;
```

### 타입 변경
```mysql
ALTER TABLE student 
	MODIFY major INT,
	MODIFY isbn VARCHAR(50) UNIQUE NOT NULL;
```

### 이름 변경
```mysql
ALTER TABLE student
	RENAME COLUMN student_number TO registration_number;
	CHANGE kind genre_code INT NOT NULL;
```

### 삭제 
```mysql
ALTER TABLE student
	DROP column admission_date;
```

### DEFAULT 속성 주기
```mysql
ALTER TABLE student 
	MODIFY major INT NOT NULL DEFAULT 101,
	MODIFY location VARCHAR(10) NOT NULL DEFAULT 'warehouse';
```

## DATETIME, TIMESTAMP 타입의 컬럼에 값을 넣는 2가지 방식
### NOW() 함수 사용하기 
```mysql
UPDATE post
	SET content = '오늘 기분이 좋아요', 
		recent_modified_time = NOW()
	WHERE id = 1;
 ```
 
 ### 속성 설정
 - DEFAULT CURRENT_TIMESTAMP : 새 row 추가 시 현재 시간으로 자동 설정
 - ON UPDATE CURRENT_TIMESTAMP : 기존 row에서 단 하나의 컬럼이라도 갱신되면 갱신될 때의 시간 자동 갱신

## Unique와 Primary Key의 차이
 - Primary Key는 NULL을 가질 수 없지만, Unique는 NULL을 허용

## Table
### CONSTRAINT 걸기
```mysql
ALTER TABLE student
	ADD CONSTRAINT st_rule CHECK (registration_number < 30000000),
	ADD CONSTRAINT page_rule CHECK (page > 0);
	
INSERT INTO student (name, registration_number)
	VALUES ('이대위', 30000000); # st_rule로 실행 불가

ALTER TABLE student DROP CONSTRAINT st_rule;
```

### 복사본 만들기, 삭제
```mysql
CREATE TABLE copy_of_undergradute AS SELECT * from undergraduate;

DROP TABLE copy_of_undergradute;
```

### 컬럼 구조만 복사하기
```mysql
CREATE TABLE copy_of_undergradute LIKE undergraduate;

INSERT INTO copy_of_undergraduate
	SELECT * FROM undergraduate WHERE major = 101; # 조건걸어서 데이터 삽입
```

### 모든 데이터 삭제
```mysql
DELETE FROM final_exam_result;
TRUNCATE final_exam_result; 
```
