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
    > 여러 테이블에 분산되어 있는 데이터를 검색 시 테이블 간의 관계(join)을 이용하여 필요한 데이터 검색   
    > 중복 데이터를 최소화 시킴
    >> 같은 데이터가 여러 컬럼 또는 테이블에 존재할 경우 정규화를 통해 해결

<br/>

### SQL
- 데이터베이스를 **조작**하는 언어
    종류|문장|설명
    :---|:---|:---
    DML(Data Manipulation Language)|INSERT|DB 객체에 데이터를 입력
    &nbsp;|UPDATE|DB 객체에 데이터를 수정
    &nbsp;|DELETE|DB 객체에 데이터를 삭제
    &nbsp;|SELECT|DB 객체에서 데이터를 조회
    DDL(Data Definition Language)|CREATE|DB 객체를 생성
    &nbsp;|ALTER|기존에 존재하는 DB 객체를 수정
    &nbsp;|DROP|DB 객체를 삭제
    &nbsp;|RENAME|테이블의 이름을 변경
    DCL(Data Control Language)|GRANT|DB 객체에 권한을 부여
    &nbsp;|REVOKE|DB 객체 권한 취소
    TCL(Transaction Control Language)|COMMIT|실행한 Query 최종 적용
    &nbsp;|ROLLBACK|실행한 Query를 마지막 COMMIT 전으로 취소하여 데이터 복구


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

        -- 다국어 처리(utf8mb3) 생성
        CREATE DATABASE dbtest DEFAULT CHARACTER SET utf8mb3 COLLATE utf8mb3_general_ci;
        -- 이모지 문자까지 처리
        CREATE DATABASE dbtest DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
        ```
    - DB 변경
        ```sql
        ALTER DATABASE dbtest DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
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
    - CASE 문 사용
        ```sql
        SELECT employee_id, first_name,
            CASE WHEN salary > 15000 THEN '고액연봉'
                 WHEN salary > 8000 THEN '평균연봉'
                 ELSE '저액연봉'
            END "연봉등급"
        FROM employees;
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
    - 서브 쿼리는 비교 연산자의 오른쪽에 기술해야 하고 반드시 괄호로 감싸져 있어야 함
        ```sql
        SELECT height FROM member WHERE mem_name='홍길동';
        SELECT mem_name, height FROM member WHERE height > 164;
        
        -- 위의 두 쿼리문을 합치면
        SELECT mem_name, height FROM member 
            WHERE height > (SELECT height FROM member WHERE mem_name='홍길동');
        ```
    - 중첩 서브 쿼리 : WHERE 문에 작성하는 서브 쿼리
      - 단일 행
      - 복수(다중) 행
      - 다중 컬럼
    - 인라인 뷰 : FROM 문에 작성하는 서브 쿼리
    - 스칼라 서브 쿼리 : SELECT 문에 작성하는 서브 쿼리

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
        DECLARE 변수이름 변수의자료형 -- 스토어드 프로시저에서 변수의 선언
        SET @변수이름 = 변수의 값 -- 일반 SQL에서 변수의 선언 및 값 대입
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
    - 어느 테이블을 먼저 읽지를 경정함에 따라 처리할 작업량이 상당히 달라짐

- 내부 조인
    - 옵티마이저가 조인의 순서를 조절하여 최적화를 수행함
    - 형식
        ```sql
        SELECT 열목록
            FROM 첫 번째 테이블
                INNER JOIN 두 번쨰 테이블 -- 그냥 JOIN으로 입력 가능
                ON 조인될 조건
            [WHERE 검색 조건];

        SELECT 열목록
            FROM 첫 번째 테이블
                INNER JOIN 두 번쨰 테이블 -- 그냥 JOIN으로 입력 가능
                USING 공통열
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
    - 반드시 OUTER 테이블을 먼저 읽어야 하므로 옵티마이저가 조인 순서를 선택할 수 없음
    - 형식
        ```sql
        SELECT 열목록
            FROM 첫 번째 테이블(LEFT 테이블)
                <LEFT | RIGHT | FULL> OUTER JOIN 두 번쨰 테이블(RIGHT 테이블)
                ON 조인될 조건
            [WHERE 검색 조건];

        SELECT 열목록
            FROM 첫 번째 테이블(LEFT 테이블)
                <LEFT | RIGHT | FULL> OUTER JOIN 두 번쨰 테이블(RIGHT 테이블)
                USING 공통열
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

- 스토어드 프로시저
    - SQL에서 프로그래밍 기능이 필요할 때 사용하는 데이터베이스 개체
        ```sql
        DELIMITER $$
        CREATE PROCEDURE 스포어드_프로시저_이름()
        BEGIN
            -- SQL 프로그래밍 코딩
        END $$
        DELIMITER ;

        CALL 스토어드_프로시저_이름(); -- 스토어드 프로시저 실행
        ```

- IF 문
    ```sql
    IF 조건식 THEN
        SQL 문장들
    END IF;
    ```

- IF ELSE 문
    ```sql
    IF 조건식 THEN
        SQL 문장들
    ELSE
        SQL 문장들
    END IF;
    ```

- 날짜 관련 함수
    > CURRENT_DATE(); : 오늘 날짜   
    > CURRENT_TIMESTAMP(); : 오늘 날짜 및 시간
    > DATEDIFF(날짜1, 날짜2); : 날짜2부터 날짜1까지 일수로 몇일인지 계산

- CASE 문
    ```sql
    CASE
        WHEN 조건1 THEN
            SQL 문장들1
        WHEN 조건2 THEN
            SQL 문장들2
        WHEN 조건3 THEN
            SQL 문장들3
        ELSE
            SQL 문장들4
    END CASE;
    ```

- WHILE 문
    ```sql
    WHILE 조건식 DO
        SQL 문장들
    END WHILE;
    ```
    - 응용
        ```sql
        DROP PROCEDURE IF EXISTS whileProc2;
        DELIMITER $$
        CREATE PROCEDURE whileProc2()
        BEGIN
            DECLARE i INT;
            DECLARE hap INT;
            SET i = 1;
            SET hap = 0;

            myWhile:
            WHILE (i <= 100) DO
                IF (i%4 = 0) THEN
                    SET i = i + 1;
                    ITERATE myWhile; -- 지정한 label로 continue
                END IF;
                SET hap = hap + i;
                IF(hap > 1000) THEN
                    LEAVE myWhile; -- 지정한 label break
                END IF;
                SET i = i + 1;
            END WHILE;
        END $$
        DELIMITER ;

        CALL whileProc2();
        ```

- 동적 SQL
    > SQL 문은 내용이 고정된 경우가 대부분 이지만, 상황에 따라 내용 변경이 필요할 때 동적 SQL을 사용하여 변경되는 내용을 실시간으로 적용하여 사용할 수 있음

    - PREPARE : SQL문을 실행하지 않고 준비
        ```sql
        PREPARE myQuery FROM 'SELECT * FROM member WHERE mem_id = "BLK";
        ```
    - EXECUTE : 준비한 SQL 문을 실행
        ```sql
        EXECUTE myQuery;
        ```
    - DEALLOCATE PREFARE : 준비한 SQL문 해제
        ```sql
        DEALLOCATE PREFARE myQuery;
        ```
    
    - 응용
        ```sql
        SET @curDate = CURRENT_TIMESTAMP();

        PREPARE myQuery FROM 'INSERT INTO gate_table Values(null, ?)';
        EXECUTE myQuery USING @curDate;
        DEALLOCATE PREPARE myQuery;
        ```

<br>

---

<br>

## 테이블과 뷰

<br>

### 테이블

- 개요
  - 테이블 : 표 형태로 구성된 2차원 구조로써 행과 열로 구성됨

  - 행 : row 또는 record로 불림
  - 열 : column 또는 field로 불림

- SQL로 테이블 생성
  - DB 생성
    ```sql
    DROP DATABASE IF EXISTS db_name;
    CREATE DATABASE db_name;
    ```

  - 테이블 생성
    ```sql
    USE db_name;
    DROP TABLE IF EXISTS member;
    CREATE TABLE member
    (
        mem_id      CHAR(8) NOT NULL PRIMARY KEY,
        mem_name    VARCHAR(10) NOT NULL,
        mem_number  TINYINT NOT NULL,
        addr        CHAR(2) NOT NULL,
        phone       CHAR(8) NULL,
        height      TINYINT UNSIGNED NULL,
    );
    ```
  - 외래키로 연결되는 테이블 생성
    ```sql
    DROP TABLE IF EXISTS buy;
    CREATE TABLE buy
    (
        num INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
        mem_id CHAR(8) NOT NULL,
        price INT UNSIGNED NOT NULL,
        amount SMALLINT UNSIGNED NOT NULL,
        FOREIGN KEY(mem_id) REFERENCES member(mem_id) -- 외래키지정
    )
    ```
  - 데이터 입력하기
    ```sql
    INSERT INTO 테이블 [(열1, 열2, ...)] VALUES (값1, 값2, ...);
    ```

<br>

### 제약조건

<br>

- 개요
  - 제약조건 : 데이터의 무결성을 지키기 위해 제한하는 조건
    > 데이터의 무결성 : 데이터에 결함이 없음   
    > 아이디가 중복될 경우 여러 문제를 야기하는데, 이를 데이터의 결함이라 함

  - 제약조건의 종류|
    :---|
    PRIMARY KEY|
    FOREIGN KEY|
    UNIQUE|
    CHECK|
    DEFAULT|
    NULL|

  - 기본키 제약조건
    - 테이블의 많은 행 데이터 중 데이터를 구분할 수 있는 식별자로써 기본키에 입력되는 값은 중복될 수 없으며, NULL 값이 입력될 수 없음

    - 기본키로 생성시 자동으로 클러스터형 인덱스가 생성됨
    - 각 테이블은 기본키를 1개만 가질 수 있음
    - 테이블 삭제시 외래 키 테이블 삭제 후 기본 키 테이블 삭제해야함
    - 설정 방법
        ```sql
        -- 첫번째 방법
        CREATE TABLE member
        (
            mem_id      CHAR(8) NOT NULL PRIMARY KEY,
            phone       CHAR(8) NULL
        );

        -- 두번째 방법
        CREATE TABLE member
        (
            mem_id      CHAR(8) NOT NULL,
            phone       CHAR(8) NULL,
            PRIMARY KEY(mem_id)
        );

        -- 세번째 방법
        CREATE TABLE member
        (
            mem_id      CHAR(8) NOT NULL,
            phone       CHAR(8) NULL
        );
        ALTER TABLE member
            ADD CONSTRAINT
            PRIMARY KEY (mem_id);

        -- 기본키에 이름 지정
        CREATE TABLE member
        (
            mem_id      CHAR(8) NOT NULL,
            phone       CHAR(8) NULL,
            CONSTRAINT PRIMARY KEY PK_member_mem_id (mem_id)
        );
        ```

  - 외래키 제약조건
    - 두 테이블 사이의 관계를 연결해주고, 그 결과 데이터의 무결성을 보장해주는 역할

    - 기본키가 있는 테이블을 기준 테이블, 외래 키가 있는 테이블을 참조 테이블이라 부름
    - 참조 테이블이 참조하는 기준 테이블의 열은 반드시 기본키나 고유키로 설정되어 있어야 함
    - 설정 방법
        ```sql
        CREATE TABLE member
        (
            mem_id      CHAR(8) NOT NULL PRIMARY KEY,
            phone       CHAR(8) NULL
        );

        -- 첫번째 방법
        CREATE TABLE buy
        (
            num      INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
            user_id  CHAR(8) NOT NULL,
            FOREIGN  KEY(user_id) REFERENCES member(mem_id)
        );

        -- 두번째 방법
        CREATE TABLE buy
        (
            num      INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
            user_id  CHAR(8) NOT NULL
        );
        ALTER TABLE buy
            ADD CONSTRAINT
            FOREIGN KEY(user_id)
            REFERENCES member(mem_id);
        ```
    - 기본키-외래키로 맺어진 후엔 기준 테이블의 열 이름이 변경되지 않음
    - 기준 테이블의 열 이름을 변경하거나 삭제할때 참조 테이블도 자등으로 변경되도록 설정
        ```sql
        CREATE TABLE buy
        (
            num      INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
            user_id  CHAR(8) NOT NULL
        );
        ALTER TABLE buy
            ADD CONSTRAINT
            FOREIGN KEY(user_id)
            REFERENCES member(mem_id)
            ON UPDATE CASCADE
            ON DELETE CASCADE;
        ```

  - 고유키 제약조건
    - 중복되지 않는 유일한 값을 입력해야 하는 조건
    - NULL 입력을 허용함
        ```sql
        CREATE TABLE buy
        (
            num      INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
            user_id  CHAR(8) NOT NULL,
            email CHAR(30) NULL UNIQUE
        );
        ```

  - 체크 제약조건
    - 입력되는 데이터를 점검하는 기능
        ```sql
        CREATE TABLE buy
        (
            num      INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
            user_id  CHAR(8) NOT NULL,
            height   TINYINT UNSIGNED NULL CHECK (height >= 100),
            phone    CHAR(3) NULL
        );

        ALTER TABLE buy
            ADD CONSTRAINT
            CHECK (phone IN ('02', '031', '032', '054', '055', '061' ));
        ```

  - 기본값 정의
    - 값을 입력하지 않았을 때 자동으로 입력될 값을 미리 지정
        ```sql
        CREATE TABLE buy
        (
            num      INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
            user_id  CHAR(8) NOT NULL,
            height   TINYINT UNSIGNED NULL DEFAULT 160,
            phone    CHAR(3) NULL
        );

        ALTER TABLE buy
            ALTER COLUMN phone SET DEFAULT '02';

        INSERT INTO buy VALUES(NULL, '홍길동', default, default);
        ```

<br>

### 뷰 : 가상의 테이블

<br>

- 개요
  - 데이터베이스 개체 중 하나로써 SELECT 문이 뷰의 실체임

    ```sql
    -- 생성
    CREATE VIEW 뷰이름
    AS
        SELECT 문;
    
    CREATE VIEW v_member
    AS
        SELECT mem_id, mem_name, addr FROM member;

    -- 사용
    SELECT 열이름 FROM 뷰이름 [WHERE 조건];
    
    SELECT * FROM v_member;
    ```

  - 뷰를 사용하는 이유
    - 사용자 권한에 따라 접근 가능한 테이블 정보에 차별을 두어 보안강화

    - 자주사용하는 복잡한 SQL을 뷰로 생성하여 단순화

- 실제 작동
  - 생성, 수정, 삭제
    - 별칭 사용 가능하며 중간에 띄어쓰기 사용이 가능

    - 작은 따옴표, 큰 따옴표 둘다 사용가능하며, 형식상 AS를 사용하여 코드를 명확히 함
    - 뷰 조회시 열 이름에 공백이 있으면 백틱으로 묶어줘야 함
    ```sql
    -- 생성
    CREATE VIEW v_viewtest
    AS
        SELECT B.mem_id 'M ID', M.mem_name AS 'M Name', B.prod_name "P Name", CONCAT(M.phone1, M.phone2) AS "Office Phone"
            FROM buy B
                INNER JOIN member M
                ON B.mem_id = M.mem_id;
    
    SELECT DISTINCT `M ID`, `M Name` FROM v_viewtest;

    -- 수정
    ALTER VIEW v_viewtest
    AS
        SELECT B.mem_id 'M_ID', M.mem_name AS 'M_Name', B.prod_name "P_Name", CONCAT(M.phone1, M.phone2) AS "Phone"
            FROM buy B
                INNER JOIN member M
                ON B.mem_id = M.mem_id;
    
    SELECT DISTINCT M_ID, M_Name FROM v_viewtest;

    -- 삭제
    DROP VIEW v_viewtest;
    ```

  - 데이터베이스 개체의 생성, 수정, 삭제

    -  모든 데이터베이스 개체(테이블, 뷰, 인덱스, 스토어드 프로시저, 스토어드 함수, 트리거 등)를 생성
        ```sql
        CREATE 개체_종류
        ```
    - 수정
        ```sql
        ALTER 개체_종류
        ```
    - 삭제
        ```sql
        DROP 개체_종류
        ```

  - 뷰의 정보 확인
    ```sql
    CREATE VIEW            -- 같은 이름의 뷰가 존재하면 오류가 발생   
    CREATE OR REPLACE VIEW -- 기존 뷰가 있어도 덮어쓰기하여 오류가 발생하지 않음
    ```

    - 기존 뷰의 정보 확인 : 뷰에서 PRIMARY KEY 등의 정보는 확인불가
        ```sql
        DESCRIBE v_viewtest;
        DESC v_viewtest;

        DESCRIBE member; -- PRIMARY KEY 등의 정보도 조회됨

        SHOW CREATE VIEW; -- 뷰의 소스코드 확인
        ```

  - 뷰를 통한 데이터의 수정, 삭제
    ```sql
    -- 수정
    UPDATE v_member SET addr = '부산' WHERE mem_id = 'BLK';
    ```
    - 뷰가 참조하는 테이블의 열 중에서 NOT NULL로 설정된 열을 모두 포함하지 않으면 입력불가능

    - 뷰에 설정된 값의 범위를 벗어나는 값을 입력되지 않도록 하는 명령어
        ``` sql
        ALTER VIEW v_height167
        AS
            SELECT * FROM member WHERE height >= 167
                    WITH CHECK OPTION;
        ```

  - 단순 뷰와 복합 뷰
    - 단순 뷰 : 하나의 테이블로 만든 뷰
    - 복합 뷰 : 두 개 이상의 테이블로 만든 뷰
      > 주로 두 테이블을 조인한 결과를 사용   
      > 복합 뷰는 읽기 전용이므로, 데이터 입력, 수정, 삭제할 수 없음

  - 테이블은 뷰가 참조하고 있어도 삭제되고, 이후 뷰를 조회하면 에러코드발생
      
<br>

---

<br>

## 인덱스

<br>

- 개요
  - 데이터를 빠르게 찾을 수 있도록 도와주는 도구로써, 현실적으로 실무에서 인덱스 없이 DB운영이 불가능

  - 클러스터형 인덱스와 보조 인덱스로 나뉨
    > 클러스터형 인덱스 : 기본키로 지정하면 자동 생성되며 테이블당 1개만 만들 수 있고 지정한 열을 기준으로 자동 정렬됨   
    > 보조 인덱스 : 고유 키로 지정하면 자동 생성되며 여러 개를 만들 수도 있지만 자동 정렬되지 않음

  - 인덱스 조회
    ```sql
    SHOW INDEX FROM tableName;
    ```

- 인덱스의 내부 작동
  - 클러스터형 인덱스와 보조 인덱스는 모두 내부적으로 균형 트리로 만들어짐
  - 인덱스를 구성하면 데이터 변경 작업(INSERT, UPDATE, DELETE)시 페이지 분할 작업으로 인해 성능이 나빠짐

- 인덱스의 실제 사용
  - 인덱스 생성
    ```sql
    CREATE [UNIQUE] INDEX indexName
        ON tableName (rowName) [ASC | DESC]
    ```

  - 인덱스 제거
    ```sql
    DROP INDEX indexName ON tableName
    ```

  - 보조 인덱스 생성
    ```sql
    CREATE INDEX idx_member_addr
        ON member (addr);

    -- 생성 후 실제로 적용 시키려면 테이블을 분석/처리 해주어야 한다
    ANALYZE TABLE member;
    SHOW TABLE STATUS LIKE 'member';
    ```

  - 고유 보조 인덱스 생성
    ```sql
    CREATE UNIQUE INDEX idx_member_mem_name
        ON member (mem_name);

    -- 생성 후 실제로 적용 시키려면 테이블을 분석/처리 해주어야 한다
    ANALYZE TABLE member;
    SHOW TABLE STATUS LIKE 'member';
    ```
<br>

---

<br>

## 스토어드 프로시저

<br>

### 스토어드 프로시저 사용

- 개요
  - SQL에 프로그래밍을 추가하여 일반 프로그래밍 언어와 비슷한 효과를 낼 수 있음

  - 쿼리문의 집합으로 볼 수 있으며 어떠한 동작을 일괄 처리하기 위한 용도
  - DB의 개체 중 한 가지로써 테이블처럼 각 DB내부에 저장됨

- 형식
  ```sql
  DELIMITER $$
  CREATE PROCEDURE Nnme_proc (IN 또는 OUT 매개변수)
  BEGIN
    SQL CODES
  END $$
  DELIMITER ;

  CALL name_proc();

  DROP PROCEDURE name_proc;
  ```

- 매개변수의 사용
  ```sql
  IN 입력매개변수이름 데이터형식
  CALL 프로시저이름(전달값);

  DELIMITER $$
  CREATE PROCEDURE user_proc (
      IN userNumber INT,
      IN userHeight INT   )
  BEGIN
    SELECT * FROM member
      WHERE mem_number > userNumber AND height > userHeight;
  END $$
  DELIMITER ;

  CALL user_proc(6, 165);



  OUT 출력매개변수이름 데이터형식
  CALL 프로시저이름(@변수명);
  SELECT @변수명;

  DELIMITER $$
  CREATE PROCEDURE user_proc2 (
      IN txtValue CHAR(10),
      OUT outValue INT   )
  BEGIN
    INSERT INTO noTable VALUES(NULL, txtValue);
    SELECT MAX(id) INTO outValue FROM noTable;
  END $$
  DELIMITER ;

  CREATE TABLE IF NOT EXISTS noTable(
    id INT AUTO_INCREMENT PRIMARY KEY,
    txt CHAR(10)
  );

  CALL user_proc2('테스트1', @myValue);
  SELECT CONCAT('입력된 ID 값 = ', @myValue);

  ```

- 동적 SQL
  ```sql
  DROP PROCEDURE IF EXISTS dynamic_proc
  DELIMITER $$
  CREATE PROCEDURE dynamic_proc(
    IN tableName VARCHAR(20)
  )
  BEGIN
    SET @sqlQuery = CONCAT('SELECT * FROM ', tableName);
    PREPARE myQuery FROM @sqlQuery;
    EXECUTE myQuery;
    DEALLOCATE PREPARE myQuery;
  END $$
  DELIMITER ;

  CALL dynamic_proc('member');

  ```

<br>

### 스토어드 함수와 커서

<br>

- 개요
  - 스토어드 함수란 MySQL에서 제공하는 내장 함수 외에 직접 함수를 만드는 기능
  - 커서란 스토어드 프로시저 안에서 한 행씩 처리할 때 사용하는 프로그래밍 방식

- 스토어드 함수
  - 원형
    ```sql
    SET GLOBAL log_bin_trust_function_creators = 1; -- 스토어드 함수 생성 권한 허용

    DELIMITER $$
    CREATE FUNCTION 스토어드함수이름(매개변수)
        RETURNS 반환형식
    BEGIN
        SQL COEDS
        RETURN 반환값;
    END $$
    DELIMITER ;

    SELECT 스토어드함수이름();
    ```

- 커서
  - 테이블에서 한 행씩 처리하기 위한 방식
  - 작동 순서
    > 커서선언 -> 반복조건선건 -> 커서열기 -> 데이터가져오기 -> 데이터처리하기 -> 커서닫기
  
  - 통합 코드
    ```sql
    DROP PROCEDURE IF EXISTS cursor_proc;
    DELIMITER $$
    CREATE PROCEDURE cursor_proc()
    BEGIN
        -- 사용할 변수 준비
        DECLARE memNumber INT;
        DECLARE cnt INT DEFAULT 0;
        DECLARE totNumber INT DEFAULT 0;
        DECLARE endOfRow BOOLEAN DEFAULT FALSE;

        -- 커서 선언
        DECLARE memberCursor CURSOR FOR
            SELECT mem_number FROM member;

        -- 반복 조건 선언
        DECLARE CONTINUE HANDLER
            FOR NOT FOUND SET endOfRow = TRUE;

        -- 커서 열기
        OPEN memberCursor;

        -- 행 반복하기
        cursor_loop : LOOP
            FETCH memberCursor INTO memNumber;

            IF endOfRow THEN
                LEAVE cursor_loop;
            END IF;

            SET cnt = cnt + 1;
            SET totNumber = totNumber + memNumber;
        END LOOP cursor_loop;

        SELECT (totNumber/cnt) AS '회원의 평균 인원 수';

        -- 커서 닫기
        CLOSE memberCursor;
    END $$
    DELIMITER ;

    CALL cursor_proc();
    ```

<br>

### 트리거

<br>

- 개요
  - DML(INSERT, UPDATE, DELETE) 문의 이벤트가 발생할 때 자동으로 실행되는 프로그래밍 기능
  - 자동으로 수행하여 사용자가 추가 작업을 잊어버리는 실수를 방지

- 원형
    ```sql
    DROP TRIGGER IF EXISTS myTrigger;
    DELIMITER $$
    CREATE TRIGGER myTrigger
        AFTER DELETE
        ON trigger_table
        FOR EACH ROW
    BEGIN
        SET @msg = '가수 그룹이 삭제됨';
    END $$
    DELIMITER ;
    ```

- 활용
  - 테이블에 입력/수정/삭제되는 정보를 백업하는 용도로 활용

    ```sql
    -- 테이블 복사시 기본 키 등의 설정은 복사되지 않음
    CREATE TABLE singer (SELECT mem_id, mem_name, mem_number, addr FROM member);
    
    CREATE TABLE backup_singer
    (
        mem_id CHAR(8) NOT NULL,
        mem_name VARCHAR(10) NOT NULL,
        mem_number INT NOT NULL,
        addr CHAR(2) NOT NULL,
        modType CHAR(2),
        modDate DATE,
        modUser VARCHAR(30)
    );

    DROP TRIGGER IF EXISTS singer_updateTrg;
    DELIMITER $$
    CREATE TRIGGER singer_updateTrg
        AFTER UPDATE -- 변경 후에 작동하도록 지정
        ON singer -- 트리거를 부착할 테이블
        FOR EACH ROW
    BEGIN
        INSERT INTO backup_singer VALUES(OLD.mem_id, OLD.mem_name, OLD.mem_number, OLD.addr, '수정', CURDATE(), CURRENT_USER());
    END $$
    DELIMITER ;
    ```
    > OLD 테이블은 UPDATE, DELETE가 수행될 때, 변경되기 전의 데이터가 잠깐 저장되는 임시 테이블