# SQL (`MySQL` 기준 작성)

## DBMS와 SQL
- 데이터베이스 : row & coulmn으로 이뤄진 table
- `DBMS` : 데이터베이스 관리시스템
- `SQL` : DBMS에 명령을 내리기 위해 사용하는 언어 (My SQL 사용예정)
- My SQL : 페이스북, 유튜브 등을 비롯한 유명한 서비스에서도 활발히 사용되고 있는 DBMS (오픈소스)

## DBMS와 서버-클라이언트 구조
- DBMS는 주요 구성요소 2종류의 프로그램 有 : client를 통해 server에 접속해 원하는 명령을 내린다.   
 1. `client(클라이언트)` : 사용자가 server에 접속해서 원하는 데이터베이스 관련 작업을 할 수 있도록, SQL을 입력할 수 있는 화면 등을 제공하는 프로그램   
 2. `server(서버)` : client로부터 SQL 문 등을 전달받아 데이터베이스 관련 작업을 직접 처리하는 프로그램
- MySQL에서 서버 프로그램 이름 = 클라이언트 프로그램 이름 = ‘mysqld’ → 보통 CLI 환경에서 사용하지만 GUI환경에서도 가능.
- Oracle이 공식적으로 제공하는 GUI환경 : MySQL Workbench
- sys : MySQL 서버의 성능 관련 정보들을 갖고있는 데이터베이스

## 데이터베이스 생성하기
- 데이터 베이스를 만들어 주는 구문 ``` create database copang_main```

## Primary key의 종류
- `primary key(기본키)` : 테이블에서 하나의 ROW를 고유하게 식별할 수 있도록 해주는 column
- `Natural Key` : 개체가 갖고 있는 `속성을 나타내는 컬럼이 Primary Key가 됐을 때` 
- Surrogate Key : 개체의 실제 속성은 아니지만 Primary Key로 쓰기 위해 추가한 컬럼 (주로 1부터 순차적으로 증가하는 숫자)   
 ㄴ Auto Increment(AL) = 이전보다 1이 더 큰 정수를 자동으로 넣어주는 기능

## NOT NULL의 의미
- NOT NULL = NULL이 아니다. (NULL=값이 존재하지 않는 상태) = primary key는 NN이어야 한다.

## 데이터 조회로 기본 다지기
### SELECT : 테이블의 데이터를 조회할 때 사용하는 구문
```
SELECT * FROM copang_main.member;     # "*" 는 asterisk로 all coulumn
``` 
```
SELECT email, age, address FROM copang_main.member;     # "FROM"은 ~로 부터
```

### WHERE : 조건을 달아 찾을 때 쓰는 구문
``` 
SELECT * FROM copang_main.member where EMAIL = 'taehos@hanmail.net'; 
```
```
SELECT * FROM copang_main.member where age >= 27;
SELECT * FROM copang_main.member where age BETWEEN 30 AND 39;        # 30, 39 포함 그 사이 숫자
SELECT * FROM copang_main.member where age NOT BETWEEN 30 AND 39;
SELECT * FROM copang_main.member where sign_up_day > '2019-01-01';
SELECT * FROM copang_main.member where address LIKE '서울%';         # LIKE  처럼
SELECT * FROM copang_main.member where address LIKE '%고양%';
SELECT * FROM copang_main.member where gender != 'm';                # != 같지 않음
SELECT * FROM copang_main.member where gender <> 'm';                # <> 같지 않음
SELECT * FROM copang_main.member where age in (20, 30);              # IN 이 중에 있는
SELECT * FROM copang_main.member where email LIKE 'c_____@%'         # LIKE 뒤의 언더바 하나=문자 하나
```
### 연도, 월, 일 추출하기
```
SELECT * FROM copang_main.member where year(birthday) = '1992';         #  1992년에 태어난 회원만 조회
SELECT * FROM copang_main.member WHERE MONTH(sign_up_day) IN (6,7,8);   # 여름(6, 7, 8월)에 가입한 회원들만 조회하기
SELECT * FROM copang_main.member WHERE DAYOFMONTH(sign_up_day) BETWEEN 15 AND 31;  # 각 달의 후반부(15일~31일)에 가입했던 회원들만 조회하기
```

