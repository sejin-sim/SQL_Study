# SQL Tutorials (`MySQL` 기준 작성)

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
- `Surrogate Key` : 개체의 실제 속성은 아니지만 `Primary Key로 쓰기 위해 추가한 컬럼` (주로 1부터 순차적으로 증가하는 숫자)   
 ㄴ Auto Increment(AL) = 이전보다 1이 더 큰 정수를 자동으로 넣어주는 기능

## NOT NULL의 의미
- `NOT NULL` = NULL이 아니다. (NULL=값이 존재하지 않는 상태) = primary key는 `NN`이어야 한다.


## 데이터 조회로 기본 다지기
### SELECT : 테이블의 데이터를 조회할 때 사용하는 구문
```mysql
SELECT * FROM copang_main.member;     # "*" 는 asterisk로 all coulumn
``` 
```mysql
SELECT email, age, address FROM copang_main.member;     # "FROM"은 ~로 부터
```

### WHERE : 조건을 달아 찾을 때 쓰는 구문
```mysql
SELECT * FROM copang_main.member where EMAIL = 'taehos@hanmail.net'; 
```
```mysql
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
```mysql
SELECT * FROM copang_main.member where year(birthday) = '1992';         #  1992년에 태어난 회원만 조회
SELECT * FROM copang_main.member WHERE MONTH(sign_up_day) IN (6,7,8);   # 여름(6, 7, 8월)에 가입한 회원들만 조회하기
SELECT * FROM copang_main.member WHERE DAYOFMONTH(sign_up_day) BETWEEN 15 AND 31;  # 각 달의 후반부(15일~31일)에 가입했던 회원들만 조회하기
```

### 날짜 간의 차이 구하기
```mysql
SELECT email, sign_up_day, DATEDIFF(sign_up_day, '2019-01-01') FROM copang_main.member; # 2019년 1월 1일~sign_up_day 차이 일 수
SELECT email, sign_up_day, CURDATE(), DATEDIFF(sign_up_day, CURDATE()) FROM copang_main.member; # 오늘 날짜 기준
SELECT email, sign_up_day, DATEDIFF(sign_up_day, birthday) /365 FROM copang_main.member;        # 몇살이었을 때 가입?
```

### 날짜 더하기 빼기 
```mysql
SELECT email, sign_up_day, date_add(sign_up_day, INTERVAL 300 DAY) FROM copang_main.member;  # 가입일(sign_up_day) 기준으로 300일 이후의 날짜
SELECT email, sign_up_day, DATE_SUB(sign_up_day, INTERVAL 250 DAY) FROM copang_main.member;  # 가입일(sign_up_day) 기준 250일 이전의 날짜
```

### UNIX Timestamp 값 
```mysql
SELECT email, sign_up_day, from_unixtime(UNIX_TIMESTAMP(sign_up_day)) FROM copang_main.member;  # DATE 타입의 값을 Unix Timestamp로 바꿔줌
```

## 여러 개의 조건 걸기
```mysql
SELECT * FROM copang_main.member
WHERE gender = 'm'	AND address LIKE '서울%' AND age BETWEEN 25 and 29;
```
```mysql
SELECT * FROM copang_main.member
WHERE (gender = 'm' AND height >= 180) OR (gender = 'f' AND height >= 170)
```
### 여러 조건을 걸 때 주의할 점
1. OR를 사용할 때의 주의사항 - WHERE id = 1 OR id = 2 (O)
2. AND와 OR 간의 우선순위 : AND가 우선 순위 이므로, 괄호를 꼭 쳐주자

## 문자열 패턴 매칭 조건을 사용할 때 주의할 점
1. 이스케이핑(escaping) 문제 : \ 후 기재
2. 대소문자 구분 문제 :  BINARY '%g%'     소문자만 조회

## 데이터 정렬해서 보기
```mysql
SELECT * FROM sales ORDER BY YEAR(sign_up_day) DESC, email ASC; # 이름을 먼저 쓴 컬럼을 우선으로 해서 정렬이 차례대로 수행
SELECT * FROM sales ORDER BY CAST(registration_num AS signed);  # 일시적으로 다른 데이터 타입으로 변경할 수 있게 해주는 함수
```

## 데이터의 일부만 추려보기
```mysql
SELECT * FROM db.search_result ~ ORDER BY registration_date DESC LIMIT 0, 10
```

## 데이터의 특성 구하기
### 집계 함수 : 여러 row의 값들을 동시에 고려해서 실행되는 함수
```mysql
SELECT COUNT(email) FROM copang_main.member; # 갯수
SELECT MAX(height) FROM copang_main.member;  # 최대 값
SELECT MIN(weight) FROM copang_main.member;  # 최소 값
SELECT AVG(weight) FROM copang_main.member;  # 평균 값
SELECT SUM(weight) FROM copang_main.member;  # 합계
SELECT STD(weight) FROM copang_main.member;  # 표준편차
```

### 산술 함수 : 각 row의 값마다 실행되는 함수
```mysql
SELECT ABS(weight) FROM copang_main.member;    # 절대값
SELECT SQRT(weight) FROM copang_main.member;   # 제곱근
SELECT CEIL(weight) FROM copang_main.member;   # 올림
SELECT FLOOR(weight) FROM copang_main.member;  # 내림 
SELECT ROUND(weight) FROM copang_main.member;  # 반올림 
```

## NULL 다루는 방법
```mysql
SELECT * FROM copang_main.member WHERE address IS NULL;    # NULL 값 조회
SELECT * FROM copang_main.member WHERE address IS not NULL;# NULL 아닌 값 조회
```
```mysql
WHERE height IS NULL	OR weight IS NULL	OR address IS NULL;
SELECT 	COALESCE(height, '####'), COALESCE(weight, '---'), COALESCE(address, '@@@')
FROM copang_main.member;  # COALESCE 는 결합한다는 뜻으로, NULL인 값을 채워넣어준다.
```

## 이상한 값을 제외하고 싶다면?
```mysql
SELECT AVG(age) FROM copang_main.member WHERE age between 5 and 100;
SELECT * FROM copang_main.member WHERE address NOT LIKE '%호';
SELECT * FROM copang_main.member WHERE address LIKE '%호';
```

## 컬럼의 값 변환해서 보기
```mysql
SELECT 
    email,
    CONCAT(height, 'cm', ',', weight, 'kg') AS '키와 몸무게',
    weight / ((height/100) * (height/100)) AS BMI,
  (CASE
     WHEN weight IS NULL OR height IS NULL THEN '비만 여부 알 수 없음'
     WHEN weight/((height/100) * (height/100)) >= 25 THEN '과체중 또는 비만'
     WHEN weight/((height/100) * (height/100)) >= 18.5 
          AND weight/((height/100) * (height/100)) < 25 THEN '정상'
     ELSE '저체중'
  END) AS obesity_check
FROM copang_main.member;
```

## NULL을 다른 값으로 변환하는 다양한 함수
### COALESCE 함수 
```mysql
SELECT COALESCE(height, 'N/A') FROM copang_main.member;
SELECT COALESCE(height, weight * 2.3, 'N/A') FROM copang_main.member; # NULL이면 weight *2.3, 그래도 NULL이면 'N/A'
```
### IFNULL 함수
```mysql
SELECT IFNULL(height, 'N/A') FROM copang_main.member;   IF NULL이면, 'N/A'로 표시
```

### IF 함수
```mysql
SELECT IF(height IS NOT NULL, height, 'N/A') FROM copang_main.member; # height가 not null이면,  height 반환 아니면 'N/A'
```

### CASE 함수 
```mysql
SELECT CASE WHEN height IS NOT NULL THEN height ELSE 'N/A'	END FROM copang_main.member;
```


### 고유값만 보기
```mysql
SELECT DISTINCT(gender) FROM copang_main.member;
SELECT DISTINCT(SUBSTRING(address, 1, 2)) FROM copang_main.member; # 첫번째 시작 2개만큼 
```

### 문자열 관련 함수들
```mysql
SELECT *, length(address) FROM copang_main.member;
SELECT email, upper(email), lower(email)  FROM copang_main.member;
SELECT email, LPAD(age, 10, '0'), RPAD(age, 10, '!') FROM copang_main.member;
```

### 그루핑해서 보기
```mysql
SELECT	SUBSTRING(address, 1,2) AS region, gender, COUNT(*)
FROM copang_main.member 
GROUP BY substring(address, 1, 2), gender
WITH ROLLUP HAVING region IS NOT NULL
ORDER BY region ASC, gender DESC;
```

## SELECT 문의 입력 및 실행 순서
### 입력 순서
SELECT - FROM - WHERE - GROUP BY - HAVING - ORDER BY - LIMIT
### 실행 순서
1. FROM : 어느 테이블을 대상으로 할 것인지를 먼저 결정합니다. 
2. WHERE : 해당 테이블에서 특정 조건(들)을 만족하는 row들만 선별합니다. 
3. GROUP BY : row들을 그루핑 기준대로 그루핑합니다. 하나의 그룹은 하나의 row로 표현됩니다.
4. HAVING : 그루핑 작업 후 생성된 여러 그룹들 중에서, 특정 조건(들)을 만족하는 그룹들만 선별합니다. 
5. SELECT : 모든 컬럼 또는 특정 컬럼들을 조회합니다. SELECT 절에서 컬럼 이름에 alias를 붙인 게 있다면, 이 이후 단계(ORDER BY, LIMIT)부터는 해당 alias를 사용할 수 있습니다.
6. ORDER BY : 각 row를 특정 기준에 따라서 정렬합니다. 
7. LIMIT : 이전 단계까지 조회된 row들 중 일부 row들만을 추립니다. 


## 테이블 간의 연결 고리
`Foreign Key` :  '다른 테이블의 특정 row를 식별할 수 있게 해주는 컬럼'   
ㄴ stock table 에서 item의 item_id 를 referenced Table로 지정해 준다.

## 다른 종류의 테이블 조인하기
```mysql
SELECT i.id,
       i.name,
       s.item_id,
       s.inventory_count
# FROM item AS i RIGHT OUTER JOIN stock # 오른쪽 기준 조회
# FROM item LEFT OUTER JOIN stock       # 왼쪽 기준 조회
FROM item AS i INNER JOIN stock AS s    # 둘다 있는 것 조회
ON i.id = s.item_id;
```

## 결합 연산과 집합 연산
```mysql
SELECT * FROM member_A 
INTERSECT    #  A ∩ B (INTERSECT 연산자 사용)
MINUS        #  A - B (MINUS 연산자 또는 EXCEPT 연산자 사용)
UNION        #  A U B (UNION 연산자 사용) OR UNION ALL (열이 생략되는 경우도 있기 때문)
SELECT * FROM member_B
```

## ON 대신 USING을 쓸 수도 있어요
ON old.id = new.id 와 USING(id)의 의미는 같다.

## 다른 3개의 테이블 조인해 의미있는 데이터 추출하기
```mysql
SELECT
    YEAR(i.registration_date) as '등록 연도',
    COUNT(*) as '리뷰 개수',
    AVG(r.star) as '별점 평균값'
FROM
    review as r
    INNER JOIN item as i
    ON r.item_id = i.id
    INNER JOIN member as m
    ON r.mem_id = m.id
WHERE i.gender = 'u'
GROUP BY YEAR(i.registration_date)
HAVING COUNT(*) >= 10
ORDER BY AVG(r.star) DESC;
```

## 다른 종류의 조인들
1. NATURAL JOIN -  같은 이름의 컬럼을 찾아서 자동으로 그것들을 조인 조건을 설정하고, INNER JOIN을 해주는 조인
2. CROSS JOIN - 두 테이블의 Cartesian Product를 구하는 조인
3. SELF JOIN -  테이블이 자기 자신과 조인을 하는 경우. 같은 테이블을 마치 별도의 테이블인 것처럼 간주하고 진행된다는 점
4. FULL OUTER JOIN -  LEFT OUTER JOIN 결과와 RIGHT OUTER JOIN 결과를 합치는 조인
5. Non-Equi JOIN -  동등 조건이 아닌 다른 종류의 조건

# 서브쿼리란? 하위 데이터베이스에 보내는 요청
```mysql
SELECT i.id, i.name, AVG(star) AS avg_star
FROM
    item as i LEFT OUTER JOIN review as r
    ON r.item_id = i.id
GROUP BY i.id, i.name
HAVING avg_star < (SELECT AVG(star) FROM review)     # 서브쿼리
ORDER BY avg_star DESC;
```
```mysql
SELECT * FROM item
WHERE id IN
(
SELECT item_id
FROM review
GROUP BY item_id HAVING COUNT(*) >=3  # WHERE 절에 있는 서브쿼리
);
```

## ANY(SOME), ALL
1. ANY=SOME의 의미 - 하나라도 조건을 만족하는 경우가 있으면 True를 리턴한다
2. ALL의 의미 - 모든 경우에 대해서 해당 조건이 성립해야 True를 리턴

## FROM 절에 있는 서브쿼리
```mysql
SELECT AVG(review_count)
FROM 
(SELECT SUBSTRING(address, 1, 2) AS region,
	COUNT(*) AS review_count
 FROM review AS r LEFT OUTER JOIN member AS m
 ON r.mem_id = m.id
 GROUP BY SUBSTRING(address, 1, 2)
HAVING region IS NOT NULL
AND region != '안드') AS review_count_summary;
```
