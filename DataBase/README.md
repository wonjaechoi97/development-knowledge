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
    ```sql
    INSERT INTO member VALUES('2013', NULL, '홍길동', 9, '서울', '02', '170');
    ```

### 테이블 조회
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

<br/>

### 서브쿼리
- SELECT 안에 또 다른 SELECT가 들어가는 것
    ```sql
    SELECT height FROM member WHERE mem_name='홍길동';
    SELECT mem_name, height FROM member WHERE height > 164;
    
    -- 위의 두 쿼리문을 합치면
    SELECT mem_name, height FROM member 
        WHERE height > (SELECT height FROM member WHERE mem_name='홍길동');
    ```

<br/>

### 
- 

<br/>

---

<br>

## 

