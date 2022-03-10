# 데이터베이스와 SQL 💾
## 데이터베이스 알아보기
<br/>

### 데이터베이스와 DBMS
- **데이터베이스** : 데이터의 집합

- **DBMS** : **데이터베이스**를 관리하고 운영하는 소프트웨어
    > 다양한 데이터가 저장되어 있는 **데이터베이스**는 여러 명의 사용자나 응용 프로그램과 **공유**하고 **동시접근**이 가능해야 함

- **SQL** : 구조화된 질의 언어
    > **DBMS**에 데이터를 **구축**, **관리**, **활용**하기 위해 사용되는 언어임

<br/>

### DBMS의 분류
- 계층형, 망형, **관계형**, 객체지향형, 객체관계형 등으로 분류됨

- **관계형** : 대부분의 DBMS에서 사용되는 형태, **RDBMS**라고 부름
    > **테이블**이라는 최소단위로 구성되며, 하나이상의 **행(row)** 과 **열(column)** 로 이루어져 있음

<br/>

### SQL
- 데이터베이스를 **조작**하는 언어

- 국제표준화기구에서 **표준 SQL**을 발표
    > 각 회사의 DBMS는 표준 SQL을 준수하되, 제품의 특성을 반영한 SQL을 사용함
    >> ORACLE - PL/SQL   
    >> SQL Server - T-SQL   
    >> MySQL - SQL   

<br/>
<br/>
<br/>
<br/>

---

<br>

## SQL 기본 문법

<br>

> 주석문 : ``-- 주석문, 띄어쓰기 주의``

### 데이터베이스와 테이블 생성과 삭제
- 데이터베이스 생성과 삭제
    - DB 생성
        ```sql
        CREATE DATABASE DB이름
        ```
    - DB이름과 같은 DB를 삭제
        ```sql
        DROP DATABASE DB이름
        ```
    - DB이름과 같은 DB가 있다면 삭제
        ```sql
        DROP DATABASE IF EXISTSDB이름
        ```
    - DB이름의 DB를 선택설정
        ```sql
        USE DB이름

        SELECT * FROM dbname.member; -- USE 사용 전

        USE dbname;
        SELECT * FROM member; -- USE 사용 후
        ```

- 테이블 생성과 삭제
    - 테이블 생성
        ```sql
        CREATE TABLE member -- 회원테이블
        (
            mem_id      CHAR(8) NOT NULL,
            mem_num     INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
            mem_name    VARCHAR(10) NOT NULL,
            mem_number  INT NOT NULL,
            addr        CHAR(2) NOT NULL,
            phone       CHAR(3),
            height      CHAR(8),
            FOREIGN KEY (mem_id) REFERENCES buy(mem_id)
        )
        ```

### 데이터 입력
- INSERT
    - 테이블에 데이터를 삽입
        ```sql
        INSERT INTO 테이블 [(열1, 열2, ...)] VALUES (값1, 값2, ...);

        INSERT INTO member VALUES('2013', NULL, '홍길동', 9, '서울', '02', '170');
        ```
    - 특정 열은 입력에서 제외 (제외한 행은 NULL 삽입)
        ```sql
        INSERT INTO member (mem_id, mem_name) VALUES('22', '홍길동');
        ```
    - 입력 순서를 변경
        ```sql
        INSERT INTO member (mem_name, mem_id) VALUES('홍길동', '22');
        ```
    - 다른 테이블의 데이터를 한 번에 입력
        ```sql
        INSERT INTO 테이블 [(열1, 열2, ...)] SELECT 문;
        ```

- AUTO_INCREMENT
    - 테이블 생성시 AUTO_INCREMENT로 지정하는 열은 반드시 PRIMARY KEY
        > 자동 증가하는 부분은 NULL로 지정하면 자동으로 채워짐
        ```sql
        INSERT INTO hongong VALUES (NULL, '길동', 25); -- 1
        INSERT INTO hongong VALUES (NULL, '둘리', 22); -- 2
        INSERT INTO hongong VALUES (NULL, '똘비', 21); -- 3

        INSERT INTO hongong VALUES (NULL, '길동', 25), (NULL, '둘리', 22), (NULL, '똘비', 21); -- 3줄입력을 1줄로 입력할 수 있음
        ```
    - 현재값을 확인하는 SQL
        ```sql
        SELECT LAST_INSERT_ID();
        ```
    - 시작 값을 변경
        ```sql
        ALTER TABLE hongong AUTO_INCREMENT=100;
        ```
    - 증가 값을 변경 (시스템 변수)
        ```sql
        SET @@auto_increment_increment=3;
        ```
        - 전체 시스템 변수의 종류 조회
            ```sql
            SHOW GLOBAL VARIABLES
            ```

### 데이터 조회
- DESC (Describe)
    - 테이블의 구조를 조회
        ```sql
        DESC 테이블이름
        ```

- SELECT
    - 테이블에서 원하는 데이터를 추출
        ```sql
        SELECT 열이름   
            FROM 테이블이름   
            WHERE 조건식   
            GROUP BY 열이름    
            HAVING 조건식    
            ORDER BY 열이름    
            LIMIT 숫자   
        ```

    - 원하는 열 추출
        ```sql
        SELECT mem_name FROM member; -- mem_name 열만 추출
        SELECT mem_name, addr FROM member; -- mem_name 열과 addr열 추출
        SELECT mem_name 이름, addr "집의 주소" FROM member; -- 별명사용, 공백은 묶음
        ```

- 조건을 추가하여 데이터 추출
    ```sql
    SELECT * FROM member WHERE mem_name='홍길동';
    SELECT * FROM member WHERE height <= 162 AND mem_number> 6;
    ```
    - 범위 검색
        ``` sql
        SELECT * FROM member WHERE height *>= 163 AND height <= 165;
        SELECT * FROM member WHERE height BETWEEN 163 AND 165;
        ```
    - 여러 문자를 검색
        ``` sql
        SELECT mem_name, addr FROM member WHERE addr='경기' OR addr='전남' OR addr='경남';
        SELECT mem_name, addr FROM member WHERE addr IN('경기', '전남', '경남');
        ```
    - 문자열의 일부 글자를 검색
        ```sql
        SELECT * FROM member WHERE mem_name LIKE '김%'  -- 첫글자가 김인 이름
        SELECT * FROM member WHERE mem_name LIKE '__환' -- 마지막 글자가 환인 이름
        ```
    - 중복된 결과를 제거
        ```sql
        SELECT DISTINCT addr FROM member;
        ```

- ORDER BY 절
    - 결과가 출력되는 순서를 세팅
        ```sql
        SELECT mem_id, mem_name FROM member ORDER BY height (ASC | DESC);
        ```
    - 정렬 기준을 여러 개 열로 지정할 수 있음
        ```sql
        SELECT mem_id, mem_name FROM member ORDER BY height DESC, mem_name ASC;
        ```
    - 출력의 개수를 제한
        ```sql
        SELECT * FROM member LIMIT 3; -- 기준없이 개수제한을 두는경우는 드뭄
        SELECT * FROM member ORDER BY height DESC LIMIT 3;
        ```
    
- GROUP BY 절
    - 그룹으로 묶어줌
        ```sql
        SELECT mem_id "회원 아이디", SUM(amount) "총 구매 개수" FROM buy GROUP BY mem_id;
        ```

    - 집계 함수
        함수명|설명
        :---|:---
        SUM()|합계를 구함
        AVG()|평균을 구함
        MIN()|최소값을 구함
        MAX()|최대값을 구함
        COUNT()|행의 개수를 셈
        COUNT(DISTINCT)|행의 개수를 셈(중복은 1개로 인정)
        ```sql
        SELECT mem_id "회원 아이디", SUM(price*amount) "총 구매 개수" FROM buy GROUP BY mem_id;

        SELECT AVG(amount) "평균 구매 개수" FROM buy;
        SELECT mem_id, AVG(amount) "평균 구매 개수" FROM buy GROUP BY mem_id;

        SELECT COUNT(*) FROM member;
        SELECT COUNT(phone) "연락처가 있는 회원" FROM member;
        ```

- Having 절
    - 집계 함수에 대해서 조건을 제한
        > WHERE도 조건을 제한하는 것이지만 집계함수를 사용할 수 없음
        ```sql
        SELECT mem_id "회원 아이디", SUM(price*amount) "총 구매 개수" FROM buy GROUP BY mem_id HAVINT SUM(price*amount) > 1000;
        ```

- 서브쿼리
    - SELECT 안에 또 다른 SELECT가 들어가는 것
        ```sql
        SELECT height FROM member WHERE mem_name='홍길동';
        SELECT mem_name, height FROM member WHERE height > 164;
        
        -- 위의 두 쿼리문을 합치면
        SELECT mem_name, height FROM member 
            WHERE height > (SELECT height FROM member WHERE mem_name='홍길동');
        ```

<br/>

### 데이터 수정
- UPDATE
    - 기존에 입력되어 있는 값을 수정
        ```sql
        UPDATE 테이블이름
            SET 열1=값1, 열2=값2, ...
            WHERE 조건;

        UPDATE city_popul
            SET city_name='서울'
            WHERE city_name='Seoul';
        ```

- WHERE 없는 UPDATE
    - 모든 행에 일괄적으로 수정 적용
        ```sql
        UPDATE city_popil
            SET population = population / 10000;
        ```

<br/>

### 데이터 삭제
- DELETE
    - 행 데이터 삭제
        ```sql
        DELETE FROM 테이블이름 WHERE 조건;

        DELETE FROM city_popul WHERE city_name LIKE 'New%' LIMIT 5;
        ```
- 대용량 테이블 삭제방법 3가지
    - DELETE : 가장 오랜 시간이 걸림, 테이블의 구조 유지
        ```sql
        DELETE FROM big_table;
        ```
    - DROP : 가장 빠름, 테이블 자체를 삭제
        ```sql
        DROP TABLE big_table;
        ```
    - TRUNCATE : 테이블의 구조는 유지하지만 속도가 매우 빠르나, WHERE절 사용 불가
        ```sql
        TRUNCATE TABLE big_table;
        ```

<br/>

---

<br>

## SQL 고급 문법

<br>

### MySQL의 데이터 형식
- 데이터 형식

    - 정수형
        데이터 형식|바이트|범위
        :---|:--:|:--:
        TINYINT|1|-128 ~ 127
        SMALLINT|2|-32,768 ~ 32,767
        INT|4|-2,147,483,648 ~ 2,147,483,647
        BIGINT|8|약 -900경 ~ 900경

        > UNSIGNED : 값의 범위를 0부터 지정하여 2배의 범위를 사용

    - 문자형
        데이터 형식|바이트
        :---|:--:
        CHAR(n)|1~255
        VARCHAR(n)|1~16,383

    - 대량의 데이터 형식
        형식|데이터 형식|바이트
        :--:|:---|:--:
        TEXT 형식|TEXT|1~65,536
        &nbsp;|LONGTEXT|1~4,294,967,295
        BLOB 형식|BLOB|1~65,536
        &nbsp;|LONGBLOB|1~4,294,967,295
        > TEXT 형식 : 소설이나 영화 대본과 같은 내용을 저장   
        > BLOB 형식 : 이미지, 동영상 등의 데이터(이진 데이터)를 저장 

    - 실수형
        데이터 형식|바이트|설명
        :---|:--:|:---
        FLOAT|4|소수점 아래 7자리까지 표현
        DOUBLE|8|소수점 아래 15자리까지 표현

    - 날짜형
        데이터 형식|바이트|설명
        :---|:--:|:---
        DATE|3|날짜만 저장, YYYY-MM-DD 형식으로 사용
        TIME|3|시간만 저장, HH:MM:SS 형식으로 사용
        DATETIME|8|날짜 및 시간을 저장, YYYY-MM-DD HH:MM:SS 형식으로 사용

    - 변수의 사용
        ```sql
        SET @변수이름 = 변수의 값 -- 변수의 선언 및 값 대입
        SELECT @변수이름; -- 변수의 값 출력
        ```
        > 변수는 워크벤치를 종료하면 사라지는 임시값

        ``` sql
            SET @count = 3;
            SELECT * FROM member ORDER BY height LIMIT @count; -- 문법오류

            -- 해결방법
            PREPARE mySQL FROM 'SELECT * FROM member ORDER BY height LIMIT ?';
            EXECUTE mySQL USING @count;
        ```

    - 데이터 형 변환
        - 명시적인 변환
            ```sql
            CAST ( 값 AS 데이터형식[(길이)] )
            CONVERT ( 값, 데이터형식[(길이)]  -- 형식은 다르지만 동일한 기능)

            SELECT CAST(AVG(price) AS SIGNED) '평균 가격' FROM buy;
            -- 또는
            SELECT CONVERT(AVG(price), SIGNED) '평균 가격' FROM buy;
            -- 함수 안에 올 수 있는 데이터 형식 : CHAR, SIGNED, UNSIGNED, DATE, TIME, DATETIME
            ```
        - 암시적인 변환
            ```sql
            SELECT '100' + '200'; -- 300
            SELECT CONCAT('100', '200'); -- 100200
            SELECT CONCAT(100, '200'); -- 100200
            SELECT 100 + '200'; -- 300
            ```

<br>

### 두 테이블을 묶는 조인

- 조인
    - 두 개의 테이블을 서로 묶어서 하나의 결과를 만들어 내는 것

- 내부 조인
    - 형식
        ```sql
        SELECT 열목록
            FROM 첫 번째 테이블
                INNER JOIN 두 번쨰 테이블 -- 그냥 JOIN으로 입력 가능
                ON 조인될 조건
            [WHERE 검색 조건];
        ```
    - 동일한 열 이름이 존재하면 테이블이름을 표기해야함
        ```sql
        SELECT *
            FROM buy
                INNER JOIN member
                ON buy.mem_id = member.mem_id -- 테이블이름 함께 표기
            WHERE buy.mem_id = 'GRL';
        ```
    - 테이블이름을 간결하게 표현
        ```sql
        SELECT B.mem_id, M.mem_name, B.prod_name, M.addr
            FROM buy B -- 테이블에 별칭을 붙임
                INNER JOIN member M
                ON B.mem_id = M.mem_id
        ```
    - 내부 조인은 두 테이블 모두에 있는 내용만 조인됨
        > 양쪽 중에 한 곳이라도 내용이 있을 때 조인하러면 외부 조인을 사용

- 외부 조인
    - 두 테이블을 조인할 때 필요한 내용이 한 쪽 테이블에만 있어도 결과를 추출가능
    - 형식
        ```sql
        SELECT 열목록
            FROM 첫 번째 테이블(LEFT 테이블)
                <LEFT | RIGHT | FULL> OUTER JOIN 두 번쨰 테이블(RIGHT 테이블)
                ON 조인될 조건
            [WHERE 검색 조건];
        ```
        > LEFT OUTER JOIN : 왼쪽 테이블의 내용은 모두 출력되어야 한다
        > RIGHT OUTER FOIN : 오른쪽 테이블의 내용은 모두 출력되어야 한다
        > FULL OUTER JOIN : 왼쪽이든 오른쪽이든 한쪽에 들어 있는 내용이면 출력되어야 한다

- 기타 조인
    - 상호 조인
        - 양쪽 테이블의 모든 행을 조인
            ```sql
            SELECT * 
                FROM buy
                    CROSS JOIN member;
            ```
        - 특징
            - ON 구문 사용할 수 없음
            - 주로 테스트를 위해 대용량의 데이터를 생성하는 용도로 사용됨
                ```sql
                CREATE TABLE cross_table
                    SELECT *
                        FROM sakila.actor
                            CROSS JOIN world.country;
                ```
    - 자체 조인
        - 하나의 테이블에 서로 다른 별칭을 붙여서 조인
            ```sql
            SELECT 열목록
                FROM 테이블 별칭A
                    INNER JOIN 테이블 별칭B
                    ON 조인될 조건
                [WHERE 검색 조건];
            ```

<br>

### SQL 프로그래밍

- IF 문
    

<br>