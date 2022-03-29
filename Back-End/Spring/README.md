# **Spring** 🐍

## 목차

- [오브젝트와 의존관계](#오브젝트와-의존관계)

<br>

---

### 오브젝트와 의존관계

#### DAO

> Data Access Object : DB를 사용해 데이터를 조회하거나 조작하는 기능을 전담하도록 만든 오브젝트

#### JDBC를 이용하는 작업의 일반적인 순서

> DB 연결을 위한 Connection을 가져옴  
> SQL을 담은 Statement(또는 PreparedStatement)를 만들고 실행  
> 조회의 경우 SQL 쿼리의 실행 결과를 ResultSet으로 받아서 오브젝트에 옮김  
> 작업을 마친 후 Connection, Statement, ResultSet 같은 리소스를 반드시 닫아줌

<br>

[목차로 이동](#목차)