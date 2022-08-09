# **JPA** 🔗

### 참고자료

- 자바 ORM 표준 JPA 프로그래밍 (김영한 저)

### 학습 프로젝트 저장소

- [링크]()

## 목차
>- [JPA 소개](#jpa-소개)
>>- [SQL을 직접 다룰 때 발생하는 문제점](#sql을-직접-다룰-때-발생하는-문제점)
>>- [패러다임의 불일치](#패러다임의-불일치)
>>- [JPA란 무엇인가?](#jpa란-무엇인가)
>- [JPA 시작](#jpa-시작)
>>- [라이브러리와 프로젝트 구조](#라이브러리와-프로젝트-구조)
>>- [객체 매핑 시작](#객체-매핑-시작)
>>- [persistence.xml 설정](#persistencexml-설정)
>>- [애플리케이션 개발](#애플리케이션-개발)
>- [영속성 관리](#영속성-관리)
>>- [영속성 관리 개요](#영속성-관리-개요)
>>- [엔티티 매니저 팩토리와 엔티티 매니저](#엔티티-매니저-팩토리와-엔티티-매니저)
>>- [영속선 컨텍스트란?](#영속선-컨텍스트란)
>>- [엔티티의 생명주기](#엔티티의-생명주기)
>>- [영속성 컨텍스트의 특징](#영속성-컨텍스트의-특징)
>>- [플러시](#플러시)
>>- [준영속](#준영속)
>>- [영속성 관리 정리](#영속성-관리-정리)
>- [엔티티 매핑](#엔티티-매핑)
>>- [엔티티 매핑 개요](#엔티티-매핑-개요)
>>- [@Entity](#entity)
>>- [@Table](#table)
>>- [다양한 매핑 사용](#다양한-매핑-사용)
>>- [데이터베이스 스키마 자동 생성](#데이터베이스-스키마-자동-생성)
>>- [DDL 생성 기능](#ddl-생성-기능)
>>- [기본 키 매핑](#기본-키-매핑)
>>- [필드와 컬럼 매핑: 레퍼런스](#필드와-컬럼-매핑-레퍼런스)
>- [연관관계 매핑 기초](#연관관계-매핑-기초)
>>- [연관관계 매핑 기초 개요](#연관관계-매핑-기초-개요)
>>- [단방향 연관관계](#단방향-연관관계)
>>- [연관관계 사용](#연관관계-사용)
>>- [양방향 연관관계](#양방향-연관관계)
>>- [연관관계의 주인](#연관관계의-주인)
>>- [양방향 연관관계 저장](#양방향-연관관계-저장)
>>- [양방향 연관관계의 주의점](#양방향-연관관계의-주의점)
>- [다양한 연관관계 매핑](#다양한-연관관계-매핑)
>>- [다양한 연관관계 매핑 개요](#다양한-연관관계-매핑-개요)
>>- [다대일](#다대일)
>>- [일대일](#일대일)
>>- [다대다 [N:N]](#다대다-nn)

<br>

---

## JPA 소개

>### SQL을 직접 다룰 때 발생하는 문제점
>- 반복, 반복 그리고 반복
>>- DB는 객체 구조와는 다르게 데이터 중심의 구조를 가지므로 객체를 DB에 직접 저장하거나 조회할 수 없음
>>- 개발자가 객체지향 애플리케이션과 DB 중간에서 SQL과 JDBC API를 사용하여 변환 작업을 직접 해주어야 함
>>- 객체를 DB에 CRUD하려면 많은 SQL과 JDBC API를 코드로 작성해야 하며 테이블마다 반복해야 하므로 데이터 접근 계층 (DAO) 개발에 많은 시간을 낭비하게됨
>- SQL에 의존적인 개발
>>- 객체에 연관된 객체를 사용할 수 있을지 없을지는 전적으로 사용하는 SQL에 달려있으므로 DAO를 사용하여 SQL을 숨겨도 결국 DAO를 열어서 어떤 SQL이 실행되는지 확인해야 한다는 문제점이 있음
>>- 비즈니스 요구사항을 모델링한 객체를 엔티티라고 하는데, SQL에 모든 것을 의존하는 상황에서는 개발자들이 엔티티를 신뢰하고 사용할 수 없고 DAO를 열어서 일일히 확인해야 함
>>- 물리적으로 SQL과 JDBC API를 DAO에 숨기는 데 성공했지만 논리적으로 엔티티와 아주 강한 의존관계를 가지고 있으므로 진정한 의미의 계층 분할이 아님
>>- 강한 의존 관계 때문에 조회할 때나 필드를 추가할 때나 DAO의 CRUD 코드와 SQL 대부분을 변경해야 하는 문제가 발생함
>- JPA와 문제 해결
>>- JPA를 사용하면 객체를 DB에 저장하고 관리할 때, 개발자가 직접 SQL을 작성하는 것이 아니라 JPA가 제공하는 API를 사용하고, JPA가 개발자 대신 SQL을 생성해서 DB에 전달함
>>- JPA가 제공하는 CRUD API
>>>- 저장 기능
>>>>```java
>>>>jpa.persist(member);
>>>>```
>>>>- 호출시 JPA가 객체와 매핑정보를 보고 적절한 INSERT SQL을 생성해서 DB에 전달함
>>>- 조회 기능
>>>>```java
>>>>String memberId = "helloId";
>>>>Member member = jpa.find(Member.class, memberId);
>>>>```
>>>>- 호출시 JPA가 객체와 매핑정보를 보고 적절한 SELECT SQL을 생성해서 DB에 전달하고 그 결과로 Member 객체를 생성해서 반환함
>>>- 수정 기능
>>>>```java
>>>>Member member = jpa.find(Member.class, memberId);
>>>>member.setName("이름변경");
>>>>```
>>>>- JPA는 별도의 수정 메서드를 제공하지 않는 대신 객체를 조회해서 값을 변경하면 트랜잭션을 커밋할 때 DB에 적절한 UPDATE SQL이 전달됨
>>>- 연관된 객체 조회
>>>>```java
>>>>Member member = jpa.find(Member.class, memberId);
>>>>Team team = member.getTeam();
>>>>```
>>>>- JPA는 연관된 객체를 사용하는 시점에 적절한 SELECT SQL을 실행하므로, JPA를 사용하면 연관된 객체를 마음껏 조회할 수 있음

<br>

[목차로 이동](#목차)

>### 패러다임의 불일치
>- 상속
>>- 객체는 상속이라는 기능을 가지고 있지만 테이블은 상속이라는 기능이 없음
>>- DB 모델링의 슈퍼타입, 서브타입 관계를 사용하면 그나마 객체 상속과 가장 유사한 형태로 테이블을 설계할 수 있음
>>- 객체 모델 코드
>>>```java
>>>abstract class Item {
>>>  Long id;
>>>  String name;
>>>  int price;
>>>}
>>>
>>>class Album extends Item {
>>>  String artist;
>>>}
>>>
>>>class Movie extends Item {
>>>  String director;
>>>  String actor;
>>>}
>>>
>>>class Book extends Item {
>>>  String author;
>>>  String isbn;
>>>}
>>>```
>>- 패러다임 불일치를 해결하기 위해 소모하는 비용
>>>- 위 코드에서 하나의 객체를 저장하려면 객체를 분해해서 두 개의 SQL을 만들어야 함
>>>>```sql
>>>>INSERT INTO ITEAM ...
>>>>INSERT INTO ALBUM ...
>>>>```
>>>- JDBC API로 코드를 완성하려면 부모 객체에서 부모 데이터만 꺼내서 ITEM용 INSERT SQL을 작성하고, 자식 객체에서 자식 데이터만 꺼내서 ALBUM용 INSERT SQL을 작성해야하며, 자식 타입에 따라 DTYPE도 저장해야 하는 등 코드량이 만만치 않음
>>>- 조회할 때도, ITEM과 ALBUM 테이블을 조인 후 조회하여 그 결과로 Album 객체를 생성해야 함
>>>- 만약 해당 객체들을 DB가 아닌 자바 컬렉션에 보관한다면 타입에 대한 고민 없이 해당 컬렉션을 그냥 사용하면 됨
>>>>```java
>>>>list.add(album);
>>>>list.add(movie);
>>>>
>>>>Album album=list.get(albumId);
>>>>```
>>- JPA와 상속
>>>- JPA는 상속과 관련된 패러다임의 불일치 문제를 개발자 대신 해결해 주기 때문에 개발자는 자바 컬렉션에 객체를 저장하듯이 JPA에게 객체를 저장하면 됨
>>>- JPA를 사용하여 Item을 상속한 Album 객체를 저장하는 과정 예시
>>>>```java
>>>>jpa.persist(album);
>>>>```
>>>>- persist() 메서드를 사용하여 객체를 저장하면 JPA는 두 개의 SQL을 실행해서 객체를 ITEM, ALBUM 두 테이블에 나누어 저장함
>>>>>```sql
>>>>>INSERT INTO ITEAM ...
>>>>>INSERT INTO ALBUM ...
>>>>>```
>>>- JPA를 사용하여 Album 객체를 조회하는 과정 예시
>>>>```java
>>>>String albumId="id100";
>>>>Album album=jpa.find(Album.class, albumId);
>>>>```
>>>>- JPA는 ITEM과 ALBUM 두 테이블을 조인해서 필요한 데이터를 조회하고 그 결과를 반환함
>>>>>```sql
>>>>>SELECT I.*, A.*
>>>>>  FROM ITEM I
>>>>>  JOIN ALBUM A ON I.ITEM_ID = A.ITEM_ID
>>>>>```
>- 연관관계
>>- 개요
>>>- 객체는 참조를 사용하여 다른 객체와 연관관계를 가지며 참조에 접근해서 연관된 객체를 조회, 반면에 테이블은 외래 키를 사용해서 다른 테이블과 연관관계를 가지며 조인을 사용해서 연관된 테이블을 조회함
>>>- 참조를 사용하는 객체와 외래 키를 사용하는 관계형 DB 사이의 패러다임 불일치는 매우 극복하기 어려운 문제점이 있음
>>>>- Member 객체는 Member.team 필드에 Team 객체의 참조를 보관하여 Team 객체와 관계를 맺고, team 참조 필드에 접근하면 Member와 연관된 Team을 조회할 수 있음
>>>>- MEMBER 테이블은 MEMBER.TEAM_ID 외래 키 컬럼을 사용해서 TEAM 테이블과 관계를 맺고, 외래 키를 사용해서 두 테이블을 조인하면 연관된 TEAM 테이블을 조회할 수 있음
>>>>- 또한 객체는 참조가 있는 방향으로만 조회할 수 있지만 테이블은 양방향이 가능함
>>- 객체를 테이블에 맞추어 모델링
>>>- 테이블에 맞춘 객체 모델
>>>>```java
>>>>class Member {
>>>>  String id;        // MEMBER_ID 컬럼 사용
>>>>  Long teamId;      // TEAM_ID FK 컬럼 사용
>>>>  String username;  // USERNAME 컬럼 사용
>>>>}
>>>>
>>>>class Team {
>>>>  Long id;        // TEAM_ID PK 사용
>>>>  String name;    // NAME 컬럼 사용
>>>>}
>>>>```
>>>- 관계형 DB는 조인이 있으므로 외래 키의 값을 그대로 보관해도 되지만 객체는 연관된 객체의 참조를 보관해야하므로 조회할 수 없음
>>- 객체지향 모델링
>>>- 참조를 사용하는 객체 모델
>>>>```java
>>>>class Member {
>>>>  String id;        // MEMBER_ID 컬럼 사용
>>>>  Team team;        // 참조로 연관관계를 맺음
>>>>  String username;  // USERNAME 컬럼 사용
>>>>  
>>>>  Team getTeam() {
>>>>    return team;
>>>>  }
>>>>}
>>>>
>>>>class Team {
>>>>  Long id;       // TEAM_ID PK 사용
>>>>  String name;   // NAME 컬럼 사용
>>>>}
>>>>```
>>>- 객체지향 모델링을 사용하면 객체를 테이블에 저장하거나 조회하기가 어려움
>>>- Member 객체는 team 필드로 연관관계를 맺고 MEMBER 테이블은 TEAM_ID 외래 키로 연관관계를 맺기 떄문에 객체 모델은 외래 키가 필요 없고 참조만 있으면 되는 반면에 테이블은 참조가 필요 없고 외래 키만 있으면 되므로, 결국 개발자가 중간에서 변환 역할을 해야함
>>- JPA와 연관관계
>>>```java
>>>member.setTeam(team); // 회원과 팀 연관관계 설정
>>>jpa.persist(member);  // 회원과 연관관계 함께 저장
>>>```
>>>- JPA는 team의 참조를 외래 키로 변환해서 INSERT SQL을 DB에 전달하고, 객체를 조회할 때 외래 키를 참조로 변환하는 일도 JPA가 처리함
>>>```java
>>>Member member = jpa.find(Member.class, memberId);
>>>Team team = member.getTeam();
>>>```
>>>- SQL을 직접 다루어도 열심히 코드만 작성하면 극복할 수 있는 문제로 보일 수 있으나, 연관관계와 관련하여 극복하기 어려운 패러다임의 불일치 문제가 후술됨
>- 객체 그래프 탐색
>>- 개요
>>>- 객체에서 회원이 소속된 팀을 조회할 땐 참조를 사용하여 연관된 팀을 찾는 것을 객체 그래프 탐색이라함
>>>- SQL을 직접 다루면 처음 실행하는 SQL에 따라 객체 그래프의 탐색범위가 정해지는데 이것은 객체지향에선 너무 큰 제약임
>>>>- 비즈니스 로직에 따라 사용하는 객체 그래프가 다른데 언제 끊어질지 모를 객체 그래프를 함부로 탐색할 수 없기 때문
>>>>```java
>>>>class MemberService {
>>>>  ...
>>>>  public void process() {
>>>>    Member member = memberDAO.find(memberId);
>>>>    member.getTeam();  // member->team 객체 그래프 탐색이 가능한가?
>>>>    member.getOrder().getDelivery(); // ???
>>>>  }
>>>>}
>>>>```
>>>>- MemberService는 memberDAO를 통해 member 객체를 조회했지만 연관된 Team, Order, Delivery 방향으로 객체 그래프를 탐색할 수 있을지 없을지 코드만 보고 전혀 예측할 수 없고, DAO를 열어서 SQL을 직접 확인해야함
>>>>- 엔티티가 SQL에 논리적으로 종속되어 발생하는 문제임
>>>>>- 문제를 해결하고자 member와 연관된 모든 객체 그래프를 DB에서 조회해서 애플리케이션 메모리에 올려두는 것은 현실성이 없음
>>>>>- 결국 MemberDAO에 회원을 조회하는 메서드를 상황에 따라 여러 벌 만들어서 사용해야 함
>>>>>```java
>>>>>memberDAO.getMember();
>>>>>memberDAO.getMemberWithTeam();
>>>>>memberDAO.getMemberWithOrderWithDelivery();
>>>>>```
>>- JPA와 객체 그래프 탐색
>>>```java
>>>member.getOrder().getOrderItem()... // 자유로운 객체 그래프 탐색
>>>```
>>>- JPA는 연관된 객체를 사용하는 시점에 적절한 SQL을 실행하므로 JPA를 사용하면 연관된 객체를 신뢰하고 마음껏 조회할 수 있음
>>>- 해당 기능은 실제 객체를 사용하는 시점까지 DB조회를 미룬다고 하여 지연 로딩이라 함
>>>- JPA는 지연 로딩은 투명(transparent)하게 처리하여 추가적인 코드를 작성할 필요없음
>>>>```java
>>>>// 처음 조회 시점에 SELECT MEMBER SQL
>>>>Member member = jpa.find(Member.class, memberId);
>>>>
>>>>Order order = member.getOrder();
>>>>order.getOrderDate();  // Order를 사용하는 시점에 SELECT ORDER SQL
>>>>```
>>>>- 마지막줄 처럼 실제 Order 객체를 사용하는 시점에 JPA는 DB에서 ORDER 테이블을 조회하는 것처럼 지연 로딩을 사용함
>>>>- Member를 사용할 때마다 Order를 함께 사용하면, 한 테이블씩 조회하것 보다 Member를 조회하는 시점에 SQL 조인을 사용해서 Member와 Order를 함께 조회하는것이 효과적임
>>>- JPA는 연관된 객체를 즉시 함께 조회할지 아니면 실제 사용되는 시점에 지연해서 조회할지를 간단한 설정으로 정의할 수 있음
>- 비교
>>- 개요
>>>- DB는 기본 키의 값으로 각 row를 구분하는 반면에 객체는 동일성(identity) 비교와 동등성(equality) 비교라는 두 가지 비교 방법이 있음
>>>>- 동일성 비교는 == 비교, 객체 인스턴스의 주소 값을 비교함
>>>>- 동등성 비교는 equlas() 메서드로 객체 내부의 값을 비교함
>>>```java
>>>class MemberDAO {
>>>  public Member getMember(String memberId) {
>>>    String sql = "SELECT * FROM MEMBER WHERE MEMBER_ID = ?";
>>>    ...
>>>    // JDBC API, SQL 실행
>>>    return new Member(...);
>>>  }
>>>}
>>>
>>>String memberId = "100";
>>>Member member1 = memberDAO.getMember(memberId);
>>>Member member2 = memberDAO.getMember(memberId);
>>>
>>>member1 == member2; // 다르다
>>>```
>>>- 같은 회원 객체를 두 번 조회했지만 객체 측면에서 볼 떄 둘은 다른 인스턴스 이기 때문에 false가 반환됨
>>>```java
>>>Member member1 = list.get(0);
>>>Member member2 = list.get(0);
>>>member1 == member2 // 같다
>>>```
>>>- 컬렉션에 저장했다면 동일성 비교에 성공함
>>>- 패러다임 불일치 문제를 해결하기 위해 DB의 같은 row를 조회할 때마다 같은 인스턴스를 반환하도록 구현하는 것은 쉽지 않으며, 여러 트랜잭션이 동시에 실행되는 상황까지 고려한다면 더욱 어려워짐
>>- JPA와 비교
>>>- JPA는 같은 트랜잭션일 때 같은 객체가 조회되는 것을 보장함
>>>```java
>>>String memberId = "100";
>>>Member member1 = jpa.find(Member.class, memberId);
>>>Member member2 = jpa.find(Member.class, memberId);
>>>member1 == member2; // 같음
>>>```
>- 정리
>>- 객체 모델과 관계 DB 모델은 지향하는 패러다임이 서로 달라서, 차이를 극복하려고 개발자가 너무 많은 시간과 코드를 소비한다는 점이 문제임
>>- 객체지향 어플리케이션답게 정교한 객체 모델링을 할수록 패러다임 문제가 더 커진다는 더 큰 문제가 있고 틈을 메우기 위해 개발자가 소모해야 하는 비용도 점점 더 많아지며, 결국 객체 모델링은 힘을 잃고 점점 데이터 중심의 모델로 변해감
>>- 자바 진영에서 오랜 기간 패러다임 불일치 문제를 해결하기 위해 많은 노력을 기울여온 결과물이 JPA이며, JPA는 패러다임 불일치 문제를 해결해주고 정교환 객체 모델링을 유지하게 도와줌

<br>

[목차로 이동](#목차)

>### JPA란 무엇인가?
>- 개요
>>- JPA(Java Persistence API)는 자바 진영의 ORM 기술 표준이며, 애플리케이션과 JDBC 사이에서 동작함
>>- ORM(Object-Relational Mapping)은 객체와 관계형 DB를 매핑한다는 의미
>>- ORM 프레임워크는 객체와 테이블을 매핑해서 패러다임의 불일치 문제를 개발자 대신 해결해줌
>>>- 객체를 DB에 저장할 때 INSERT SQL을 직접 작성하지 않고 객체를 자바 컬렉션에 저장하듯 ORM 프레임워크에 저장하면됨
>>>- ORM 프레임워크가 적절한 INSERT SQL을 생성해서 DB에 객체를 저장해줌
>>>```java
>>>jpa.persist(member); // 저장
>>>
>>>Member member = jpa.find(memberId); // 조회
>>>```
>>- ORM 프레임워크는 단순히 SQL을 개발자 대신 생성해서 DB에 전달하는것 뿐만 아니라 다양한 패러다임 불일치 문제들도 해결해줌
>>- 객체 측면에서는 정교한 객체 모델링을 할 수 있고 관계형 DB는 DB에 맞도록 모델링하면 됨
>>- 둘을 어떻게 매핑해야 하는지 매핑 방법만 ORM 프레임워크에 알려주면 되므로 개발자는 DB 중심인 관계형 DB를 사용해도 객체지향 어플리케이션 개발에 집중할 수 있음
>>- 자바 진영의 ORM 프레임워크에는 하이버네이트가 가장 많이 사용되며 거의 대부분의 패러다임 불일치 문제를 해결해주는 성숙한 ORM 프레임워크임
>- JPA소개
>>- 과거 자바 진영은 EJB라는 기술 표준을 만들었는데 엔티티 빈이라는 ORM 기술도 포함되어 있었으나 복잡하고 성숙도도 떨어졌으며 J2EE 애플리케이션 서버에서만 동작했음
>>- 이때 EJB의 ORM 기술과 비교해서 가볍고 실용적이고 성숙도도 높으며 J2EE 서버 없이도 동작하는 하이버네이트라는 오픈소스 ORM 프레임워크가 등장하여 많은 개발자가 사용하였고, 결국 하이버네이트를 기반으로 JPA라는 새로운 자바 ORM 기술 표준이 만들어짐
>>- JPA는 자바 ORM 기술에 대한 API 표준 명세이므로 JPA를 사용하려면 JPA를 구현한 ORM 프레임워크를 선택해야하는데 하이버네이트가 가장 대중적임
>>- JPA라는 표준 덕분에 특정 구현 기술에 대한 의존도를 줄일 수 있고 다른 구현 기술로 손쉽게 이동할 수 있는 장점이 있음
>>- JPA 표준은 일반적이고 공통적인 기능의 모음이므로 표준을 먼저 이해하고 필요에 따라 JPA 구현체가 제공하는 고유의 기능을 알아가면 됨
>>>- JPA 1.0 (JSP 220) 2006년 : 초기버전으로 복합키와 연관관계 기능이 부족
>>>- JPA 2.0 (JSP 317) 2009년 : 대부분의 ORM 기능을 포함하고 JPA Criteria가 추가됨
>>>- JPA 2.1 (JSP 338) 2013년 : 스토어드 프로시저 접근, 컨버터, 엔티티 그래프 기능이 추가됨
>- 왜 JPA를 사용해야 하는가?
>>- 생산성
>>>- JPA를 사용하면 자바 컬렉션에 객체를 저장하듯이 JPA에게 저장할 객체를 전달하면 CRUD SQL을 작성하고 JDBC API를 사용하는 지루하고 반복적인 일은 JPA가 대신 처리해줌
>>>- 추가적으로 CREATE TABLE같은 DDL 문을 자동으로 생성해주는 기능도 있으므로 DB 설계 중심의 패러다임을 객체 설계 중심으로 역전시킬 수 있음
>>- 유지보수
>>>- SQL을 직접 다루면 엔티티에 필드를 하나만 추가해도 관련된 등록, 수정, 조회 SQL과 결과를 매핑하기 위한 JDBC API 코드를 모두 변경해야 했으나 JPA를 사용하면 대신 처리해주므로 유지보수해야 하는 코드 수가 줄어듬
>>- 패러다임의 불일치 해결
>>>- JPA는 상속, 연관관계, 객체 그래프 탐색, 비교하기와 같은 패러다임의 불일치 문제를 해결해줌
>>- 성능
>>>- JPA는 애플리케이션과 DB 사이에서 다양한 성능 최적화 기회를 제공함
>>>```java
>>>String memberId = "helloId";
>>>Member member1 = jpa.find(memberId);
>>>Member member2 = jpa.find(memberId);
>>>```
>>>- JDBC API를 사용했다면 DB와 두 번 통신해야하지만, JPA를 사용하면 회원을 조회하는 SQL을 한 번만 DB에 전달하고 두 번째는 조회한 회원 객체를 재사용함
>>- 데이터 접근 추상화와 벤더 독립성
>>>- 관게 DB는 같은 기능도 벤더마다 사용법이 다른 경우가 많은데 단적인 예로 페이징 처리는 DB마다 달라서 사용법을 각각 배워야 하므로 애플리케이션은 처음 선택한 DB 기술에 종속되고 다른 DB로 변경하기는 매우 어려움
>>>- JPA는 애플리케이션과 DB 사이에 추상화된 데이터 접근 계층을 제공하여 특정 DB 기술에 종속되지 않도록 하여 만약 DB를 변경하면 JPA에게 다른 DB를 사용한다고 알려주기만 하면 됨
>>- 표준
>>>- JPA는 자바 진영의 ORM 기술 표준이고, 표준을 사용하면 다른 구현 기술로 손쉽게 변경할 수 있음

<br>

[목차로 이동](#목차)

---

## JPA 시작

>### 라이브러리와 프로젝트 구조
>- 개요
>>- JPA 구현체로 하이버네이트를 사용하기 위한 핵심 라이브러리
>>>- hibernate-core : 하이버네이트 라이브러리
>>>- hibernate-entitymanager : 하이버네이트가 JPA 구현체로 동작하도록 JPA 표준을 구현한 라이브러리
>>>- hibernate-jpa-2.1-api : JPA 2.1 표준 API를 모아둔 라이브러리

<br>

[목차로 이동](#목차)

>### 객체 매핑 시작
>```sql
>CREATE TABLE MEMBER (
>  ID VARCHAR(255) NOT NULL,
>  NAME VARCHAR(255),
>  AGE INTEGER NOT NULL,
>  PRIMARY KEY (ID)
>)
>```
>```java
>package jpabook.start;
>
>import javax.persistence.*;
>
>@Entity
>@Table(name="MEMBER")
>public class Member {
>
>  @Id
>  @Column(name = "ID")
>  private String id;
>
>  @Column(name = "NAME")
>  private String username;
>
>  private Integer age;
>
>  // Getter, Setter
>}
>```
>- @Entity
>>- 해당 클래스를 테이블과 매핑한다고 JPA에게 알려주는 어노테이션으로, 해당 어노테이션이 사용된 클래스를 엔티티 클래스라 함
>- @Table
>>- 엔티티 클래스에 매핑할 테이블 정보를 알려줌
>>- 여기서는 name 속성을 사용하여 Member 엔티티를 MEMBER 테이블에 매핑했음
>>- 생략시 클래스 이름을 테이블 이름으로 매핑함 (더 정확히는 엔티티 이름을 사용함)
>- @Id
>>- 엔티티 클래스의 필드를 테이블의 기본 키에 매핑함
>>- 해당 어노테이션이 사용된 필드를 식별자 필드라고 함
>- @Column
>>- 필드를 컬럼에 매핑함
>>- name 속성을 사용하여 Member 엔티티의 username 필드를 MEMBER 테이블의 NAME 컬럼에 매핑함
>- 매핑 정보가 없는 필드
>>- age 필드에는 매핑 어노테이션이 없는데, 매핑 어노테이션을 생략하면 필드명을 사용해서 컬럼명으로 매핑함
>>- 필드명이 age이므로 age 컬럼으로 매핑함
>>- 만약 대소문자를 구분하는 DB를 사용한다면 @Column(name="AGE") 처럼 명시적으로 매핑해야 함

<br>

[목차로 이동](#목차)

>### persistence.xml 설정
>- JPA는 persistence.xml을 사용하여 필요한 설정 정보를 관리함
>- 해당 설정 파일이 META-INF/persistence.xml 클래스 패스 경로에 있으면 별도의 설정 없이 JPA가 인식할 수 있음
>>```xml
>><?xml version="1.0" encoding="UTF-8"?>
>><persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.1">
>>  <persistence-unit name="jpabook">
>>    <properties>
>>      <!-- 필수 속성 -->
>>      <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
>>      <property name="javax.persistence.jdbc.user" value="sa"/>
>>      <property name="javax.persistence.jdbc.password" value=""/>
>>      <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
>>      <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect" />
>>
>>      <!-- 옵션 -->
>>      <property name="hibernate.show_sql" value="true" />
>>      <property name="hibernate.format_sql" value="true" />
>>      <property name="hibernate.use_sql_comments" value="true" />
>>      <property name="hibernate.id.new_generator_mappings" value="true" />
>>    </properties>
>>  </persistence-unit>
>></persistence>
>>```
>>```xml
>><persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.1">
>>```
>>- 설정 파일은 persistence로 시작하며 XML 네임스페이스와 사용할 버전을 지정함
>>```xml
>><persistence-unit name="jpabook">
>>```
>>- JPA 설정은 영속성 유닛부터 시작하는데 일반적으로 연결할 DB 하나당 영속성 유닛을 등록하며 영속성 유닛에는 고유한 이름을 부여해야 함
>>```xml
>><properties>
>>  <!-- 필수 속성 -->
>>  <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
>>  <property name="javax.persistence.jdbc.user" value="sa"/>
>>  <property name="javax.persistence.jdbc.password" value=""/>
>>  <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
>>  <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect" />
>>  ...
>>```
>>- JPA 표준 속성
>>>- javax.persistence.jdbc.driver : JDBC 드라이버
>>>- javax.persistence.jdbc.user : DB 접속 아이디
>>>- javax.persistence.jdbc.password : DB 접속 비밀번호
>>>- javax.persistence.jdbc.url : DB 접속 URL
>>- 하이버네이트 속성
>>>- hibernate.dialect : 데이터베이스 방언(Dialect) 설정
>>- 이름이 javax.persistence로 시작하는 속성은 JPA 표준 속성으로 특정 구현체에 종속되지 않음
>>- 반면에 hibernate로 시작하는 속성은 하이버네이트 전용 속성임
>- 데이터베이스 방언
>>- JPA는 특정 DB에 종속적이지 않은 기술이므로 다른 DB로 손쉽게 교체할 수 있으나, 각 DB마다 제공하는 SQL 문법과 함수가 조금씩 다른 문제점이 있음
>>- DB별 차이점 예시
>>>- 데이터 타입 : 가변 문자 타입으로 MySQl은 VARCHAR, 오라클은 VARCHAR2를 사용함
>>>- 다른 함수명 : 문자열을 자르는 함수로 SQL 표준은 SUBSTRING(), 오라클은 SUBSTR()을 사용함
>>>- 페이징 처리 : MySQL은 LIMIT, 오라클은 ROWNUM을 사용함
>>- 이처럼 SQL 표준을 지키지 않거나 특정 DB만의 고유한 기능을 JPA에서는 방언(Dialect)라 함
>>- 애플리케이션 개발자가 특정 데이터베이스에 종속되는 기능을 많이 사용하면 추후 DB 교체가 어려울 수 있음
>>- 하이버네이트를 포함한 대부분의 JPA 구현체들은 이런 문제를 해결하고자 다양한 DB 방언 클래스를 제공함
>```xml
><!-- 옵션 -->
><property name="hibernate.show_sql" value="true" />
><property name="hibernate.format_sql" value="true" />
><property name="hibernate.use_sql_comments" value="true" />
><property name="hibernate.id.new_generator_mappings" value="true" />
>```
>- hibernate.show_sql : 하이버네이트가 실행한 SQL을 출력함
>- hibernate.format_sql : 하이버네이트가 실행한 SQL을 출력할 때 보기 쉽게 정렬함
>- hibernate.use_sql_comments : 쿼리를 출력할 때 주석도 함께 출력함
>- hibernate.id.new_generator_mappings : JPA 표준에 맞춘 새로운 키 생성 전략을 사용함

<br>

[목차로 이동](#목차)

>### 애플리케이션 개발
>```java
>package jpabook.start;
>
>import javax.persistence.*;
>import java.util.List;
>
>public class JpaMain {
>  public static void main(String[] args) {
>
>    // 엔티티 매니저 팩토리 생성
>    EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");
>    EntityManager em = emf.createEntityManager(); // 엔티티 매니저 생성
>
>    EntityTransaction tx = em.getTransaction(); // 트랜잭션 기능 획득
>
>    try {
>        tx.begin(); // 트랜잭션 시작
>        logic(em);  // 비즈니스 로직
>        tx.commit();// 트랜잭션 커밋
>    } catch (Exception e) {
>        e.printStackTrace();
>        tx.rollback(); // 트랜잭션 롤백
>    } finally {
>        em.close(); // 엔티티 매니저 종료
>    }
>
>    emf.close(); // 엔티티 매니저 팩토리 종료
>  }
>
>  public static void logic(EntityManager em) {
>    String id = "id1";
>    Member member = new Member();
>    member.setId(id);
>    member.setUsername("지한");
>    member.setAge(2);
>
>    // 등록
>    em.persist(member);
>
>    // 수정
>    member.setAge(20);
>
>    // 한 건 조회
>    Member findMember = em.find(Member.class, id);
>    System.out.println("findMember=" + findMember.getUsername() + ", age=" + findMember.getAge());
>
>    // 목록 조회
>    List<Member> members = em.createQuery("select m from Member m", Member.class).getResultList();
>    System.out.println("members.size=" + members.size());
>
>    // 삭제
>    em.remove(member);
>  }
>}
>```
>- 크게 3부분으로 나눠 설명
>>- 엔티티 매니저 설정
>>- 트랜잭션 관리
>>- 비즈니스 로직
>- 엔티티 매니저 설정
>>- 엔티티 매니저 팩토리 생성
>>>- JPA를 시작하려면 우선 persistence.xml 의 설정 정보를 사용하여 엔티티 매니저 팩토리를 생성함
>>>- Persistence 클래스는 엔티티 매니저 팩토리를 생성하여 JPA를 사용할 수 있게 준비함
>>>>```java
>>>>EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");
>>>>```
>>>- 해당 코드를 실행 시 META-INF/persistence.xml 에서 이름이 jpabook인 영속성 유닛을 찾아 엔티티 매니저 팩토리를 생성함
>>>- 이때 persistence.xml 의 설정 정보를 읽어 JPA를 동작시키기 위한 기반 객체를 만들고 JPA 구현체에 따라서는 DB 커넥션 풀도 생성하므로 엔티티 매니저 팩토리의 생성 비용은 아주 크므로 애플리케이션 전체에서 딱 한 번만 생성하고 공유하여 사용하는것을 권장함
>>- 엔티티 매니저 생성 
>>>```java
>>>EntityManager em = emf.createEntityManager();
>>>```
>>>- 엔티티 매니저 팩토리에서 엔티티 매니저를 생성함
>>>- JPA의 기능 대부분은 엔티티 매니저가 제공함
>>>>- 대표적으로 엔티티 매니저를 사용해서 엔티티를 DB에 CRUD할 수 있음
>>>- 엔티티 매니저는 내부에 데이터소스(DB 커넥션)를 유지하면서 DB와 통신하므로 애플리케이션 개발자는 엔티티 매니저를 가상의 DB로 생각할 수 있음
>>>- 참고로 엔티티 매니저는 DB 커넥션과 밀접한 관계가 있으므로 스레드간에 공유하거나 재사용하면 안됨
>>- 종료
>>>```java
>>>em.close();
>>>emf.close();
>>>```
>>>- 사용이 끝난 엔티티 매니저는 다음처럼 반드시 종료해야하며, 애플리케이션을 종료할 때 엔티티 매니저 팩토리도 종료해야 함
>- 트랜잭션 관리
>>```java
>>EntityTransaction tx = em.getTransaction();
>>
>>try {
>>    tx.begin();
>>    logic(em);
>>    tx.commit();
>>} catch (Exception e) {
>>    tx.rollback();
>>}
>>```
>>- JPA에서 트랜잭션 없이 데이터를 변경하면 예외가 발생하므로 JPA를 사용하면 항상 트랜잭션 안에서 데이터를 변경해야함
>>- 트랜잭션을 시작하려면 엔티티 매니저에서 트랜잭션 API를 받아와야함
>>- 트랜잭션 API를 사용하여 비즈니스 로직이 정상 동작하면 트랜잭션을 커밋하고 예외가 발생하면 트랜잭션을 롤백함
>- 비즈니스 로직
>>```java
>>public static void logic(EntityManager em) {
>>  String id = "id1";
>>  Member member = new Member();
>>  member.setId(id);
>>  member.setUsername("지한");
>>  member.setAge(2);
>>
>>  em.persist(member);
>>
>>  member.setAge(20);
>>
>>  Member findMember = em.find(Member.class, id);
>>  System.out.println("findMember=" + findMember.getUsername() + ", age=" + findMember.getAge());
>>
>>  List<Member> members = em.createQuery("select m from Member m", Member.class).getResultList();
>>  System.out.println("members.size=" + members.size());
>>
>>  em.remove(member);
>>}
>>```
>>- 출력 결과
>>>```
>>>findMember=지한, age=20
>>>members.size=1
>>>```
>>- 비즈니스 로직을 보면 CRUD 작업이 엔티티 매니저를 통해서 수행되는 것을 알 수 있음
>>- 엔티티 매니저는 객체를 저장하는 가상의 DB처럼 보임
>>- 등록
>>>- 엔티티를 저장하려면 엔티티 매니저의 persist() 메서드에 저장할 엔티티를 넘겨주면 됨
>>>- JPA는 회원 엔티티의 매핑 정보(어노테이션)을 분석해서 다음과 같은 SQL을 만들어 DB에 전달함
>>>>```sql
>>>>INSERT INTO MEMBER (ID, NAME, AGE) VALUES ('id1', '지한', 2)
>>>>```
>>- 수정
>>>- 엔티티를 수정한 후 수정 내용을 반영하려면 em.update() 같은 메서드를 호출해야 할 것 같은데 단순히 엔티티의 값만 변경하였음
>>>- JPA는 어떤 엔티티가 변경되었는지 추적하는 기능을 갖추고 있어서 엔티티의 값만 변경하면 다음과 같은 UPDATE SQL을 생성하여 DB에 값을 변경함
>>>>```sql
>>>>UPDATE MEMBER
>>>>  SET AGE=20, NAME='지한'
>>>>WHERE ID='id1'
>>>>```
>>- 수정
>>>- 엔티티를 삭제하려면 엔티티 매니저의 remove() 메서드에 삭제하려는 엔티티를 넘겨줌
>>>- JPA는 다음 DELETE SQL을 생성하여 실행함
>>>>```sql
>>>>DELETE FROM MEMBER WHERE ID='id1'
>>>>```
>>- 한 건 조회
>>>- find() 메서드는 조회할 엔티티 타입과 @Id로 DB 테이블의 기본 키와 매핑한 식별자 값으로 엔티티 하나를 조회하는 가장 단순한 조회 메서드임
>>>- 해당 메서드를 호출하면 다음 SELECT SQL을 생성하여 DB에 조회한 결과 값으로 엔티티를 생성하여 반환함
>>>>```sql
>>>>SELECT * FROM MEMBER WHERE ID='id1'
>>>>```
>- JPQL
>>- JPA를 사용하면 애플리케이션 개발자는 엔티티 객체를 중심으로 개발하고 DB에 대한 처리는 JPA에 맡겨야 함
>>- JPA는 엔티티 객체를 중심으로 개발하므로 검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색해야 함
>>- 테이블이 아닌 엔티티 객체를 대상으로 검색하려면 DB의 모든 데이터를 애플리케이션으로 불러와서 엔티티 객체로 변경한 다음 검색해야하는데 사실상 불가능함
>>- 애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL을 사용해야 함
>>- JPA는 JPQL (Java Persistence Query Language) 이라는 쿼리 언어로 문제를 해결함
>>- JPQL은 SQL을 추상화한 객체지향 쿼리 언어임
>>- JPQL은 SQL 문법과 거의 유사하여 SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN 등을 사용할 수 있음
>- JPQL과 SQL의 차이점
>>- JPQL은 엔티티 객체를 대상으로 쿼리함 즉, 클래스와 필드를 대상으로 쿼리함
>>- SQL은 DB 테이블을 대상으로 쿼리함
>- 하나 이상의 회원 목록을 조회하는 코드
>>```java
>>TypedQuery<Member> query = em.createQuery("select m from Member m", Member.class);
>>List<Member> members = query.getresultList();
>>```
>>- select m from Member m 이 JPQL이고, 여기서 from Member 는 회원 엔티티 객체를 말하는 것이지 MEMBER 테이블이 아님
>>- JPQL은 DB 테이블을 전혀 알지 못함
>>- JPQL을 사용하려면 먼저 em.createQuery(JPQL, 반환 타입) 메서드를 실행하여 쿼리 객체를 생성한 후 쿼리 객체의 getResultList() 메서드를 호출함
>>- JPA는 JPQL을 분석하여 적절한 SQL을 만들어 DB에서 데이터를 조회함
>>>```sql
>>>SELECT M.ID, M.NAME, M.AGE FROM MEMBER M
>>>```

<br>

[목차로 이동](#목차)

---

## 영속성 관리

>### 영속성 관리 개요
>- JPA가 제공하는 기능은 크게 엔티티와 테이블을 매핑하는 설계 부분과 매핑한 엔티티를 실제 사용하는 부분으로 나눌 수 있음
>- 해당 챕터에서는 매핑한 엔티티를 엔티티 매니저를 통해 어떻게 사용하는지 설명함
>- 엔티티 매니저는 엔티티를 CRUD 등의 엔티티와 관련된 모든 일을 처리하며 이름 그대로 엔티티를 관리하는 관리자임
>- 개발자 입장에서 엔티티 매니저는 엔티티를 저장하는 가상의 DB로 생각하면됨

<br>

[목차로 이동](#목차)

>### 엔티티 매니저 팩토리와 엔티티 매니저
>- DB를 하나만 사용하는 애플리케이션은 일반적으로 EntityManagerFactory를 하나만 생성함
>>```java
>>// 공장 만들기, 비용이 아주 많이 듬
>>EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");
>>```
>>- 호출 시 META-INF/persistence.xml에 있는 정보를 바탕으로 EntityManagerFactory를 생성함
>>```java
>>// 공장에서 엔티티 매니저 생성, 비용이 거의 들지 않음
>>EntityManager em = emf.createEntityManager();
>>```
>>- 엔티티 매니저 팩토리는 이름 그대로 엔티티 매니저를 만드는 공장인데, 공장을 만드는 비용은 상당히 크므로 한 개만 만들어서 애플리케이션 전체에서 공유하도록 설계되어 있음
>>- 반면에 공장에서 엔티티 매니저를 만드는 비용은 거의 들지않음
>>- 엔티티 매니저 팩토리는 여러 스레드가 동시에 접근해도 안전하므로 서로 다른 스레드 간 공유해도 되지만, 엔티티 매니저는 여러 스레드가 동시에 접근하면 동시성 문제가 발생하므로 스레드 간에 절대 공유하면 안됨
>>- 엔티티 매니저는 DB연결이 꼭 필요한 시점까지 커넥션을 얻지 않음
>>- 하이버네이트를 포함한 JPA 구현체들은 엔티티 매니저 팩토리를 생성할 때 커넥션풀도 만드는데 이는 J2SE 환경에서 사용하는 방법이고, JPA를 J2EE 환경(Spring 포함)에서 사용하면 해당 컨테이너가 제공하는 데이터소스를 사용함

<br>

[목차로 이동](#목차)

>### 영속선 컨텍스트란?
>- JPA를 이해하는 데 가장 중요한 용어로서 번역에 어려움이 있으나 해석하자면 엔티티를 영구 저장하는 환경이라는 의미임
>- 엔티티 매니저로 엔티티를 저장하거나 조회하면 엔티티 매니저는 영속성 컨텍스트에 엔티티를 보관하고 관리함
>```java
>em.persist(member);
>```
>>- 해당 코드를 단순히 회원 엔티티를 저장한다고 표현했으나 정확히 이야기하면 엔티티 매니저를 사용해서 회원 엔티티를 영속성 컨텍스트에 저장한다는 의미임
>- 영속성 컨텍스트는 논리적인 개념에 가까워 눈에 보이지 않음
>- 영속성 컨텍스트는 엔티티 매니저를 생성할 때 하나 만들어지며 엔티티 매니저를 통해서 영속성 컨텍스트에 접근할 수 있고, 영속성 컨텍스트를 관리할 수 있음

<br>

[목차로 이동](#목차)

>### 엔티티의 생명주기
>- 엔티티의 4가지 상태
>>- 비영속 (new/transient) : 영속성 컨텍스트와 전혀 관계가 없는 상태
>>- 영속 (managed) : 영속성 컨텍스트에 저장된 상태
>>- 준영속 (detached) : 영속성 컨텍스트에 저장되었다가 분리된 상태
>>- 삭제 (removed) : 삭제된 상태
>- 비영속
>>- 엔티티 객체를 생성했을때 순수한 객체 상태이며 아직 저장하지 않아 영속성 컨텍스트나 데이터베이스와는 전혀 관련이 없는 상태임
>>```java
>>// 객체를 생성한 상태
>>Member member = new Member();
>>member.setId("member1");
>>member.setUsername("회원1");
>>```
>- 영속
>>- 엔티티 매니저를 통해 영속성 컨텍스트에 저장되어 영속성 컨텍스트가 관리하는 엔티티의 상태를 영속 상태라 함
>>- em.find() 나 JPQL 을 사용해서 조회한 엔티티도 영속성 컨텍스트가 관리하는 영속 상태임
>>```java
>>em.persist(member);
>>```
>- 준영속
>>- 영속성 컨텍스트가 관리하던 영속 상태의 엔티티가 영속성 컨텍스트가 관리하지 않게되면 해당 엔티티의 상태를 준영속 상태라 함
>>- em.detach() 를 호출하여 특정 엔티티를 준영속 상태로 만들 수 있음
>>- em.close() 를 호출하여 영속성 컨텍스트를 닫거나 em.clear() 를 호출하여 영속성 컨텍스트를 초기화해도 영속성 컨텍스트가 관리하던 영속 상태의 엔티티는 준영속 상태가 됨
>>```java
>>em.detach(member);
>>```
>- 삭제
>>- 엔티티를 영속성 컨텍스트와 데이터베이스에서 삭제함
>>```java
>>em.remove(member);
>>```

<br>

[목차로 이동](#목차)

>### 영속성 컨텍스트의 특징
>- 특징
>>- 영속성 컨테이너와 식별자 값
>>>- 영속성 컨텍스트는 엔티티를 식별자 값(@Id로 테이블의 기본 키와 매핑한 값)으로 구분함
>>>- 따라서 영속 상태는 식별자 값이 반드시 있어야 하며 식별자 값이 없으면 예외가 발생함
>>- 영속성 컨테이너와 데이터베이스 저장
>>>- JPA는 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 엔티티를 DB에 반영하는데 이것을 플러시(flush)라 함
>>- 영속성 컨텍스트가 엔티티를 관리할때의 장점
>>>1. 1차 캐시
>>>2. 동일성 보장
>>>3. 트랜잭션을 지원하는 쓰기 지연
>>>4. 변경 감지
>>>5. 지연 로딩
>- 엔티티 조회
>>- 1차 캐시
>>>- 영속성 컨텍스트는 내부에 캐시를 가지고 있는데 이것을 1차 캐시라 함
>>>- 영속 상태의 엔티티는 모두 1차 캐시에 저장되며 저장 형태를 예시로 설명하면 영속성 컨텍스트 내부에 Map이 하나 있는데 키는 @Id로 매핑한 식별자고 값은 엔티티 인스턴스임
>>>- 코드의 흐름을 통한 설명
>>>```java
>>>// 엔티티를 생성한 상태(비영속)
>>>Member member = new Member();
>>>member.setId("member1");
>>>member.setUsername("회원1");
>>>
>>>// 엔티티를 영속 (1차 캐시에 회원 엔티티를 저장함, DB엔 저장되지 않은 상태)
>>>em.persist(member);
>>>
>>>// 엔티티 조회 (1차 캐시에서 엔티티를 찾고 없으면 DB에서 조회함)
>>>Member member = em.find(Member.class, "member1");
>>>```
>>- 영속 엔티티의 동일성 보장
>>```java
>>Member a = em.find(Member.class, "member1");
>>Member b = em.find(Member.class, "member2");
>>
>>System.out.println(a == b);
>>```
>>>- em.find(Member.class, "member1") 를 반복해서 호출해도 영속성 컨텍스트는 1차 캐시에 있는 같은 엔티티 인스턴스를 반환하므로 결과는 참임
>>>- 결론적으로 영속성 컨텍스트는 성능상 이점과 엔티티의 동일성을 보장함
>- 엔티티 등록
>```java
>EntityManager em = emf.createEntityManager();
>EntityTransaction transaction = em.getTransaction();
>// 엔티티 매니저는 데이터 변경 시 트랜잭션을 시작해야 함
>transaction.begin();
>
>em.persist(memberA);
>em.persist(memberB);
>// 여기까지 INSERT SQL을 DB에 보내지 않음
>
>// 커밋하는 순간 DB에 INSERT SQL을 보냄
>transaction.commit();
>```
>>- 쓰기 지연(transactional write-behind)
>>>- 엔티티 매니저는 트랜잭션을 커밋하기 직전까지 데이터베이스에 엔티티를 저장하지 않고 내부 쿼리 저장소에 INSERT SQL을 차곡차곡 모아두고 트랜잭션을 커밋할 때 모아둔 쿼리를 DB에 보내는데 이것을 트랜잭션을 지원하는 쓰기 지연이라 함
>>>- 트랜잭션을 커밋하면 엔티티 매니저는 우선 영속성 컨텍스트를 플러시함
>>>>- 플러시는 영속성 컨텍스트의 변경 내용을 DB에 동기화하는 작업으로, 쓰기 지연 SQL 저장소에 모인 쿼리를 DB에 보내므로 등록, 수정, 삭제한 엔티티가 이때 DB에 반영됨
>>>- 영속성 컨텍스트의 변경 내용을 DB에 동기화한 후에 실제 DB 트랜잭션을 커밋함
>>- 트랜잭션을 지원하는 쓰기 지연이 가능한 이유
>>>- 등록 쿼리를 그때 그때 DB에 전달해도 트랜잭션을 커밋하지 않으면 등록하지 않은 상태와 같기 때문에 어떻게든 커밋 직전에만 DB에 SQL을 전달하면 문제가 없음
>- 엔티티 수정
>>- SQL 수정 쿼리의 문제점
>>>- 회원의 이름과 나이를 변경하는 기능을 개발했는데 회원의 등급을 변경하는 기능이 추가되면 회원의 등급을 변경하는 수정 쿼리를 추가로 작성하여 2개의 수정쿼리가 되며, 둘을 합쳐 하나의 수정 쿼리로 개선할 수 있을 거라고 생각하기 쉬움
>>>- 합친 쿼리를 사용해서 이름과 나이를 변경하는 데 실수로 등급 정보를 입력하지 않거나, 등급을 변경하는 데 실수로 이름과 나이를 입력하지 않을 수 있으며 또는 기존 코드를 전부 수정해야하는 문제가 있어 수정 쿼리를 상황에 따라 계속 추가하게됨
>>>- 이런 개발 방식의 문제점은 수정 쿼리가 많아지는 것은 물론이고 비즈니스 로직을 분석하기 위해 SQL을 계속 확인해야하므로 직접적이든 간접적이든 비즈니스 로직이 SQL을 의존하게됨
>>- 변경 감지 (dirty checking)
>>>- JPA로 엔티티를 수정할 때 트랜잭션 커밋 직전에 em.update()를 메서드를 실행해야 할 것 같지만 이런 메서드는 없음
>>>- JPA는 단순히 엔티티를 조회해서 데이터만 변경하면 엔티티의 변경사항을 DB에 자동으로 반영하는 변경 감지 기능을 제공함
>>>- JPA는 엔티티를 영속성 컨텍스트에 보관할 때, 최초 상태를 복사해서 저장해두는데 이것을 스냅샷이라 하며, 이후 플러시 시점에 스냅샷과 엔티티를 비교해서 변경된 엔티티를 찾음
>>>- 변경 감지는 영속성 컨텍스트가 관리하는 영속 상태의 엔티티에만 적용됨
>>>- 변경 감지로 인해 실행된 UPDATE SQL은 동적으로 수정 쿼리가 생성되는 것이 아니라 엔티티의 모든 필드를 업데이트함
>>>>- 모든 필드를 사용하면 DB에 보내는 데이터의 전송량이 증가하는 단점이 있음
>>>>- 모든 필드를 사용하면 수정 쿼리가 바인딩 되는 데이터를 제외하면 항상 같기때문에 애플리케이션 로딩 시점에 수정 쿼리를 미리 생성해두고 재사용할 수 있음
>>>>- DB에 동일한 쿼리를 보내면 DB는 이전에 한 번 파싱된 쿼리를 재사용할 수 있음
>>>- 필드가 많거나 저장되는 내용이 너무 크면 하이버네이트 확장 기능을 통해 수정된 데이터만 사용해서 동적으로 UPDATE SQL 을 생성하는 전략을 선택하면 됨
>- 엔티티 삭제
>>- 엔티티를 삭제하기 위해선 먼저 삭제 대상 엔티티를 조회해야 함
>>```java
>>Member memberA = em.find(Member.class, "memberA"); // 삭제 대상 엔티티 조회
>>em.remove(memberA); // 엔티티 삭제
>>```
>>- em.remove에 삭제 대상 엔티티를 넘겨주면 삭제 쿼리를 쓰기 지연 SQL 저장소에 등록하여 이후 트랜잭션을 커밋해서 플러시를 호출하면 실제 DB에 삭제 쿼리를 전달함
>>- 삭제된 엔티티는 재사용하지 말고 자연스럽게 가비지 컬렉션의 대상이 되도록 두는 것이 좋음

<br>

[목차로 이동](#목차)

>### 플러시
>- 개요
>>- 플러시(flush())는 영속성 컨텍스트의 변경 내용을 DB에 반영함
>>- 플러시를 실행하면 구체적으로 벌어지는 과정
>>>1. 변경 감지가 동작해서 영속성 컨텍스트에 있는 모든 엔티티를 스냅샷과 비교해서 수정된 엔티티를 찾고, 수정된 엔티티는 수정 쿼리르 만들어 쓰기 지연 SQL 저장소에 등록함
>>>2. 쓰기 지연 SQL 저장소의 쿼리를 DB에 전송함 (등록 ,수정 ,삭제 쿼리)
>>- 영속성 컨텍스트를 플러시하는 3가지 방법
>>>1. em.flush() 를 직접 호출함
>>>2. 트랜잭션 커밋 시 플러시가 자동 호출됨
>>>3. JPQL 쿼리 실행 시 플러시가 자동 호출됨
>>- 직접 호출
>>>- 엔티티 매니저의 flush() 메서드를 직접 호출해서 영속성 컨텍스트를 강제로 플러시함
>>>- 테스트나 다른 프레임워크와 JPA를 함께 사용할 때를 제외하고 거의 사용하지 않음
>>- 트랜잭션 커밋 시 플러시 자동 호출
>>>- DB에 변경 내용을 SQL로 전달하지 않고 트랜잭션만 커밋하면 어떤 데이터도 DB에 반영되지 않으므로 트랜잭션을 커밋하기 전에 반드시 플러시를 호출해서 영속성 컨텍스트의 변경 내용을 DB에 반영해야함
>>>- JPA는 이런 문제를 예방하기 위해 트랜잭션을 커밋할 때 플러시를 자동으로 호출함
>>- JPQL 쿼리 실행 시 플러시 자동 호출
>>>- JPQL이나 Criteria 같은 객체지향 쿼리를 호출할 때도 플러시가 자동으로 실행됨
>>>- JPQL은 SQL로 변환되어 DB에서 엔티티를 조회하므로 DB에 없으면 쿼리 결과로 조회되지 않는 문제가 있음
>>>- JPA는 이런 문제를 예방하기 위해 JPQL을 실행할 때도 플러시를 자동 호출함
>- 플러시 모드 옵션
>>- 엔티티 매니저에 플러시 모드를 직접 지정하려면 javax.persistence.FlushModeType을 사용함
>>>- FlushModeType.AUTO : 커밋이나 쿼리를 실행할 때 플러시 (기본값)
>>>- FlushModeType.COMMIT : 커밋할 때만 플러시
>- 정리
>>-플러시라는 이름으로 인해 영속성 컨텍스트에 보관된 엔티티를 지운다고 생각하면 안되고, 영속성 컨텍스트의 변경 내용을 DB에 동기화하는 것임

<br>

[목차로 이동](#목차)

>### 준영속
>- 개요
>>- 영속성 컨텍스트가 관리하는 영속 상태의 엔티티가 영속성 컨텍스트에서 분리된 것을 준영속 상태라고 하며, 준영속 상태의 엔티티는 영속성 컨텍스트가 제공하는 기능을 사용할 수 없음
>>- 영속 상태의 엔티티를 준영속 상태로 만드는 3가지 방법
>>>1. em.detach(entity) : 특정 엔티티만 준영속 상태로 전환함
>>>2. em.clear() : 영속성 컨텍스트를 완전히 초기화함
>>>3. em.close() : 영속성 컨텍스트를 종료함
>- 엔티티를 준영속 상태로 전환 : detach()
>>- em.detach() 메서드는 특정 엔티티를 준영속 상태로 만듬
>>- 영속성 컨텍스트에게 해당 엔티티를 관리하지 말라는 의미이며 호출하는 순간 1차 캐시부터 쓰기 지연 SQL 저장소까지 해당 엔티티를 관리하기 위한 모든 정보가 제거됨
>>- 정리하자면 준영속 상태는 영속성 컨텍스트로부터 분리된 상태임
>- 영속성 컨텍스트 초기화 : clear()
>>- em.clear() 메서드는 영속성 컨텍스트를 초기화해서 해당 영속성 컨텍스트의 모든 엔티티를 준영속 상태로 만듬
>- 영속성 컨텍스트 종료 : close()
>>- 영속성 컨텍스트를 종료하면 해당 영속성 컨텍스트가 관리하던 영속 상태의 엔티티가 모두 준영속 상태가 됨
>>- 영속 상태의 엔티티는 주로 영속성 컨텍스트가 종료되면서 준영속 상태가 되며, 개발자가 직접 준영속 상태로 만드는 일은 드뭄
>- 준영속 상태의 특징
>>- 거의 비영속 상태에 가까움
>>>- 영속성 컨텍스트가 관리하지 않으므로 1차 캐시, 쓰기 지연, 변경 감지, 지연 로딩을 포함한 영속성 컨텍스트가 제공하는 어떠한 기능도 동작하지 않음
>>- 식별자 값을 가지고 있음
>>>- 비영속 상태는 식별자 값이 없을 수도 있지만 준영속 상태는 이미 한 번 영속 상태였으므로 반드시 식별자 값을 가지고 있음
>>- 지연 로딩을 할 수 없음
>>>- 지연 로딩(LAZY LOADING)은 실제 객체 대신 프록시 객체를 로딩해두고 해당 객체를 실제 사용할 때 영속성 컨텍스트를 통해 데이터를 불러오는 방법이기 때문에, 준영속 상태는 영속성 컨텍스트가 관리하지 않아 지연 로딩 시 문제가 발생함
>- 병합 : merge()
>>- 준영속 상태의 엔티티를 다시 영속 상태로 변경하려면 병합을 사용함
>>- merge() 메서드는 준영속 상태의 엔티티를 받아서 그 정보로 새로운 영속 상태의 엔티티를 반환함
>>- 준영속 병합
>>```java
>>static EntityManagerFactory emf = Persistence.createEntityManagerFactory("jpabook");
>>
>>public static void main(String args[]) {
>>  Member member = createMember("memberA", "회원1");
>>  member.setUsername("회원명변경"); // 준영속 상태에서 변경
>>  mergeMember(member);
>>}
>>
>>static Member createMember(String id, String username) { 
>>  //== 영속성 컨텍스트1 시작 ==//
>>  EntityManager em1 = emf.createEntityManager();
>>  EntityTransaction tx1 = em1.getTransaction();
>>  tx1.begin();
>>
>>  Member member = new Member();
>>  member.setId(id);
>>  member.setUsername(username);
>>
>>  em1.persist(member);
>>  tx1.commit();
>>
>>  em1.close();
>>  //== 영속성 컨텍스트1 종료 ==//
>>
>>  return member;
>>}
>>
>>static void mergeMember(Member member) {
>>  //== 영속성 컨텍스트2 시작 ==//
>>  EntityManager em2 = emf.createEntityManager();
>>  EntityTransaction tx2 = em2.getTransaction();
>>
>>  tx2.begin();
>>  Member mergeMember = em2.merge(member);
>>  
>>  // 향후 추천되는 방식
>>  member = em2.merge(member);
>>  tx2.commit();
>>
>>  // 준영속 상태
>>  System.out.println("member = " + member.getUsername());
>>
>>  // 영속 상태
>>  System.out.println("mergeMember = " + mergeMember.getUsername());
>>
>>  System.out.println("em2 contains member = " + em2.contains(member));
>>  System.out.println("em2 contains mergeMember = " + em2.contains(mergeMember));
>>  em2.close();
>>  //== 영속성 컨텍스트2 종료 ==//
>>}
>>```
>>>1. member 엔티티는 createMember() 메서드의 영속성 컨텍스트1 에서 영속 상태였다가 영속성 컨텍스트1이 종료되면서 준영속 상태가 됨
>>>2. member.setUserName("회원명변경")을 호출해서 회원 이름을 변경했지만 준영속 상태인 member 엔티티를 관리하는 영속성 컨텍스트가 더는 존재하지 않으므로 수정사항을 DB에 반영할 수 없음
>>>3. 준영속 상태의 엔티티를 수정하려면 준영속 상태를 다시 영속 상태로 변경해야하는데 이때 em2.merge(member)를 호출해서 영속 상태로 변경하면 트랜잭션을 커밋할 때 수정했던 회원 명이 DB에 반영됨
>>- 비영속 병합
>>>- 병합은 비영속 엔티티도 영속 상태로 만들 수 있음
>>>- 파라미터로 넘어온 엔티티의 식별자 값으로 영속성 컨텍스트를 조회하고 찾는 엔티티가 없으면 DB에서 조회하고, DB에서도 없으면 새로운 엔티티를 생성해서 병합함
>>>- 병ㅎ바은 준영속, 비영속을 신경 쓰지 않고 식별자 값으로 엔티티를 조회할 수 있으면 불러서 병합하고, 조회할 수 없으면 새로 생성해서 병합하므로 save or update 기능을 수행한다고 볼 수 있음

<br>

[목차로 이동](#목차)

>### 영속성 관리 정리
>- 엔티티 매니저는 엔티티 매니저 팩토리에서 생성하며 자바를 직접 다루는 J2SE 환경에서는 엔티티 매니저를 만들면 그 내부에 영속성 컨텍스트도 함께 만들어지며, 영속성 컨텍스트는 엔티티 매니저를 통해서 접근할 수 있음
>- 영속성 컨텍스트는 애플리케이션과 DB 사이에서 객체를 보관하는 가상의 DB 같은 역할을 하며, 영속성 컨텍스트 덕분에 1차 캐시, 동일성 보장, 트랜잭션을 지원하는 쓰기 지연, 변경 감지, 지연 로딩 기능을 사용할 수 있음
>- 영속성 컨텍스트에 저장한 엔티티는 플러시 시점에 DB에 반영되는데 일반적으로 트랜잭션을 커밋할 때 영속성 컨텍스트가 플러시됨
>- 영속성 컨텍스트가 관리하는 엔티티를 영속 상태의 엔티티라 하는데, 영속성 컨텍스트가 해당 엔티티를 더 이상 관리하지 못하면 그 엔티티는 준영속 상태의 엔티티라 하며 준영속 상태의 엔티티는 더는 영속성 컨텍스트의 관리를 받지 못하므로 영속성 컨텍스트가 제공하는 1차 캐시, 동일성 보장, 트랜잭션을 지원하는 쓰기 지연, 변경 감지, 지연 로딩 같은 기능들을 사용할 수 없음

<br>

[목차로 이동](#목차)

---

## 엔티티 매핑

>### 엔티티 매핑 개요
>- JPA를 사용하는 데 가장 중요한 일은 엔티티와 테이블을 정확히 매핑하는 것임
>>- JPA는 다양한 매핑 어노테이션을 지원하므로 매핑 어노테이션을 숙지하고 사용해야함
>- JPA가 지원하는 4가지 분류의 매핑 어노테이션
>>1. 객체와 테이블 매핑
>>>- @Entity, @Table
>>2. 기본 키 매핑
>>>- @Id
>>3. 필드와 컬럼 매핑
>>>- @Column
>>4. 연관관계 매핑
>>>- @ManyToOne, @JoinColumn

<br>

[목차로 이동](#목차)

>### @Entity
>- JPA를 사용해서 테이블과 매핑할 클래스는 @Entity 어노테이션을 필수로 붙여야 함
>>- @Entity 가 붙은 클래스는 JPA가 관리하는 것으로, 엔티티라 부름
>- 속성
>>- name
>>>- JPA에서 사용할 엔티티 이름을 지정함
>>>- 보통 기본값인 클래스 이름을 사용함
>>>>- 설정하지 않으면 클래스 이름을 그대로 사용함
>>>- 만약 다른 패키지에 이름이 같은 엔티티 클래스가 있다면 이름을 지정해서 충돌하지 않도록 해야 함
>- @Entity 적용 시 주의사항
>>- 기본 생성자는 필수임 (파라미터 없는 public 또는 protected 생성자)
>>- final 클래스, enum, interface, inner 클래스에는 사용할 수 없음
>>- 저장할 필드에 final 을 사용하면 안됨

<br>

[목차로 이동](#목차)

>### @Table
>- 엔티티와 매핑할 테이블을 지정함
>- 속성
>>- name
>>>- 매핑할 테이블 이름
>>>>- 생략시 매핑한 엔티티 이름을 테이블 이름으로 사용함
>>- catalog
>>>- catalog 기능이 있는 DB 에서 catalog 를 매핑함
>>- schema
>>>- schema 기능이 있는 DB 에서 schema 를 매핑함
>>- uniqueConstraints(DDL)
>>>- DDL 생성 시에 유니크 제약조건을 만듬
>>>- 2개 이상의 복합 유니크 제약조건도 만들 수 있음
>>>- 이 기능은 스키마 자동 생성 기능을 사용해서 DDL을 만들 때만 사용됨

<br>

[목차로 이동](#목차)

>### 다양한 매핑 사용
>- 추가된 요구사항
>>1. 회원을 일반 회원과 관리자로 구분해야 함
>>2. 회원 가입일과 수정일이 있어야 함
>>3. 회원을 설명할 수 있는 필드가 있어야 하며, 길이 제한이 없음
>>```java
>>// Member.java
>>import javax.persistence.*;
>>import java.util.Date;
>>
>>@Entity
>>@Table(name="MEMBER")
>>public class Member {
>>
>>  @Id
>>  @Column(name = "ID")
>>  private String id;
>>
>>  @Column(name = "NAME")
>>  private String username;
>>
>>  private Integer age;
>>
>>  //=== 추가
>>  @Enumerated(EnumType.STRING)
>>  private RoleType roleType; 
>>
>>  @Temporal(TemporalType.TIMESTAMP)
>>  private Date createdDate;
>>
>>  @Temporal(TemporalType.TIMESTAMP)
>>  private Date lastModifiedDate;
>>
>>  @Lob
>>  private String description;
>>
>>  @Transient
>>  private String temp;
>>
>>  //Getter, Setter
>>  ...
>>}
>>
>>// RoleType.java
>>public enum RoleType {
>>  ADMIN, USER
>>}
>>```
>>>- roleType
>>>>- 자바의 enum을 사용하여 회원의 타입을 구분함
>>>>- 자바의 enum을 사용하려면 @Enumerated 어노테이션으로 매핑해야 함
>>>- createdDate, lastModifiedDate
>>>>- 자바의 날짜 타입은 @Temporal 을 사용하여 매핑함
>>>- description
>>>>- 회원을 설명하는 필드는 길이 제한이 없음
>>>>- 따라서 데이터베이스의 VARCHAR 타입 대신에 CLOB 타입으로 지정해야 함
>>>>- @Lob 을 사용하면 CLOB, BLOC 타입을 매핑할 수 있음

<br>

[목차로 이동](#목차)

>### 데이터베이스 스키마 자동 생성
>- 스키마 자동 생성 개요
>>- JPA는 DB 스키마를 자동으로 생성하는 기능을 지원함
>>- 클래스의 매핑 정보를 보면 어떤 테이블에 어떤 칼럼을 사용하는지 알 수 있음
>>- JPA는 매핑 정보와 DB 방언을 사용하여 DB 스키마를 생성함
>- 자동 생성 기능 사용
>>- persistence.xml 에 속성 추가
>>>- \<property name="hibernate.hbm2ddl.auto" value="create" />
>>>>- 속성 추가 시 애플리케이션 실행 시점에 DB 테이블을 자동으로 생성함
>>>- \<property name="hibernate.show_sql" value="true" />
>>>>- 속성 추가 시 콘솔에 실행되는 테이블 생성 DDL 을 출력할 수 있음
>>- 자동 생성되는 DDL은 지정한 DB 방언에 따라 달라짐
>>- 스키마 자동 생성 기능이 만든 DDL 은 운영 환경에서 사용할 만큼 완벽하지는 않으므로 개발 환경에서 사용하거나 매핑을 어떻게 해야하는지 참고하는 정도로만 사용하는 것이 좋음
>- hibernate.hbm2ddl.auto 속성
>
>옵션|설명
>:--|:--
>create | 기존 테이블을  삭제하고 새로 생성함, DROP + CREATE
>create-drop | create 속성에 추가로 애플리케이션을 종료할 때 생성한 DDL을 제거함, DROP + CREATE + DROP
>update | DB 테이블과 엔티티 매핑 정보를 비교해서 변경 사항만 수정함
>validate | DB 테이블과 엔티티 매핑정보를 비교해서 차이가 있으면 경고를 남기고 애플리케이션을 실행하지 않음, 해당 설정은 DDL 을 수정하지 않음
>none | 자동 생성 기능을 사용하지 않으려면 hibernate.hbm2ddl.auto 속성 자체를 삭제하거나 유효하지 않은 옵션 값을 주면 됨, 참고로 none은 유효하지 않은 옵션 값임
>- 이름 매핑 전략 변경
>>- java 언어는 관례 상 카멜 케이스를 사용하지만, DB는 관례 상 스네이크 케이스를 사용함
>>- 객체 변수를 카멜 케이스로 DB에 매핑하고 싶다면 spring.jpa.hibernate.naming.physical-strategy 값을 변경하면 됨
>>>- spring.jpa.hibernate.naming.physical-strategy = org.hibernate.boot.model.naming.SpringPhysicalNamingStrategy

<br>

[목차로 이동](#목차)

>### DDL 생성 기능
>- 제약조건
>>- not null, 길이 제한
>>>- @Column(name = "NAME", nullable = false, length = 10)
>>- 테이블 유니크 제약조건
>>```java
>>@Entity
>>@Table(name="MEMBER", uniqueConstraints = {@UniqueConstraint(
>>       name = "NAME_AGE_UNIQUE",
>>       columnNames = {"NAME", "AGE"} )})
>>public class Member {
>>```
>>- 이런 기능들은 DDL을 자동으로 생성할 때만 사용되고 JPA의 실행 로직에는 영향을 주지 않음
>>>- 하지만 해당 기능들을 사용 시 애플리케이션 개발자가 엔티티만 보고도 손쉽게 다양한 제약조건을 파악할 수 있는 장점이 있음

<br>

[목차로 이동](#목차)

>### 기본 키 매핑
>- JPA가 제공하는 DB 기본 키 생성 전략
>>- 직접 할당 : 기본 키를 애플리케이션에서 직접 할당함
>>- 자동 생성 : 대리 키 사용 방식
>>>- IDENTITY : 기본 키 생성을 DB에 위임함
>>>- SEQUENCE : DB 시퀀스를 사용하여 기본 키를 할당함
>>>- TABLE : 키 생성 테이블을 사용함
>>- 자동 생성 전략이 다양한 이유는 DB 벤더마다 지원하는 방식이 다르기 때문임
>>- 기본 키를 직접 할당하려면 @Id 만 사용하면 되고, 자동 생성 전략을 사용하려면 @Id 에 @GeneratedValue 를 추가하고 원하는 키 생성 전략을 선택하면 됨
>>- 키 생성 전략을 사용하려면 persistence.xml 에 hibernate.id.new_generator_mappings=true 속성을 추가해야 함
>- 기본 키 직접 할당 전략
>>- 기본 키를 직접 할당하려면 @Id 로 매핑함
>>- @Id 적용 가능 자바 타입
>>>- 자바 기본형
>>>- 자바 래퍼형
>>>- String
>>>- java.util.Date
>>>- java.sql.Date
>>>- java.math.BigDecimal
>>>- java.math.BigInteger
>>- 기본 키 직접 할당 전략은 em.persist() 로 엔티티를 저장하기 전에 애플리케이션에서 기본 키를 직접 할당하는 방법임
>- IDENTITY 전략
>>- 기본 키 생성을 DB에 위임하는 전략으로 주로 MySQL, PostgreSQL, SQL Server, DB2 에서 사용함
>>- 개발자가 엔티티에 직접 식별자를 할당하면 @Id 어노테이션만 있으면 되지만 식별자가 생성되는 경우에는 @GeneratedValue 어노테이션을 사용하고 식별자 생성 전략을 선택해야 함
>>- @GeneratedValue 의 strategy 속성 값을 GenerationType.IDENTITY 로 지정함
>>- 이 전략을 사용하면 JPA 는 기본 키 값을 얻어오기 위해 DB 를 추가로 조회함
>>```java
>>// IDENTITY 매핑 코드
>>@Entity
>>public class Board {
>>  @Id
>>  @GeneratedValue(strategy=GenerationType.IDENTITY)
>>  private Long id;
>>  ...
>>}
>>
>>// IDENTITY 사용 코드
>>private static void logic(EntityManager em) {
>>  Board board=new Board();
>>  em.persist(board);
>>  System.out.println("board.id="+board.getId()); // board.id=1
>>}
>>```
>>- 사용 코드를 보면 em.persist() 를 호출해서 엔티티를 저장한 직후에 할당된 식별자 값을 출력하는데, 출력된 값 1은 저장 시점에 DB가 생성한 값을 JPA가 조회한 것임
>>- 엔티티가 영속 상태가 되려면 식별자가 반드시 필요한데, IDENTITY 식별자 생성 전략은 엔티티를 DB 에 저장해야 식별자를 구할 수 있으므로 em.persist() 를 호출하는 즉시 INSERT SQL 이 DB에 전달되므로 해당 전략을 트랜잭션을 지원하는 쓰기 지연이 동작하지 않음
>- AUTO 전략
>>- DB 의 종류도 많고 기본 키를 만드는 방법도 다양하므로 GenerationType.AUTO 를 사용하여 선택한 DB 방언에 따라 IDENTITY, SEQUENCE, TABLE 전략 중 하나를 자동으로 선택하는 방법을 사용함
>>```java
>>// IDENTITY 매핑 코드
>>@Entity
>>public class Board {
>>  @Id
>>  @GeneratedValue(strategy=GenerationType.AUTO)
>>  private Long id;
>>  ...
>>}
>>
>>// @GeneratedValue.strategy 의 기본값은 AUTO 이므로 다음과 같이 사용해도 결과는 같음
>>@Entity
>>public class Board {
>>  @Id @GeneratedValue
>>  private Long id;
>>  ...
>>}
>>```
>>- AUTO 전략의 장점은 DB를 변경해도 코드를 수정할 필요가 없다는 것으로, 특히 키 생성 전략이 아직 확정되지 않은 개발 초기 단계나 프로토타입 개발 시 편리하게 사용할 수 있음

<br>

[목차로 이동](#목차)

>### 필드와 컬럼 매핑: 레퍼런스
>- 필드와 컬럼 매핑 분류
>>분류|매핑 어노테이션|설명
>>:--|:--|:--
>>필드와 컬럼 매핑|@Column|컬럼을 매핑함
>>&nbsp;|@Enumerated|자바의 enum 타입을 매핑함
>>&nbsp;|@Temporal|날짜 타입을 매핑함
>>&nbsp;|@Lob|BLOB, CLOB 타입을 매핑함
>>&nbsp;|@Transient|특정 필드를 DB에 매핑하지 않음
>>기타|@Access|JPA 가 엔티티에 접근하는 방식을 지정함
>- @Column
>>- 객체 필드를 테이블 컬럼에 매핑하며 가장 많이 사용되고 기능도 많음
>>>속성|기능|기본값
>>>:--|:--|:--
>>>name|필드와 매핑할 테이블의 컬럼 이름|객체의 필드 이름
>>>insertable(거의 사용하지 않음)|엔티티 저장 시 이 필드도 같이 저장함<br>false 로 설정하면 이 필드는 DB에 저장하지 않음<br>false 옵션은 읽기 전용일 때 사용함|true
>>>updatable(거의 사용하지 않음)|엔티티 수정 시 이 필드도 같이 수정함<br>false 로 설정하면 DB에 수정하지 않음<br>false 옵션은 읽기 전용일 때 사용함|true
>>>table(거의 사용하지 않음)|하나의 엔티티를 두 개 이상의 테이블에 매핑할 때 사용함<br>지정한 필드를 다른 테이블에 매핑할 수 있음|현재 클래스가 매핑된 테이블
>>>nullable(DDL)|null 값의 허용 여부를 설정함<br>false로 설정하면 DDL 생성 시에 not null 제약조건이 붙음|true
>>>unique(DDL)|@Table 의 uniqueConstraints 와 같지만 한 컬럼에 간단히 유니크 제약조건을 걸 때 사용함<br>만약 두 컬럼 이상을 사용해서 유니크 제약조건을 사용하려면 클래스 레벨에서 @Table.uniqueConstraints 를 사용해야 함
>>>columnDefinition(DDL)|DB 컬럼 정보를 직접 줄 수 있음|필드의 자바 타입과 방언 정보를 사용해서 적절한 컬럼 타입을 생성함
>>>length(DDL)|문자 길이 제약조건, String 타입에만 사용함|255
>>>precision, scale(DDL)|BigDecimal 타입에서 사용함(BigInteger 도 사용할 수 있음)<br>precision 은 소수점을 포함한 전체 자리수를, scale 은 소수의 자릿수임<br>참고로 double, float 타입에는 적용되지 않음<br>아주 큰 숫자나 정밀한 소수를 다루어야 할 때만 사용함|precision=19, scale=2
>- @Enumerated
>>- 자바의 enum 타입을 매핑할 때 사용함
>>>속성|기능|기본값
>>>:--|:--|:--
>>>value|EnumType.ORDINAL: enum 순서를 DB에 저장<br>EnumType.STRING : enum 이름을 DB에 저장| EnumType.ORDINAL
>>```java
>>// enum 클래스
>>enum RoleType {
>>  ADMIN, USER
>>}
>>
>>// enum 이름으로 매핑
>>@Enumerated(EnumType.STRING)
>>private RoleType roleType;
>>
>>// enum 사용법
>>member.setRoleType(RoleType.ADMIN); // -> DB에 문자 ADMIN 으로 저장됨
>>```
>>- EnumType.ORDINAL 은 enum 에 정의된 순서대로 ADMIN 은 0, USER 는 1 값이 DB에 저장됨
>>>- 장점 : DB에 저장되는 데이터 크기가 작음
>>>- 단점 : 이미 저장된 enum 의 순서를 변경할 수 없음
>>- EnumType.STRING 은 enum 이름 그대로 DB에 저장됨
>>>- 장점 : 저장된 enum 의 순서가 바뀌거나 enum 이 추가되어도 안전함
>>>- 단점 : DB 에 저장되는 데이터 크기가 ORDINAL 에 비해서 큼
>- @Temporal
>>- 날짜 타입(java.util.Date, java.util.Calendar) 을 매핑할 때 사용함
>>>속성|기능|기본값
>>>:--|:--|:--
>>>value|TemporalType.DATE : 날짜, 데이터베이스 date 타입과 매핑(예: 2013-10-11)<br>TemporalType.TIME : 시간, 데이터베이스 time 타입과 매핑(예: 11:11:11)<br>TemporalType.TIMESTAMP : 날짜와 시간, 데이터베이스 timestamp 타입과 매핑(예: 2013-10-11 11:11:11)|TemporalType 은 필수로 지정해야 함
>- @Lob
>>- DB 의 BLOB, CLOB 타입과 매핑함
>>- @Lob 에는 지정할 수 있는 속성이 없는 대신에 매핑하는 필드 타입이 문자면 CLOB 로 매핑하고 나머지는 BLOB 로 매핑함
>>```java
>>@Lob
>>private String lobString;
>>
>>@Lob
>>private byte[] lobByte;
>>```
>- @Transient
>>- 이 필드는 매핑하지 않도록 설정하여 DB에 저장하지도 않고 조회하지도 않음
>>- 객체에 임시로 어떤 값을 보관하고 싶을 때 사용함
>>```java
>>@Transient
>>private Integer temp;
>>```
>- @Access
>>- JPA 가 엔티티에 접근하는 방식을 지정함
>>>- 필드 접근 : AccessType.FIELD 로 지정하여 필드에 직접 접근함, 필드 접근 권한이 private 여도 접근할 수 있음
>>>- 프로퍼티 접근 : AccessType.PROPERTY 로 지정하여 접근자(Getter)를 사용함
```java
// @Id 가 필드에 있으므로 @Access(AccessType.FIELD) 로 설정한 것과 같으므로 @Access 는 생략 가능함
@Entity
@Access(AccessType.FIELD)
public class Member {
  @Id
  private String id;

  private String data1;
  private String data2;
  ...
}

// @Id 가 프로퍼티에 있으므로 @Access(AccessType.PROPERTY) 로 설정한 것과 같으므로 @Access 는 생략 가능함
@Entity
@Access(AccessType.PROPERTY)
public class Member {

  private String id;

  private String data1;
  private String data2;

  @Id
  public String getId() {
    return id;
  }

  @Column
  public String getData1() {
    return data1;
  }

  public String getData2() {
    return data2;
  }
}

// @Id 가 필드에 있으므로 기본은 필드 접근 방식을 사용하고, getFullName() 만 프로퍼티 접근 방식을 사용함
// 회원 엔티티를 저장하면 회원 테이블의 FULLNAME 컬럼에 firstName + lastName 의 결과가 저장됨
@Entity
public class Member {

  @id
  private String id;

  @Transient
  private String firstName;

  @Transient
  private String lastName;

  @Access(AccessType.PROPERTY)
  public String getFullName() {
    return firstName + lastName;
  }
  ...
}
```

<br>

[목차로 이동](#목차)

---

## 연관관계 매핑 기초

>### 연관관계 매핑 기초 개요
>- 엔티티들은 대부분 다른 엔티티와 연관관계가 있는데 객체는 참조(주소)를 사용해서 관계를 맺고 테이블은 외래 키를 사용해서 관계를 맺음
>- 객체의 참조와 테이블의 외래 키를 매핑하는 것이 이 장의 목표임
>- 연관관계 매핑을 이해하기 위한 핵심 키워드
>>- 방향(Direction)
>>>- 단방향, 양방향이 있음
>>>- 예를 들어 회원과 팀이 관계가 있을 때 회원 -> 팀 또는 팀 -> 회원 둘 중 한 쪽만 참조하는 것을 단방향 관계라 하고, 회원 -> 팀, 팀 -> 회원 양쪽 모두 서로 참조하는 것을 양방향 관계라 함
>>>- 방향은 객체관계에만 존재하고 테이블 관계는 항상 양방향임
>>- 다중성(Multiplicity)
>>>- 다대일, 일대다, 일대일, 다대다 다중성이 있음
>>>- 예를 들어 회원과 팀이 관계가 있을 때 여러 회원은 한 팀에 속하므로 회원과 팀은 다대일 관계임
>>>- 반대로 한 팀에 여러 회원이 소속될 수 있으므로 팀과 회원은 일대다 관계임
>>- 연관관계의 주인(Owner)
>>>- 객체를 양방향 연관관계로 만들면 연관관계의 주인을 정해야 함

<br>

[목차로 이동](#목차)

>### 단방향 연관관계
>- 개요
>>- 회원과 팀의 관계를 통해 다대일 단방향 관계를 알아봄
>>>- 회원과 팀이 있음
>>>- 회원은 하나의 팀에만 소속될 수 있음
>>>- 회원과 팀은 다대일 관계임
>>- 객체 연관관계
>>>- 회원 객체와 팀 객체는 단방향 관계임
>>>- 회원은 Member.team 필드를 통해서 팀을 알 수 있지만 반대로 팀은 회원을 알 수 없음
>>- 테이블 연관관계
>>>- 회원 테이블과 팀 테이블은 양방향 관계임
>>>- 회원 테이블의 TEAM_ID 외래 키를 통해서 회원과 팀을 조인할 수 있고 반대로 팀과 회원도 조인할 수 있음
>>- 객체 연관관계와 테이블 연관관계의 가장 큰 차이
>>>- 참조를 통한 연관관계는 언제나 단방향임
>>>- 객체간에 연관관계를 양방향으로 만들고 싶으면 반대쪽에도 필드를 추가해서 참조를 보관해야 하므로 결국 연관관계를 하나 더 만들어야 함
>>>- 양쪽에서 서로 참조하는 것을 양방향 연관관계라 하지만 정확히 이야기하면 이것은 양방향 관계가 아니라 서로 다른 단방향 관계 2개임
>>- 객체 연관관계 vs 테이블 연관관계 정리
>>>- 객체는 참조(주소)로 연관관계를 맺음
>>>- 테이블은 외래 키로 연관관계를 맺음
>>>- 참조를 사용하는 객체의 연관관계는 단방향임
>>>- 외래 키를 사용하는 테이블의 연관관계는 양방향임
>>>- 객체를 양방향으로 참조하려면 단방향 연관관계를 2개 만들어야 함
>- 순수한 객체 연관관계
>```java
>// JPA를 사용하지 않은 순수한 회원과 팀 클래스
>public class Member {
>  private String id;
>  private String username;
>
>  private Team team; // 팀의 참조를 보관
>
>  public void setTeam(Team team) {
>    this.team = team;
>  }
>
>  ...
>}
>
>public class Team {
>  private String id;
>  private String name;
>
>  ...
>}
>
>// 동작 코드
>public static void main(String[] args) {
>  Member member1 = new Member("member1", "회원1");
>  Member member2 = new Member("member2", "회원2");
>  Team team1 = new Team("team1", "팀1");
>
>  member1.setTeam(team1);
>  member2.setTeam(team2);
>
>  Team findTeam = member1.getTeam();
>}
>```
>>- 객체는 참조를 사용해서 연관관계를 탐색할 수 있는데 이것을 객체 그래프 탐색이라 함
>- 테이블 연관관계
>>- 데이터베이스는 외래 키를 사용해서 연관관계를 탐색할 수 있는데 이것을 조인이라 함
>- 객체 관계 매핑
>```java
>// 매핑한 회원 엔티티
>@Entity
>public class Member {
>  @Id
>  @Column(name = "MEMBER_ID")
>  private String id;
>
>  private String username;
>
>  // 연관관계 매핑
>  @ManyToOne
>  @JoinColumn(name="TEAM_ID")
>  private Team team;
>
>  // 연관관계 설정 
>  public void setTeam(Team team) {
>    this.team = team;
>  }
>  ...
>}
>
>// 매핑한 팀 엔티티
>@Entity
>public class Team {
>  @Id
>  @Column(name = "TEAM_ID")
>  private String id;
>
>  private String name;
>
>  ...
>}
>```
>>- @ManyToOne
>>>- 이름 그대로 다대일(N:1) 관계라는 매핑 정보임
>>>- 연관관계를 매핑할 때 이렇게 다중성을 나타내는 어노테이션을 필수로 사용해야 함
>>- @JoinColumn(name="TEAM_ID")
>>>- 조인 컬럼은 외래 키를 매핑할 때 사용함
>>>- name 속성에는 매핑할 외래 키 이름을 지정함
>- @JoinColumn
>>- 외래 키를 매핑할 때 사용함
>>
>>속성|기능|기본값
>>:--|:--|:--
>>name|매핑할 외래 키 이름|필드명+_+참조하는 테이블의 기본 키 컬럼명
>>referencedColumnName|외래 키가 참조하는 대상 테이블의 컬럼명|참조하는 테이블의 기본 키 컬럼명
>>foreignKey(DDL)|외래 키 제약조건을 직접 지정할 수 있음<br>이 속성은 테이블을 생성할 때만 사용함|
>>unique<br>nullable<br>insertable<br>updatable<br>columnDefinition<br>table|@Column 의 속성과 같음
>>- @JoinColumn 생략
>>>- 생략하면 외래 키를 찾을 때 기본 전략을 사용함
>>>- 기본 전략 : 필드명+_+참조하는 테이블의 컬럼명
>>>>- 예) 필드명(team)+_(밑줄)+참조하는 테이블의 컬럼명(TEAM_ID) = team_TEAM_ID 외래 키를 사용함
>- @ManyToOne
>>- 다대일 관계에서 사용함
>>
>>속성|기능|기본값
>>:--|:--|:--
>>optional|false 로 설정하면 연관된 엔티티가 항상 있어야 함|true
>>fetch|글로벌 페치 전략을 설정함|@ManyToOne=FetchType.EAGER<br>@OneToMany=FetchType.LAZY
>>cascade|영속성 전이 기능을 사용함|
>>targetEntity|연관된 엔티티의 타입 정보를 설정함<br>이 기능은 거의 사용하지 않음<br>컬렉션을 사용해도 제네릭으로 타입 정보를 알 수 있음|

<br>

[목차로 이동](#목차)

>### 연관관계 사용
>- 저장
>```java
>public void testSave() {
>  // 팀1 저장
>  Team team1 = new Team("team1", "팀1");
>  em.persist(team);
>
>  // 회원1 저장
>  Member member1 = new Member("member1", "회원1");
>  member1.setTeam(team1); // 연관관계 설정 member1 -> team1
>  em.persist(member1);
>
>  // 회원2 저장
>  Member member2 = new Member("member2", "회원2");
>  member2.setTeam(team1); // 연관관계 설정 member2 -> team1
>  em.persist(member2);
>}
>```
>- 조회
>>- 연관관계가 있는 엔티티를 조회하는 2가지 방법
>>>- 객체 그래프 탐색 (객체 연관관계를 사용한 조회)
>>>- 객체지향 쿼리 사용 (JPQL)
>>- 객체 그래프 탐색
>>>- member.getTeam() 을 사용해서 member 와 연관된 team 엔티티를 조회할 수 있음
>>>```java
>>>Member member = em.find(Member.class, "member1");
>>>Team team = member.getTeam(); // 객체 그래프 탐색
>>>System.out.println("팀 이름 = " + team.getName());
>>>// 출력 결과 : 팀 이름 = 팀1
>>>```
>>- 객체지향 쿼리 사용
>>>- 회원을 대상으로 조회하는데 팀1에 소속된 회원만 조회하려면 회원과 연관된 팀 엔티티를 검색 조건으로 사용해야함
>>>- SQL은 연관된 테이블을 조인해서 검색조건을 사용하면 됨
>>>```java
>>>private static void queryLogicJoin(EntityManager em) {
>>>  String jpql = "select m from Member m join m.team t where t.name=:teamName";
>>>
>>>  List<Member> resultList = em.createQuery(jpql, Member.class).setParameter("teamName", "팀1").getResultList();
>>>
>>>  for(Member member:resultList) {
>>>    System.out.println("[query] member.username=" + member.getUsername());
>>>  }
>>>}
>>>// 결과: [query] member.username=회원1
>>>// 결과: [query] member.username=회원2
>>>```
>>>- :teamName 과 같이 :로 시작하는 것은 파라미터를 바인딩받는 문법임
>- 수정
>```java
>private static void updateRelation(EntityManager em) {
>  // 새로운 팀2
>  Team team2 = new Team("team2", "팀2");
>  em.persist(team2);
>
>  // 회원1에 새로운 팀2 설정
>  Member member = em.find(Member.class, "member1");
>  member.setTeam(team2);
>}
>```
>>- 수정은 em.update() 같은 메서드가 없고 단순히 불러온 엔티티의 값만 변경해두면 트랜잭션을 커밋할 때 플러시가 일어나면서 변경 감지 기능이 작동하며 변경 사항을 DB에 자동으로 반영함
>>- 연관관계를 수정할 때도 마찬가지로 참조하는 대상만 변경하면 나머지는 JPA가 자동으로 처리함
>- 연관관계 제거
>```java
>private static void deleteRelation(EntityManager em) {
>  Member member1 = em.find(Member.class, "member1");
>  member1.setTeam(null); // 연관관계 제거
>}
>```
>>- 연관관계를 null로 설정하면 연관관계가 제거됨
>- 연관된 엔티티 삭제
>>- 연관된 엔티티를 삭제하려면 기존에 있던 연관관계를 먼저 제거하고 삭제해야 함, 그렇지 않으면 외래 키 제약조건으로 인해 DB에서 오류가 발생함

<br>

[목차로 이동](#목차)

>### 양방향 연관관계
>- 개요
>>- 회원에서 팀으로 접근하는 다대일 단방향 매핑에 더하여 반대 방향인 팀에서 회원으로 접근하는 관계를 추가하여 양방향 연관관계로 매핑할 수 있음
>>- 일대다 관계는 여러 건과 연관관계를 맺을 수 있으므로 컬렉션을 사용해야 함
>- 양방향 연관관계 매핑
>```java
>// 매핑한 회원 엔티티
>@Entity
>public class Member {
>  @Id
>  @Column(name="MEMBER_ID")
>  private String id;
>
>  private String username;
>
>  @ManyToOne
>  @JoinColumn(name="TEAM_ID")
>  private Team team;
>
>  // 연관관계 설정
>  public void setTeam(Team team) {
>    this.team = team;
>  }
>  
>  ...
>}
>
>// 매핑한 팀 엔티티
>@Entity
>public class Team {
>  @Id
>  @Column(name="TEAM_ID")
>  private String id;
>
>  private String name;
>
>  //==추가==//
>  @OneToMany(mappedBy="team")
>  private List<Member> members = new ArrayList<Member>();
>
>  ...
>}
>```
>>- 팀과 회원은 일대다 관계이므로 팀 엔티티에 컬렉션을 추가하고 일대다 관계를 매핑하기 위해 @OneToMany 매핑 정보를 사용함
>>- mappedBy 속성은 양방향 매핑일 때 사용하는데 반대쪽 매핑의 필드 이름을 값으로 주면 됨
>- 일대다 컬렉션 조회
>```java
>public void biDirection() {
>  Team team = em.find(Team.class, "team1");
>  List<Member> members - team.getMembers(); // (팀->회원) 객체 그래프 탐색
>
>  for(Member member : members) {
>    System.out.println("member.username="+member.getUsername());
>  }
>}
>// 결과
>// member.username = 회원1
>// member.username = 회원2
>```

<br>

[목차로 이동](#목차)

>### 연관관계의 주인
>- 개요
>>- 엄밀히 말하면 객체에는 양방향 연관관계라는 것이 없고 서로 다른 단방향 연관관계 2개를 애플리케이션 로직으로 잘 묶어서 양방향인 것철머 보이게 할 뿐임
>>- 엔티티를 양방향 연관관계로 설정하면 객체의 참조는 둘인데 외래 키는 하나이므로 둘 사이에 차이가 발생함
>>- 이런 차이로 인해 JPA에서는 두 객체 연관관계 중 하나를 정해서 테이블의 외래키를 관리해야 하는데 이것을 연관관계의 주인이라 함
>- 양방향 매핑의 규칙: 연관관계의 주인
>>- 연관관계의 주인만이 DB 연관관계와 매핑되고 외래 키를 관리(등록,수정,삭제)할 수 있는 반면에 주인이 아닌 쪽은 읽기만 할 수 있음
>>- 어떤 연관관계를 주인으로 정할지는 mappedBy 속성을 사용함
>>>- 주인은 mappedBy 속성을 사용하지 않음
>>>- 주인이 아니면 mappedBy 속성을 사용해서 속성의 값으로 연관관계의 주인을 지정해야 함
>>- 연관관계의 주인을 정한다는 것은 사실 외래 키 관리자를 선택하는 것임
>- 연관관계의 주인은 외래 키가 있는 곳
>>- 연관관계의 주인은 테이블에 외래 키가 있는 곳으로 정해야 함
>>- 회원 테이블이 외래 키를 가지고 있으면 Member.team 이 주인이 됨
>>- 주인이 아닌 Team.members 에는 mappedBy="team" 속성을 사용해서 주인이 아님을 설정하면서 mappedBy 속성의 값으로는 연관관계의 주인인 team을 주면 됨
>>- 여기서 mappedBy 의 값으로 사용됨 team 은 연관관계의 주인인 Member 엔티티의 team 필드를 말함
>>```java
>>class Team {
>>  @OneToMany(mappedBy="team")
>>  private List<Member> members = new ArrayList<Member>();
>>  ...
>>}
>>```
>>- DB 테이블의 다대일, 일대다 관계에서는 항상 다 쪽이 외래 키를 가지며 다 쪽인 @ManyToOne 은 항상 연관관계의 주인이 되므로 mappedBy 를 설정할 수 없으므로 @ManyToOne 에는 mappedBy 속성이 없음

<br>

[목차로 이동](#목차)

>### 양방향 연관관계 저장
>```java
>public void testSave() {
>  // 팀1 저장
>  Team team1 = new Team("team1", "팀1");
>  em.persist(team1);
>  // 회원1 저장
>  Member member1 = new Member("member1", "회원1");
>
>  member1.setTeam(team1); // 연관관계 설정 member1 -> team1
>  em.persist(member1);
>  // 회원2 저장
>  Member member2 = new Member("member2", "회원2");
>
>  member2.setTeam(team1); // 연관관계 설정 member2 -> team1
>  em.persist(member2);
>}
>```
>>- 양방향 연관관계는 연관관계의 주인이 외래 키를 관리하므로 주인이 아닌 방향은 값을 설정하지 않아도 DB에 외래 키 값이 정상 입력됨

<br>

[목차로 이동](#목차)

>### 양방향 연관관계의 주의점
>- 가장 흔한 실수
>>- 양방향 연관관계를 설정하고 가장 흔히 하는 실수는 연관관계의 주인에는 값을 입력하지 않고, 주인이 아닌 곳에만 값을 입력하는 것임
>>- DB에 외래 키 값이 정상적으로 저장되지 않으면 이것부터 의심해야함
>```java
>public void testSaveNonOwner() {
>  // 회원1 저장
>  Member member1 = new Member("member1", "회원1");
>  em.persist(member1);
>
>  // 회원2 저장
>  Member member2 = new Member("member2", "회원2");
>  em.persist(member2);
>
>  Team team1 = new Team("team1", "팀1");
>  // 주인이 아닌 곳에만 연관관계 설정
>  team1.getMembers().add(member1);
>  team1.getMembers().add(member2);
>
>  em.persist(team1);
>}
>```
>>- 회원1, 회원2 를 저장하고 팀의 컬렉션에 담은 후에 팀을 저장하면 DB에서 회원 테이블에는 TEAM_ID 에 아무런 값도 입력되지 않음
>- 순수한 객체까지 고려한 양방향 연관관계
>>- 사실은 객체 관점에서 양쪽 방향에 모두 값을 입력해주는 것이 가장 안전함
>>- 양쪽 방향 모두 값을 입력하지 않으면 JPA를 사용하지 않는 순수한 객체 상태에서 심각한 문제각 발생할 수 있음
>>- JPA를 사용하지 않고 엔티티에 대한 테스트 코드를 작성한다고 가정
>>```java
>>public void test순수한객체_양방향() {
>>  // 팀1
>>  Team team1 = new Team("team1", "팀1");
>>  Member member1 = new Member("member1", "회원1");
>>  Member member2 = new Member("member2", "회원2");
>>
>>  member1.setTeam(team1); // 연관관계 설정 member1 -> team1
>>  member2.setTeam(team1); // 연관관계 설정 member2 -> team1
>>
>>  List<Member> members = team1.getMembers();
>>  System.out.println("members.size = " + member.size());
>>}
>>// 결과: members.size = 0
>>```
>>>- ORM 은 객체와 관계형 DB 둘 다 중요하므로 DB 뿐만 아니라 객체도 함께 고려해야 함
>>- 양쪽 모두 관계를 설정한 전체 코드
>>```java
>>public void test순수한객체_양방향() {
>>  // 팀1
>>  Team team1 = new Team("team1", "팀1");
>>  Member member1 = new Member("member1", "회원1");
>>  Member member2 = new Member("member2", "회원2");
>>
>>  member1.setTeam(team1); // 연관관계 설정 member1 -> team1
>>  team1.getMembers().add(member1); // 연관관계 설정 team1 -> member1
>>  member2.setTeam(team1); // 연관관계 설정 member2 -> team1
>>  team1.getMembers().add(member2); // 연관관계 설정 team1 -> member2
>>
>>  List<Member> members = team1.getMembers();
>>  System.out.println("members.size = " + member.size());
>>}
>>// 결과: members.size = 2
>>```
>>- JPA 를 사용해서 완성한 코드
>>```java
>>public void testORM_양방향() {
>>  // 팀1 저장
>>  Team team1 = new Team("team1", "팀1");
>>  em.persist(team1);
>>
>>  Member member1 = new Member("member1", "회원1");
>>  
>>  // 양방향 연관관계 설정
>>  member1.setTeam(team1); // 연관관계 설정 member1 -> team1
>>  team1.getMembers().add(member1); // 연관관계 설정 team1 -> member1
>>  em.persist(member1);
>>
>>  Member member2 = new Member("member2", "회원2");
>>  
>>  // 양방향 연관관계 설정
>>  member2.setTeam(team1); // 연관관계 설정 member2 -> team1
>>  team1.getMembers().add(member2); // 연관관계 설정 team1 -> member2
>>  em.persist(member2);
>>}
>>```
>>>- 순수한 객체 상태에서도 동작하며, 테이블의 외래 키도 정상 입력됨
>>>- 외래 키의 값은 연관관계의 주인인 Member.team 값을 사용함
>- 연관관계 편의 메서드
>>- 양방향 연관관계는 결국 양쪽 다 신경 써야 하므로 각각 호출하다 보면 실수로 둘 중 하나만 호출해서 양방향이 깨질 수 있으므로 두 코드를 하나인 것처럼 사용하는 것이 안전함
>>```java
>>public class Member {
>>  private Team team;
>>
>>  public void setTeam(Team team) {
>>    this.team = team;
>>    team.getMembers().add(this);
>>  }
>>  ...
>>}
>>```
>- 연관관계 편의 메서드 작성 시 주의사항
>```java
>member1.setTeam(teamA);
>member1.setTeam(teamB);
>Member findMember = teamA.getMember(); // member1 이 여전히 조회됨
>```
>>- 연관관계를 변경할 때는 기존 팀이 있으면 기존 팀과 회원의 연관관계도 삭제하는 코드를 추가해야 함
>>```java
>>public class Member {
>>  private Team team;
>>
>>  public void setTeam(Team team) {
>>    // 기존 팀과 관계를 제거
>>    if(this.team != null) {
>>      this.team.getMembers().remove(this);
>>    }
>>
>>    this.team = team;
>>    team.getMembers().add(this);
>>  }
>>  ...
>>}
>>```
>>>- 객체에서 서로 다른 단방향 연관관계 2개를 양방향인 것처럼 보이게 하려면 얼마나 많은 고민과 수고가 필요한지 확인할 수 있음

<br>

[목차로 이동](#목차)

---

## 다양한 연관관계 매핑

>### 다양한 연관관계 매핑 개요
>- 엔티티의 연관관계 매핑시 3 가지 고려사항
>>- 다중성
>>- 단방향, 양방향
>>- 연관관계의 주인
>- 다중성
>>- 다대일 (@ManyToOne)
>>- 일대다 (@OneTpMany)
>>- 일대일 (@OneToOne)
>>- 다대다 (@ManyToMany)
>- 단방향, 양방향
>>- 테이블은 외래 키 하나로 조인을 사용해서 양방향으로 쿼리가 가능하므로 사실상 방향이라는 개념이 없음
>>- 객체는 참조용 필드를 가지고 있는 객체만 연관된 객체를 조회할 수 있음
>>- 객체 관계에서 한 쪽만 참조하는 것을 단방향 관계라 하고, 서로 참조하는 것을 양방향 관계라 함
>- 연관관계의 주인
>>- DB는 외래 키 하나로 두 테이블이 연관관계를 맺으므로 테이블의 연관관계를 관리하는 포인트는 외래 키 하나임
>>- 반면에 엔티티를 양방향으로 매핑하면 A->B, B->A 2곳에서 서로를 참조하므로 객체의 연관관계를 관리하는 포인트는 2곳임
>>- JPA는 두 객체 연관관계 중 하나를 정해서 DB 외래 키를 관리하는데 이것을 연관관계의 주인이라 함
>>- 외래 키를 가진 테이블과 매핑한 엔티티가 외래 키를 관리하는 것이 효율적이므로 보통 이곳을 연관관계의 주인으로 선택함
>>- 주인이 아닌 방향은 외래 키를 변경할 수 없고 읽기만 가능함
>>- 연관관계의 주인은 mappedBy 속성을 사용하지 않으며, 연관관계의 주인이 아니면 mappedBy 속성을 사용하고 연관관계의 주인 필드 이름을 값으로 입력해야 함

<br>

[목차로 이동](#목차)

>### 다대일
>- 개요
>>- 다대일 관계의 반대 방향은 항상 일대다 관계고 일대다 관계의 반대 방향은 항상 다대일 관계임
>>- DB 테이블의 일, 다 관계에서 외래 키는 항상 다쪽에 있으므로 객체 양방향 관계에서 연관관계의 주인은 항상 다쪽임
>- 다대일 단방향[N:1]
>```java
>// 회원 엔티티
>@Entity
>public class Member {
>  @Id @GeneratedValue
>  @Column(name - "MEMBER_ID")
>  private Long id;
>
>  private String username;
>
>  @ManyToOne
>  @JoinColumn(name = "TEAM_ID")
>  private Team team;
>  ...
>}
>
>// 팀 엔티티
>@Entity
>public class Team {
>  @Id @GeneratedValue
>  @Column(name = "TEAM_ID")
>  private Long id;
>
>  private String name;
>  ...
>}
>```
>>- 회원은 Member.team 으로 팀 엔티티를 참조할 수 있지만 반대로 팀에는 회원을 참조하는 필드가 없으므로 회원과 팀은 다대일 단방향 연관관계임
>>- @JoinColumn(name = "TEAM_ID") 을 사용하여 Member.team 필드를 TEAM.ID 외래 키와 매핑하여 Member.team 필드로 회원 테이블의 TEAM_ID 외래 키를 관리함
>- 다대일 양방향[N:1, 1:N]
>```java
>// 회원 엔티티
>@Entity
>public class Member {
>  @Id @GeneratedValue
>  @Column(name - "MEMBER_ID")
>  private Long id;
>
>  private String username;
>
>  @ManyToOne
>  @JoinColumn(name = "TEAM_ID")
>  private Team team;
>
>  public void setTeam(Team team) {
>    this.team = team;
>
>    // 무한루프에 빠지지 않도록 체크
>    if(!team.getMembers().contains(this)) {
>      team.getMembers().add(this);
>    }
>  }
>}
>
>// 팀 엔티티
>@Entity
>public class Team {
>  @Id @GeneratedValue
>  @Column(name = "TEAM_ID")
>  private Long id;
>
>  private String name;
>  
>  @OneToMany(mappedBy = "team")
>  private List<Member> members = new ArrayList<Member>();
>
>  public void addMember(Member member) {
>    this.members.add(member);
>    if(member.getTeam() != this) { // 무한루프에 빠지지 않도록 체크
>      member.setTeam(this);
>    }
>  }
>}
>```
>>- 양방향은 외래 키가 있는 쪽이 연관관계의 주인임
>>>- JPA는 외래 키를 관리할 때 연관관계의 주인만 사용함
>>>- 주인이 아닌 Team.members 는 조회를 위한 JPQL 이나 객체 그래프를 탐색할 때 사용함
>>- 양방향 연관관계는 항상 서로를 참조해야 함
>>>- 항상 서로를 참조하게 하려면 연관관계 편의 메서드를 작성하는 것이 좋음
>>>- 편의 메서드는 한 곳에만 작성하거나 양쪽 다 작성할 수 있는데, 양쪽에 다 작성하면 무한루프에 빠지므로 주의해야 함

<br>

[목차로 이동](#목차)

>### 일대일
>- 개요
>>- 일대일 관계는 양쪽이 서로 하나의 관계만 가짐
>>- 일대일 관계의 특징
>>>- 일대일 관계는 그반 대도 일대일 관계임
>>>- 일대일 관계는 주 테이블이나 대상 테이블 둘 중 어느곳이나 외래 키를 가질 수 있음
>>- 일대일 관계는 주 테이블이나 대상 테이블 중에 누가 외래 키를 가질지 선택해야 함
>- 주 테이블에 외래 키
>>- 일대일 관계를 구성할 때 객체지향 개발자들은 주 테이블에 외래 키가 있는 것을 선호함
>>- JPA 도 주 테이블에 외래 키가 있으면 좀 더 편리하게 매핑할 수 있음
>>- 단방향
>>```java
>>@Entity
>>public class Member {
>>  @Id @GeneratedValue
>>  @Column(name = "MEMBER_ID")
>>  private Long id;
>>
>>  private String username;
>>
>>  @OneToOne
>>  @JoinColumn(name = "LOCKER_ID")
>>  private Locker locker;
>>  ...
>>}
>>
>>@Entity
>>public class Locker {
>>  @Id @GeneratedValue
>>  @Column(name = "LOCKER_ID")
>>  private Long id;
>>
>>  private String name;
>>  ,,,
>>}
>>```
>>>- 일대일 관계 이므로 객체 매핑에 @OneToOne 을 사용했고 DB에는 LOCKER_ID 외래 키에 유니크 제약 조건을 추가함
>>- 양방향
>>```java
>>@Entity
>>public class Member {
>>  @Id @GeneratedValue
>>  @Column(name = "MEMBER_ID")
>>  private Long id;
>>
>>  private String username;
>>
>>  @OneToOne
>>  @JoinColumn(name = "LOCKER_ID")
>>  private Locker locker;
>>  ...
>>}
>>
>>@Entity
>>public class Locker {
>>  @Id @GeneratedValue
>>  @Column(name = "LOCKER_ID")
>>  private Long id;
>>
>>  private String name;
>>
>>  @OneToOne(mappedBy = "locker")
>>  private Member member;
>>  ...
>>}
>>```
>>>- 양방향이므로 연관관계의 주인을 정해야함
>>>- MEMBER 테이블이 외래 키를 가지고 있으므로 Member 엔티티에 있는 Member.locker 가 연관관계의 주인임
>>>- 따라서 반대 매핑인 사물함의 Locker.member 는 mappedBy 를 선언해서 연관관계의 주인이 아니라고 설정함
>- 대상 테이블에 외래 키
>>- 전통적인 DB 개발자들은 대상 테이블에 외래 키를 두는 것을 선호함
>>- 테이블 관계를 일대일에서 일대다로 변경할 때 테이블 구조를 그대로 유지할 수 있다는 장점이 있음
>>- 단방향
>>>- 일대일 관계 중 대상 테이블에 외래 키가 있는 단방향 관계는 JPA에서 지원하지 않으며 매핑할 수 있는 방법도 없음
>>>- 단방향 관계를 Locker 에서 Member 방향으로 수정하거나, 양방향 관계를 만들고 Locker 를 연관관계의 주인으로 설정해야 함
>>- 양방향
>>```java
>>@Entity
>>public class Member {
>>  @Id @GeneratedValue
>>  @Column(name = "MEMBER_ID")
>>  private Long id;
>>
>>  private String username;
>>
>>  @OneToOne(mappedBy = "member")
>>  private Locker locker;
>>  ...
>>}
>>
>>@Entity
>>public class Locker {
>>  @Id @GeneratedValue
>>  @Column(name = "LOCKER_ID")
>>  private Long id;
>>
>>  private String name;
>>
>>  @OneToOne
>>  @JoinColumn(name - "MEMBER_ID")
>>  private Member member;
>>  ...
>>}
>>```
>>>- 일대일 매핑에서 대상 테이블에 외래 키를 두고 싶으면 이렇게 양방향으로 매핑하고 주 엔티티인 Member 엔티티 대신에 대상 엔티티인 Locker 를 연관관계의 주인으로 만들어서 LOCKER 테이블의 외래 키를 관리하도록 함

<br>

[목차로 이동](#목차)

>### 다대다 [N:N]
>- 개요
>>- 관계형 DB는 정규화된 테이블 2개로 다대다 관계를 표현할 수 없으므로 보통 다대다 관계를 일대다, 다대일 관계로 풀어내는 연결 테이블을 사용함
>>- 객체는 테이블과 다르게 객체 2개로 다대다 관계를 만들 수 있으므로 @ManyToMany 를 사용하면 다대다 관계를 편리하게 매핑할 수 있음
>- 다대다: 단방향
>```java
>@Entity
>public class Member {
>  @Id @Column(name = "MEMBER_ID")
>  private String id;
>
>  private String username;
>
>  @ManyToMany
>  @JoinTable(name = "MEMBER_PRODUCT", joinColumns = @JoinColumn(name = "MEMBER_ID"), inverseJoinColumns = @JoinColumn(name = "PRODUCT_ID"))
>  private List<Product> products = new ArrayList<Product>();
>
>  @Entity
>  public class Product {
>    @Id @Column(name = "PRODUCT_ID")
>    private String id;
>
>    private String name;
>    ...
>  }
>}
>```
>>- 회원 엔티티와 상품 엔티티를 @ManyToMany 로 매핑함
>>- @ManyToMany 와 @JoinTable 을 사용하여 연결 테이블을 바로 매핑했기 때문에 회원과 상품을 연결하는 연결 테이블 엔티티 없이 매핑을 완료할 수 있음
>>- @JoinTable 속성
>>>- @JoinTable.name : 연결 테이블을 지정함
>>>- @JoinTable.joinColumns : 현재 방향인 회원과 매핑할 조인 컬럼 정보를 지정함
>>>- @JoinTable.inverseJoinColumns : 반대 방향인 상품과 매핑할 조인 컬럼 정보를 지정함
>>- MEMBER_PRODUCT 테이블은 다대다 관계를 일대다, 다대일 관계로 풀어내기 위해 필요한 연결 테이블일 뿐이므로 @ManyToMany 로 매핑했다면 다대다 관계를 사용할때 연결 테이블을 신경 쓰지 않아도 됨
>>- 저장
>>```java
>>public void save() {
>>  Product productA = new Product();
>>  productA.setId("productA");
>>  productA.setName("상품A");
>>  em.persist(productA);
>>
>>  Member member1 = new Member();
>>  member1.setId("member1");
>>  member1.setUsername("회원1");
>>  member1.getProducts().add(productA); // 연관관계 설정
>>  em.persist(member1);
>>}
>>```
>>- 탐색
>>```java
>>public void find() {
>>  Member member = em.find(Member.class, "member1");
>>  List<Product> products = member.getProducts(); // 객체그래프탐색
>>  for(Product product : products) {
>>    System.out.println("product.name = " + product.getName());
>>  }
>>}
>>```
>- 다대다: 양방향
>```java
>@Entity
>public class Product {
>  @Id
>  private String id;
>
>  @ManyToMany(mappedBy = "products") // 역방향 추가
>  private List<Member> members;
>  ...
>}
>```
>>- 다대다 매핑이므로 역방향도 @ManyToMany 를 사용하고 연관관계의 주인이 아닌 곳에 mappedBy 를 설정함
>>- 다대다 양방향 연관관계 설정
>>```java
>>member.getProducts().add(product);
>>product.getMembers().add(member);
>>```
>>- 양방향 연관관계는 연관관계 편의 메서드를 추가해서 관리하는 것이 편리함
>>```java
>>public void addProduct(Product product) {
>>  ...
>>  products.add(product);
>>  product.getMembers().add(this);
>>}
>>```
>>>- 연관관계 편의 메서드를 추가하면 member.addProduct(product) 처럼 간단히 양방향 연관관계를 설정할 수 있음
>>- 역방향 탐색
>>```java
>>public void findInverse() {
>>  Product product = em.find(Product.class, "productA");
>>  List<Member> members = product.getMembers();
>>  for(Member member : members) {
>>    System.out.println("member = " + member.getUsername());
>>  }
>>}
>>```
>>>- 양방향 연관관계 덕분에 역방향으로 객체 그래프를 탐색할 수 있음
>- 다대다: 매핑의 한계와 극복, 연결 엔티티 사용
>>- 개요
>>>- @ManyToMany 를 사용하면 연결 테이블을 자동으로 처리해주므로 도메인 모델이 단순해지고 여러 가지로 편리해지지만 실무에서 사용하기에는 한계가 있음
>>>- 연결 테이블에 단순히 주문한 회원 아이디와 상품 아이디만 있는 것이 아니라 주문 수량 칼럼이나 주문한 날짜 같은 추가적인 컬럼이 더 필요한데 그러면 주문 엔티티나 상품 엔티티에는 추가한 컬럼들을 매핑할 수 없기 때문에 @ManyToMany 를 사용할 수 없음
>>>- 결국 연결 테이블을 매핑하는 연결 엔티티를 만들고 이곳에 추가할 컬럼들을 매핑하고 엔티티 간의 관계도 테이블 관계처럼 다대다에서 일대다, 다대일 관계로 풀어야함
>>- 엔티티 코드
>>```java
>>@Entity
>>public class Member {
>>  @Id @Column(name = "MEMBER_ID")
>>  private String id;
>>
>>  // 역방향
>>  @OneToMany(mappedBy = "member")
>>  private List<MemberProduct> memberProducts;
>>  ...
>>}
>>
>>@Entity
>>public class Product {
>>  @Id @Column(name = "PRODUCT_ID")
>>  private String id;
>>
>>  private String name;
>>  ...
>>}
>>
>>@Entity
>>@IdClass(MemberProductId.class)
>>public class MemberProduct {
>>  @Id
>>  @ManyToOne
>>  @JoinColumn(name = "MEMBER_ID")
>>  private Member member; // MemberProductId.member 와 연결
>>
>>  @Id
>>  @ManyToOne
>>  @JoinColumn(name = "PRODUCT_ID")
>>  private Product product; // MemberProductId.product 와 연결
>>
>>  private int orderAmount;
>>  ...
>>}
>>
>>public class MemberProductId implements Serializable {
>>  private String member; // MemberProduct.member와 연결
>>  private String product; // MemberProduct.product와 연결
>>
>>  @Override
>>  public boolean equals(Object o) {...}
>>
>>  @Override
>>  public int hashCode() {...}
>>}
>>```
>>>- @IdClass 를 사용해서 복합 기본 키를 매핑함
>>- 복합 기본키
>>>- 회원상품 엔티티는 기본 키가 MEMBER_ID 와 PRODUCT_ID 로 이루어진 복합 기본키(간단히 복합 키)임
>>>- JPA 에서 복합 키를 사용하려면 별도의 식별자 클래스를 만들어야 하며 엔티티에 @IdClass 를 사용해서 식별자 클래스를 지정하면 됨
>>- 복합 키를 위한 식별자 클래스의 특징
>>>- 복합 키는 별도의 식별자 클래스로 만들어야 함
>>>- Serializable 을 구현해야 함
>>>- equals 와 hashCode 메서드를 구현해야 함
>>>- 기본 생성자가 있어야 함
>>>- 식별자 클래스는 public 이어야 함
>>>- @IdClass 를 사용하는 방법 외에 @EmbeddedId 를 사용하는 방법도 있음
>>- 식별 관계
>>>- 회원상품은 회원과 상품의 기본 키를 받아서 자신의 기본 키로 사용함
>>>- 부모 테이블의 기본 키를 받아서 자신의 기본 키 + 외래 키로 사용하는 것을 DB 용어로 식별 관계라 함
>>- 저장하는 코드
>>```java
>>public void save() {
>>  // 회원 저장
>>  Member member1 = new Member();
>>  member1.setId("member1");
>>  member1.setUsername("회원1");
>>  em.persist(member1);
>>
>>  // 상품 저장
>>  Product productA = new Product();
>>  productA.setId("ProductA");
>>  productA.setName("상품1");
>>  em.persist(productA);
>>
>>  // 회원상품 저장
>>  MemberProduct memberProduct = new MemberProduct();
>>  memberProduct.setMember(member1);   // 주문 회원 - 연관관계 설정
>>  memberProduct.setProduct(productA); // 주문 상품 - 연관관계 설정
>>  memberProduct.setOrderAmount(2);    // 주문 수량
>>  em.persist(memberProduct);
>>}
>>```
>>>- 회원상품 엔티티를 만들면서 연관된 회원 엔티티와 상품 엔티티를 설정함
>>>- 회원상품 엔티티는 DB에 저장될 때 연관된 회원의 식별자와 상품의 식별자를 가져와서 자신의 기본 키 값으로 사용함
>>- 조회하는 코드
>>```java
>>public void find() {
>>  // 기본 키 값 생성
>>  MemberProductId memberProductId = new MemberProductId();
>>  memberProductId.setMember("member1");
>>  memberProductId.setProduct("productA");
>>
>>  MemberProduct memberProduct = em.find(MemberProduct.class, memberProductId);
>>
>>  Member member = memberProduct.getMember();
>>  Product product = memberProduct.getProduct();
>>
>>  System.out.println("member = " + member.getUsername());
>>  System.out.println("product = " + product.getName());
>>  System.out.println("orderAmount = " + memberProduct.getOrderAmount());
>>}
>>```
>>>- 지금까지는 기본 키가 단순해서 기본 키를 위한 객체르 사용하는 일이 없었지만 복합 키는 항상 식별자 클래스를 만들어야 함
>>>- 복합 키를 사용하는 것은 단순히 컬럼 하나만 기본 키로 사용하는 것과 비교해서 ORM 매핑에서 처리할 일이 상당히 많아짐
>>>- 복합 키를 위한 식별자 클래스도 만들어야 하고 @IdClass 또는 @EmbeddedId 도 사용해야 하며 식별자 클래스에 equals, hashCode 도 구현해야 함
>- 다대다: 새로운 기본 키 사용
>>- 개요
>>>- 추천하는 기본 키 생성 전략은 DB에서 자동으로 생성해주는 대리 키를 Long 값으로 사용하는 것인데 간편하고 거의 영구히 쓸 수 있으며 비즈니스에 의존하지 않으며 ORM 매핑 시에 복합 키를 만들지 않아도 되므로 간단히 매핑을 완성할 수 있다는 장점이 있음
>>- 연결 테이블에 새로운 기본 키를 사용한 코드
>>```java
>>@Entity
>>public class Order {
>>  @Id @GeneratedValue
>>  @Column(name = "ORDER_ID")
>>  private Long Id;
>>
>>  @ManyToOne
>>  @JoinColumn(name = "MEMBER_ID")
>>  private Member member;
>>
>>  @ManyToOne
>>  @JoinColumn(name = "PRODUCT_ID")
>>  private Product product;
>>
>>  private int orderAmount;
>>  ...
>>}
>>```
>>>- 회원상품(MemberProduct) 보다는 주문(Order, ORDER 는 일부 DB에 예약어로 잡혀 있어 ORDERS를 사용하기도 함)이라는 이름이 더 어울리므로 변경함
>>>- 대리 키를 사용함으로써 이전에 보았던 식별 관계에 복합 키를 사용하는 것보다 매핑이 단순하고 이해하기 쉬움
>>- 저장하고 조회하는 코드
>>```java
>>public void save() {
>>  // 회원 저장
>>  Member member1 = new Member();
>>  member1.setId("member1");
>>  member1.setUsername("회원1");
>>  em.persist(member1);
>>
>>  // 상품 저장
>>  Product productA = new Product();
>>  productA.setId("ProductA");
>>  productA.setName("상품1");
>>  em.persist(productA);
>>
>>  // 회원상품 저장
>>  Order order = new Order();
>>  order.setMember(member1);   // 주문 회원 - 연관관계 설정
>>  order.setProduct(productA); // 주문 상품 - 연관관계 설정
>>  order.setOrderAmount(2);    // 주문 수량
>>  em.persist(order);
>>}
>>
>>public void find() {
>>  Long orderId = 1L;
>>  Order order = em.find(Order.class, orderId);
>>
>>  Member member = order.getMember();
>>  Product product = order.getProduct();
>>
>>  System.out.println("member = " + member.getUsername());
>>  System.out.println("product = " + product.getName());
>>  System.out.println("orderAmount = " + order.getOrderAmount());
>>}
>>```
>>>- 식별자 클래스를 사용하지 않아서 조회 코드가 단순해 졌음

<br>

[목차로 이동](#목차)

---

## 고급 매핑

>### 상속 관계 매핑
>- 