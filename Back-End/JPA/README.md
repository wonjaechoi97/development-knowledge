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
>- [고급 매핑](#고급-매핑)
>>- [고급 매핑 개요](#고급-매핑-개요)
>>- [상속 관계 매핑](#상속-관계-매핑)
>>- [@MappedSuperclass](#mappedsuperclass)
>>- [복합 키와 식별 관게 매핑](#복합-키와-식별-관게-매핑)
>>- [조인 테이블](#조인-테이블)
>>- [엔티티 하나에 여러 테이블 매핑](#엔티티-하나에-여러-테이블-매핑)
>- [프록시와 연관관계 정리](#프록시와-연관관계-정리)
>>- [프록시와 연관관계 정리 개요](#프록시와-연관관계-정리-개요)
>>- [프록시](#프록시)
>>- [즉시 로딩과 지연 로딩](#즉시-로딩과-지연-로딩)
>>- [지연 로딩 활용](#지연-로딩-활용)
>>- [영속성 전이: CASCADE](#영속성-전이-cascade)
>>- [고아 객체](#고아-객체)
>>- [영속성 전이 + 고아 객체, 생명주기](#영속성-전이--고아-객체-생명주기)
>- [값 타입](#값-타입)
>>- [값 타입 개요](#값-타입-개요)
>>- [임베디드 타입(복합 값 타입)](#임베디드-타입복합-값-타입)
>>- [값 타입과 불변 객체](#값-타입과-불변-객체)
>>- [값 타입 컬렉션](#값-타입-컬렉션)
>>- [값 타입 정리](#값-타입-정리)

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

>### 고급 매핑 개요
>- 상속 관계 매핑
>>- 객체의 상속 관계를 DB에 어떻게 매핑하는지 다룸
>- @MappedSuperclass
>>- 등록일, 수정일 같이 여러 엔티티에서 공통으로 사용하는 매핑 정보만 상속받고 싶을때 사용
>- 복합 키와 식별 관계 매핑
>>- DB의 식별자가 하나 이상일 때 매핑하는 방법을 다룸
>>- DB 설계에서 이야기하는 식별 비식별 관계에 대해서 다룸
>- 조인 테이블
>>- 테이블은 외래 키 하나로 연관관계를 맺을 수 있지만 연관관계를 관리하는 연결 테이블을 두는 방법도 있는데, 연결 테이블을 매핑하는 방법을 다룸
>- 엔티티 하나에 여러 테이블 매핑하기
>>- 보통 엔티티 하나에 테이블 하나를 매핑하지만 엔티티 하나에 여러 테이블을 매핑하는 방법도 있음

<br>

[목차로 이동](#목차)

>### 상속 관계 매핑
>- 개요
>>- RDB 에는 객체지향 언어에서 다루는 상속이라는 개념이 없는 대신 슈퍼타입 서브타입 관계 모델링 기법이 객체의 상속 개념과 가장 유사함
>>- ORM 에서 이야기하는 상속 관계 매핑은 객체의 상속 구조와 DB의 슈퍼타입 서브타입 관계를 매핑하는 것임
>>- 슈퍼타입 서브타입 논리 모델을 실제 물리 모델인 테이블로 구현할 때 선택할 수 있는 3가지 방법
>>>- 각각의 테이블로 변환
>>>>- 각각을 모두 테이블로 만들고 조회할 때 조인을 사용함, JPA 에서 조인 전략이라 함
>>>- 통합 테이블로 변환
>>>>- 테이블을 하나만 사용해서 통합함, JPA 에서 단일 테이블 전략이라 함
>>>- 서브타입 테이블로 변환
>>>>- 서브 타입마다 하나의 테이블을 만듬, JPA 에서 구현 클래스마다 테이블 전략이라 함
>- 조인 전략
>>- 조인 전략은 엔티티 각각을 모두 테이블로 만들고 자식 테이블이 부모 테이블의 기본 키를 받아서 기본 키 + 외래 키로 사용하는 전략이므로 조회시 조인을 자주 사용함
>>- 주의할 점으로 객체는 타입을 구분할 수 있지만 테이블은 타입의 개념이 없으므로 타입을 구분하는 컬럼을 추가해야함
>>```java
>>@Entity
>>@Inheritance(strategy = InheritanceType.JOINED)
>>@DiscriminatorColumn(name = "DTYPE")
>>public abstract class Item {
>>  @Id @GeneratedValue
>>  @Column(name = "ITEM_ID")
>>  private Long id;
>>
>>  private String name;
>>  private int price;
>>  ...
>>}
>>
>>@Entity
>>@DiscriminatorValue("A")
>>public class Album extends Item {
>>  private String artist;
>>  ...
>>}
>>
>>@Entity
>>@DiscriminatorValue("M")
>>public class Movie extends Item {
>>  private String director;
>>  private String actor;
>>  ...
>>}
>>```
>>>- @Inheritance(strategy = InheritanceType.JOINED)
>>>>- 상속 매핑은 부모 클래스에 @Inheritance 를 사용하면서 속성으로 매핑 전략을 지정해야 함
>>>>- 조인 전략은 InheritanceType.JOINED 을 사용함
>>>- @DiscriminatorColumn(name = "DTYPE")
>>>>- 부모 클래스에 구분 칼럼을 지정하며 해당 칼럼으로 저장된 자식 테이블을 구분할 수 있음
>>>>- 기본값이 DTYPE 이므로 @DiscriminatorColumn 으로 줄여도 됨
>>>- @DiscriminatorValue("M")
>>>>- 엔티티를 저장할 때 구분 칼럼에 입력할 값을 지정함
>>- 기본값으로 자식 테이블은 부모 테이블의 ID 컬럼명을 그대로 사용하는데, 만약 자식 테이블의 기본 키 컬럼명을 변경하고 싶으면 @PrimaryKeyJoinColumn 을 사용하면 됨
>>```java
>>@Entity
>>@DiscriminatorValue("B")
>>@PrimaryKeyJoinColumn(name = "BOOK_ID") // ID 재정의
>>public class Book extends Item {
>>  private String author;
>>  private String isbn;
>>}
>>```
>>- 조인 전략 정리
>>>- 장점
>>>>- 테이블이 정규화됨
>>>>- 외래 키 참조 무결성 제약조건을 활용할 수 있음
>>>>- 저장공간을 효율적으로 사용함
>>>- 단점
>>>>- 조회할 때 조인이 많이 사용되므로 성능이 저하될 수 있음
>>>>- 조회 쿼리가 복잡함
>>>>- 데이터를 등록한 INSERT SQL 을 두 번 실행함
>>>- 특징
>>>>- JPA 표준 명세는 구분 컬럼을 사용하도록 하지만 하이버네이트를 포함한 몇몇 구현체는 구분 컬럼(@DiscriminatorColumn) 없이도 동작함
>>>- 관련 어노테이션
>>>>- @PrimaryKeyJoinColumn
>>>>- @DiscriminatorColumn
>>>>- @DiscriminatorValue
>- 단일 테이블 전략
>>- 이름 그대로 테이블을 하나만 사용하고 구분 컬럼(DTYPE) 으로 어떤 자식 데이터가 저장되었는지 구분함
>>- 조회할 때 조인을 사용하지 않으므로 일반적으로 가장 빠름
>>- 주의할 점으로 자식 엔티티가 매핑한 컬럼은 모두 null을 허용해야 한다는 점임
>>>- Book 엔티티를 저장하면 ITEM 테이블의 AUTHOR, ISBN 컬럼만 사용하고 다른 엔티티와 매핑된 ARTIST, DIRECTOR, ACTOR 칼럼은 사용하지 않으므로 null 이 입력되기 때문임
>>```java
>>@Entity
>>@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
>>@DiscriminatorColumn(name = "DTYPE")
>>public abstract class Item {
>>  @Id @GeneratedValue
>>  @Column(name = "ITEM_ID")
>>  private Long id;
>>
>>  private String name;
>>  private String price;
>>  ...
>>}
>>
>>@Entity
>>@DiscriminatorValue("A")
>>public class Album extends Item {...}
>>
>>@Entity
>>@DiscriminatorValue("M")
>>public class Movie extends Item {...}
>>
>>@Entity
>>@DiscriminatorValue("B")
>>public class Book extends Item {...}
>>```
>>>- InheritanceType.SINGLE_TABLE 로 지정하면 단일 테이블 전략을 사용함
>>>- 테이블 하나에 모든 것을 통합하므로 구분 컬럼을 필수로 사용해야함
>>- 단일 테이블 전략 정리
>>>- 장점
>>>>- 조인이 필요 없으므로 일반적으로 조회 성능이 빠름
>>>>- 조회 쿼리가 단순함
>>>- 단점
>>>>- 자식 엔티티가 매핑한 컬럼은 모두 null 을 허용해야 함
>>>>- 단일 테이블에 모든 것을 저장하므로 테이블이 커질 수 있고, 그로인해 상황에 따라서 조회 성능이 오히려 느려질 수 있음
>>>- 특징
>>>>- 구분 컬럼을 꼭 사용해야 하므로 @DiscriminatorColumn 을 꼭 설정해야 함
>>>>- @DiscriminatorValue 를 지정하지 않으면 기본으로 엔티티 이름을 사용함
>- 구현 클래스마다 테이블 전략
>>- 자식 엔티티마다 테이블을 만들며, 자식 테이블 각각에 필요한 컬럼이 모두 있음
>>```java
>>@Entity
>>@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
>>public abstract class Item {
>>  @Id @GeneratedValue
>>  @Column(name = "ITEM_ID")
>>  private Long id;
>>
>>  private String name;
>>  private int price;
>>}
>>
>>@Entity
>>public class Album extends Item {...}
>>
>>@Entity
>>public class Album extends Item {...}
>>
>>@Entity
>>public class Album extends Item {...}
>>```
>>>- InheritanceType.TABLE_PER_CLASS 를 선택하면 구현 클래스마다 테이블 전략을 사용함
>>>- 이 전략은 자식 엔티티마다 테이블을 만들며, 일반적으로 추천하지 않는 전략임
>>- 구현 클래스 마다 테이블 전략
>>>- 장점
>>>>- 서브 타입을 구분해서 처리할 때 효과적임
>>>>- not null 제약조건을 사용할 수 있음
>>>- 단점
>>>>- 여러 자식 테이블을 함께 조회할 때 성능이 느림(SQL 에 UNION 을 사용해야 함)
>>>>- 자식 테이블을 통합해서 쿼리하기 어려움
>>>- 특징
>>>>- 구분 컬럼을 사용하지 않음
>>- 이 전략은 DB 설계자와 ORM 전문가 둘 다 추천하지 전략이므로 조인이나 단일 테이블 전략을 고려하는것을 추천함

<br>

[목차로 이동](#목차)

>### @MappedSuperclass
>- 지금까지 학습한 상속 관계 매핑은 부모 클래스와 자식 클래스를 모두 DB 테이블과 매핑했음
>- 부모 클래스는 테이블과 매핑하지 않고 부모 클래스를 상속받는 자식 클래스에게 매핑 정보만 제공하고 싶으면 @MappedSuperclass 를 사용하면 됨
>- @MappedSuperclass 는 비유를 하자면 추상 클래스와 비슷한데 @Entity 는 실제 테이블과 매핑 되지만 @MappedSuperclass 는 실제 테이블과 매핑되지 않고 단순히 매핑 정보를 상속할 목적으로만 사용됨
>```java
>@MappedSuperclass
>public abstract class BaseEntity {
>  @Id @GeneratedValue
>  private Long id;
>  private String name;
>}
>
>@Entity
>public class Member extends BaseEntity {
>  // ID 상속
>  // NAME 상속
>  private String email;
>  ...
>}
>
>@Entity
>public class Seller extends BaseEntity {
>  // ID 상속
>  // NAME 상속
>  private String shopName;
>  ...
>}
>```
>>- BaseEntity 에는 객체들이 주로 사용하는 공통 매핑 정보를 정의함
>>- 자식 엔티티들은 상속을 통해 BaseEntity 의 매핑 정보를 물려받음
>>- 여기서 BaseEntity 는 테이블과 매핑할 필요가 없고 자식 엔티티에게 공통으로 사용되는 매핑 정보만 제공하면 되므로 @MappedSuperclass 를 사용함
>>- 부모로부터 물려받은 매핑 정보를 재정의하려면 @AttributeOverrides 나 @AttributeOverride 를 사용하고, 연관관계를 재정의하려면 @AssociationOverrides 나 @AssociationOverride 를 사용함
>>```java
>>// 부모에게 상속받은 id 속성의 컬럼명을 MEMBER_ID 로 재정의함
>>@Entity
>>@AttributeOverride(name = "id", column = @Column(name = "MEMBER_ID"))
>>public class Member extends BaseEntity {...}
>>
>>// 둘 이상의 재정의 하려면 @AttributeOverrides 를 사용함
>>@Entity
>>@AttributeOverrides({
>>  @AttributeOverride(name = "id", column = @Column(name = "MEMBER_ID")),
>>  @AttributeOverride(name = "name", column = @Column(name = "MEMBER_NAME"))
>>})
>>public class Member extends BaseEntity {...}
>>```
>- @MappedSuperclass 의 특징
>>- 테이블과 매핑하지 않고 자식 클래스에 엔티티의 매핑 정보를 상속하기 위해 사용함
>>- @MappedSuperclass 로 지정한 클래스는 엔티티가 아니므로 em.find() 나 JPQL 에서 사용할 수 없음
>>- 이 클래스를 직접 생성해서 사용할 일은 거의 없으므로 추상 클래스로 만드는 것을 권장함
>- 정리
>>- @MappedSuperclass 는 테이블과는 관계가 없고 단순히 엔티티가 공통으로 사용하는 매핑 정보를 모아주는 역할을 할 뿐임
>>- ORM 에서 이야기하는 진정한 상속 매핑은 이전에 학습한 객체 상속을 DB의 슈퍼타입 서브타입 관계와 매핑하는 것임
>>- @MappedSuperclass 를 사용하면 등록일자, 수정일자, 등록자, 수정자 같은 여러 엔티티에서 공통으로 사용하는 속성을 효과적으로 관리할 수 있음
>>- 엔티티는 엔티티이거나 @MappedSuperclass 로 지정한 클래스만 상속받을 수 있음

<br>

[목차로 이동](#목차)

>### 복합 키와 식별 관게 매핑
>- 식별 관계 VS 비식별 관계
>>- DB 테이블 사이의 관계는 외래 키가 기본 키에 포함되는지 여부에 따라 식별 관계와 비식별 관계로 구분함
>>- 식별 관계
>>>- 식별 관계는 부모 테이블의 기본 키를 내려받아서 자식 테이블의 기본 키 + 외래 키로 사용하는 관계임
>>- 비식별 관계
>>>- 비식별 관계는 부모 테이블의 기본 키를 받아서 자식 테이블의 외래 키로만 사용하는 관계임
>>>- 외래 키에 NULL 을 허용하는지에 따라 나뉨
>>>>- 필수적 비식별 관계(Mandatory)
>>>>>- 외래 키에 NULL 을 허용하지 않음
>>>>>- 연관관계를 필수적으로 맺어야 함
>>>>- 선택적 비식별 관계(Optional)
>>>>>- 외래 키에 NULL 을 허용함
>>>>>- 연관관계를 맺을지 말지 선택할 수 있음
>>- 최근에는 비식별 관계를 주로 사용하고 꼭 필요한 곳에만 식별 관계를 사용하는 추세이며, JPA는 식별 관계와 비식별 관계를 모두 지원함
>- 복합 키: 비식별 관계 매핑
>```java
>// 기본 키를 구성하는 칼럼이 하나면 다음처럼 단순하게 매핑함
>@Entity
>public class Hello {
>  @Id
>  private String id;
>}
>
>// 둘 이상의 컬럼으로 구성된 복합 기본 키는 다음처럼 매핑하면 될 것 같지만 매핑 오류가 발생함
>// JPA 에서 식별자를 둘 이상 사용하려면 별도의 식별자 클래스를 만들어야 함
>@Entity
>public class Hello {
>  @Id
>  private String id1;
>  @Id
>  private String id2; // 실행 시점에 매핑 예외 발생
>}
>```
>>- 개요
>>>- JPA는 영속성 컨텍스트에 엔티티를 보관할 때 엔티티의 식별자를 키로 사용함
>>>- 식별자를 구분하기 위해 equals 와 hashCode 를 사용해서 동등성 비교를 함
>>>- 식별자 필드가 하나일 때는 보통 자바의 기본 타입을 사용하므로 문제가 없지만, 식별자 필드가 2개 이상이면 별도의 식별자 클래스를 만들고 그곳에 equals 와 hashCode 를 구현해야 함
>>>- JPA 는 복합 키를 지원하기 위해 @IdClass 와 @EmbeddedId 2가지 방법을 제공하는데 @IdClass 는 관계형 DB에 가까운 방법이고 @EmbeddedId 는 좀 더 객체지향에 가까운 방법임
>>- @IdClass
>>```java
>>// 기본 키를 복합 키로 구성했으므로 복합 키를 매핑하기 위해 식별자 클래스를 별도로 만들어야 함
>>@Entity
>>@IdClass(ParentId.class)
>>public class Parent {
>>  @Id
>>  @Column(name = "PARENT_ID1")
>>  private String id1; // ParentId.id1 과 연결
>>
>>  @Id
>>  @Column(name = "PARENT_ID2")
>>  private String id2; // ParentId.id2 와 연결
>>}
>>
>>// 식별자 클래스
>>public class ParentId implements Serializable {
>>  private String id1; // Parent.id1 매핑
>>  private String id2; // Parent.id2 매핑
>>
>>  public ParentId() {
>>  }
>>
>>  public ParentId(String id1, String id2) {
>>    this.id1 = id1;
>>    this.id2 = id2;
>>  }
>>
>>  @Override
>>  public boolean equals(Object o) {...}
>>  @Override
>>  public int hashCode() {...}
>>}
>>```
>>>- @IdClass 를 사용할 때 식별자 클래스의 조건
>>>>- 식별자 클래스의 속성명과 엔티티에서 사용하는 식별자의 속성명이 같아야 함
>>>>- Serializable 인터페이스를 구현해야 함
>>>>- equals, hashCode 를 구현해야 함
>>>>- 기본 생성자가 있어야 함
>>>>- 식별자 클래스는 public 이어야 함
>>>- 복합 키를 사용하는 엔티티를 저장
>>>```java
>>>Parent parent = new Parent();
>>>parent.setId1("myId1"); // 식별자
>>>parent.setId2("myId2"); // 식별자
>>>parent.setName("parentName");
>>>em.persist(parent);
>>>```
>>>>- 식별자 클래스인 ParentId 가 보이지 않는데, em.persist() 를 호출하면 영속성 컨텍스트에 엔티티를 등록하기 직전에 내부에서 Parent.id1, Parent.id2 값을 이용해서 식별자 클래스인 ParentId 를 생성하고 영속성 컨텍스트의 키로 사용함
>>>- 복합 키로 조회
>>>```java
>>>ParentId parentId = new ParentId("myId1", "myId2");
>>>Parent parent = em.find(Parent.class, parentId);
>>>```
>>>- 자식 클래스 추가
>>>```java
>>>@Entity
>>>public class Child {
>>>  @Id
>>>  private String id;
>>>
>>>  @ManyToOne
>>>  @JoinColumns({
>>>    @JoinColumn(name = "PARENT_ID1", referencedColumnName = "PARENT_ID1"),
>>>    @JoinColumn(name = "PARENT_ID2", referencedColumnName = "PARENT_ID2")
>>>  })
>>>  private Parent parent;
>>>}
>>>```
>>>>- 부모 테이블의 기본 키 컬럼이 복합 키이므로 자식 테이블의 외래 키도 복합 키임
>>>>- 외래 키 매핑 시 여러 컬럼을 매핑해야 하므로 @JoinColumns 어노테이션을 사용하고 각각의 외래 키 컬럼을 @JoinColumn 으로 매핑함
>>>>- @JoinColumn 의 name 속성과 referencedColumnName 속성의 값이 같으면 referencedColumnName 은 생략해도 됨
>>- @EmbeddedId
>>>- @IdClass 가 DB에 맞춘 방법이라면 @EmbeddedId 는 좀 더 객체지향적인 방법임
>>```java
>>// 엔티티에서 식별자 클래스를 직접 사용하고 @EmbeddedId 어노테이션을 사용함
>>@Entity
>>public class Parent {
>>  @EmbeddedId
>>  private ParentId id;
>>  
>>  private String name;
>>}
>>
>>// 식별자 클래스
>>@Embeddable
>>public class ParentId implements Serializable {
>>  @Column(name = "PARENT_ID1")
>>  private String id1;
>>  @Column(name = "PARENT_ID2")
>>  private String id2;
>>  
>>  // equals and hashCode 구현
>>  ...
>>}
>>```
>>>- @EmbeddedId 를 적용한 식별자 클래스는 식별자 클래스에 기본 키를 직접 매핑함
>>>- @EmbeddedId 를 적용한 식별자 클래스의 조건
>>>>- @Embeddable 어노테이션을 붙여줘야 함
>>>>- Serializable 인터페이스를 구현해야 함
>>>>- equals, hashCode 를 구현해야 함
>>>>- 기본 생성자가 있어야 함
>>>>- 식별자 클래스는 public 이어야 함
>>>- @EmbeddedId 를 사용하여 엔티티를 저장
>>>```java
>>>Parent parent = new Parent();
>>>ParentId parentId = new ParentId("myId1", "myId2");
>>>parent.setId(parentId);
>>>parent.setName("parentName");
>>>em.persist(parent);
>>>```
>>>>- 식별자 클래스 parentId를 직접 생성해서 사용함
>>>- 엔티티를 조회
>>>```java
>>>ParentId parentId = new ParentId("myId1", "myId2");
>>>Parent parent = em.find(Parent.class, parentId);
>>>```
>>>>- 조회 코드도 식별자 클래스 parentId 를 직접 사용함
>>- 복합 키와 equals(), hashCode()
>>>- 영속성 컨텍스트는 엔티티의 식별자를 키로 사용해서 엔티티를 관리하며 식별자를 비교할 때 equals() 와 hashCode() 를 사용하므로 식별자 객체의 동등성이 지켜지지 않으면 예상과 다른 엔티티가 조회되거나 엔티티를 찾을 수 없는 등 영속성 컨텍스트가 엔티티를 관리하는 데 심각한 문제가 발생하므로 복합 키는 equals() 와 hashCode() 를 필수로 구현해야 함
>>>- 식별자 클래스는 보통 equals() 와 hashCode() 를 구현할 때 모든 필드를 사용함
>>- @IdClass vs @EmbeddedId
>>>- @EmbeddedId 가 @IdClass 와 비교해서 더 객체지향적이고 중복도 없어서 좋아보이긴 하지만 특정 상황에 JPQL 이 조금 더 길어질 수 있음
>>>```java
>>>em.createQuery("select p.id.id1, p.id.id2 from Parent p"); // @EmbeddedId
>>>em.createQuery("select p.id1, p.id2 from Parent p"); // @IdClass
>>>```
>- 복합 키: 식별 관계 매핑
>>- @IdClass 와 식별 관계
>>```java
>>// 부모
>>@Entity
>>public class Parent {
>>  @Id @Column(name = "PARENT_ID")
>>  private String id;
>>  private String name;
>>  ...
>>}
>>
>>// 자식
>>@Entity
>>@IdClass(ChildId.class)
>>public class Child {
>>  @Id
>>  @ManyToOne
>>  @JoinColumn(name = "PARENT_ID")
>>  public Parent parent;
>>
>>  @Id @Column(name = "CHILE_ID")
>>  private String childId;
>>
>>  private String name;
>>  ...
>>}
>>
>>// 자식ID
>>public class ChildId implements Serializable {
>>  private String parent;  // Child.parent 매핑
>>  private String childId; // Child.childId 매핑
>>
>>  // equals, hashCode
>>  ...
>>}
>>
>>// 손자
>>@Entity
>>@IdClass(GrandChildId.class)
>>public class GrandChild {
>>  @Id
>>  @ManyToOne
>>  @JoinColumns({
>>    @JoinColumn(name = "PARENT_ID"),
>>    @JoinColumn(name = "CHILD_ID")
>>  })
>>  public Child child;
>>
>>  @Id @Column(name = "GRANDCHILE_ID")
>>  private String id;
>>  
>>  private String name;
>>  ...
>>}
>>
>>// 손자ID
>>public class GrandChildId implements Serializable {
>>  private ChildId child;  // GrandChild.parent 매핑
>>  private String id;      // GrandChild.id 매핑
>>
>>  // equals, hashCode
>>  ...
>>}
>>```
>>>- 식별 관계는 기본 키와 외래 키를 같이 매핑해야 하므로 식별자 매핑인 @Id 와 연관관계 매핑인 @ManyToOne 을 같이 사용하면 됨
>>- @EmbeddedID 와 식별 관계
>>>- @EmbeddedId 로 식별 관계를 구성할 때는 @MapsId 를 사용해야 함
>>```java
>>// 부모
>>@Entity
>>public class Parent {
>>  @Id @Column(name = "PARENT_ID")
>>  private String id;
>>  private String name;
>>  ...
>>}
>>
>>// 자식
>>@Entity
>>public class Child {
>>  @EmbeddedId
>>  private ChildId id;
>>
>>  @MapsId("parentId") // ChildId.parentId 매핑
>>  @ManyToOne
>>  @JoinColumn(name = "PARENT_ID")
>>  public Parent parent;
>>
>>  private String name;
>>  ...
>>}
>>
>>// 자식ID
>>@Embeddable
>>public class ChildId implements Serializable {
>>  private String parentId; // @MapsId("parentId") 로 매핑
>>
>>  @Column(name = "CHILD_ID")
>>  private String id;
>>
>>  // equals, hashCode
>>  ...
>>}
>>
>>// 손자
>>@Entity
>>public class GrandChild {
>>  @EmbeddedId
>>  private GrandChildId id;
>>
>>  @MapsId("childId") // GrandChildId.childId 매핑
>>  @ManyToOne
>>  @JoinColumns({
>>    @JoinColumn(name = "PARENT_ID"),
>>    @JoinColumn(name = "CHILD_ID")
>>  })
>>  public Child child;
>>  
>>  private String name;
>>  ...
>>}
>>
>>// 손자ID
>>public class GrandChildId implements Serializable {
>>  private ChildId childId; // @MapsId("childId") 로 매핑
>>
>>  @Column(name = "GRANDCHILD_ID")
>>  private String id;
>>
>>  // equals, hashCode
>>  ...
>>}
>>```
>>>- @EmbeddedId 는 식별 관계로 사용할 연관관계의 속성에 @MapsId 를 사용하면 됨
>>>- @MapsId 는 외래 키와 매핑한 연관관계를 기본 키에도 매핑하겠다는 의미임
>>>- @MapsId 의 속성 값은 @EmbeddedId 를 사용한 식별자 클래스의 기본 키 필드를 지정하면 됨
>- 비식별 관계로 구현
>```java
>// 부모
>@Entity
>public class Parent {
>  @Id @GeneratedValue
>  @Column(name = "PARENT_ID")
>  private Long id;
>  private String name;
>  ...
>}
>
>// 자식
>@Entity
>public class Child {
>  @Id @GeneratedValue
>  @Column(name = "CHILD_ID")
>  private Long id;
>  private String name;
>
>  @ManyToOne
>  @JoinColumn(name = "PARENT_ID")
>  private Parent parent;
>  ...
>}
>
>// 손자
>@Entity
>public class GrandChild {
>  @Id @GeneratedValue
>  @Column(name = "GRANDCHILD_ID")
>  private Long id;
>  private String name;
>
>  @ManyToOne
>  @JoinColumn(name = "CHILD")
>  private Child child;
>  ...
>}
>```
>>- 식별 관계의 복합 키를 사용한 코드와 비교하면 매핑도 쉽고 코드도 단순함
>>- 복합 키가 없으므로 복합 키 클래스를 만들지 않아도 됨
>- 일대일 식별 관계
>```java
>// 부모
>@Entity
>public class Board {
>  @Id @GeneratedValue
>  @Column(name = "BOARD_ID")
>  private Long id;
>
>  private String title;
>
>  @OntToOne(mappedBy = "board")
>  private BoardDetail boardDetail;
>  ...
>}
>
>// 자식
>@Entity
>public class BoardDetail {
>  @Id
>  private Long boardId;
>
>  @MapsId // BoardDetail.boardId 매핑
>  @OneToOne
>  @JoinColumn(name = "BOARD_ID")
>  private Board board;
>
>  private String content;
>  ...
>}
>```
>>- BoardDetail 처럼 식별자가 단순히 컬럼 하나면 @MapsId 를 사용하고 속성 값은 비워두면 됨
>>- 이때 @MapsId 는 @Id 를 사용해서 식별자로 지정한 BoardDetail.boardId 와 매핑됨
>>- 일대일 식별 관계 저장
>>```java
>>public void save() {
>>  Board board = new Board();
>>  board.setTitle("제목");
>>  em.persist(board);
>>
>>  BoardDetail boardDetail = new BoardDetail();
>>  boardDetail.setContent("내용");
>>  boardDetail.setBoard(board);
>>  em.persist(boardDetail);
>>}
>>```
>- 식별, 비식별 관계의 장단점
>>- DB 설계 관점에서 식별 관계보다 비식별 관계를 선호하는 이유
>>>- 식별 관계는 부모 테이블의 기본 키를 자식 테이블로 전파하면서 자식 테이블의 기본 키 컬럼이 점점 늘어나므로 결국 조인할 때 SQL이 복잡해지고 기본 키 인덱스가 불필요하게 커질 수 있음
>>>- 식별 관계는 2개 이상의 컬럼을 합해서 복합 기본 키를 만들어야 하는 경우가 많음
>>>- 식별 관계를 사용할 때 기본 키로 비즈니스 의미가 있는 자연 키 컬럼을 조합하는 경우가 많은 반면, 비식별 관계의 기본 키는 비즈니스와 전혀 관계없는 대리 키는 주로 사용함
>>>>- 비즈니스 요구사항은 시간이 지남에 따라 언젠가는 변하는데 식별 관계의 자연 키 컬럼들이 자식에 손자까지 전파되면 변경하기 힘듬
>>>- 식별 관계는 부모 테이블의 기본 키를 자식 테이블의 기본 키로 사용하므로 비식별 관계보다 테이블 구조가 유연하지 못함
>>- 객체 관계 매핑의 관점에서 비식별 관계를 선호하는 이유
>>>- 일대일 관계를 제외하고 식별 관계는 2개 이상의 컬럼을 묶은 복합 기본 키를 사용하기 때문에 JPA 에서 복합 키는 별도의 복합 키 클래스를 만들어 사용해야 하므로 컬럼이 하나인 기본 키를 매핑하는 것보다 많은 노력이 필요함
>>>- 비식별 관계의 기본 키는 주로 대리 키를 사용하는데 JPA 는 @GenerateValue 처럼 대리 키를 생성하기 위한 편리한 방법을 제공함
>>- 식별 관계의 장점
>>>- 기본 키 인덱스를 활용하기 좋고, 상위 테이블들의 기본 키 컬럼을 자식, 손자 테이블이 가지고 있으므로 특정 상황에 조인 없이 하위 테이블만으로 검색을 완료할 수 있으므로 꼭 필요한 곳에는 적절하게 사용하는 것이 좋음
>>- 정리
>>>- ORM 신규 프로젝트 진행시 비식별 관계를 사용하고 기본 키는 Long 타입의 대리 키를 사용하는것이 좋음
>>>>- 대리 키는 비즈니스와 아무 관련이 없으므로 비즈니스가 변경되어도 유연한 대처가 가능하다는 장점이 있음
>>>>- JPA 는 @GenerateValue 를 통해 간편하게 대리 키를 생성할 수 있고 식별자 컬럼이 하나여서 쉽게 매핑할 수 있음
>>>>- 식별자의 데이터 타입으로 Integer 는 21억이 한계이므로 데이터를 많이 저장하면 문제가 발생할 수 있는 반면에 Long은 920경 정도로 아주 커서 안전함
>>>- 선택적 비식별 관계보다는 필수적 비식별 관계를 사용하는 것이 좋음
>>>>- 선택적인 비식별 관계는 NULL 을 허용하므로 조인할 때에 외부 조인을 사용해야 함
>>>>- 필수적 관계는 NOT NULL 로 항상 관계가 있다는 것을 보장하므로 내부 조인만 사용해도 됨

<br>

[목차로 이동](#목차)

>### 조인 테이블
>- 개요
>>- DB 테이블의 연관관계를 설정하는 두가지 방법
>>>- 조인 컬럼 사용 (외래 키)
>>>- 조인 테이블 사용 (테이블 사용)
>>- 조인 컬럼 사용
>>>- 테이블 간에 관계는 주로 조인 컬럼이라 부르는 외래 키 컬럼을 사용해서 관리함
>>>- 예를 들어 회원과 사물함이 있는데 각각 테이블에 데이터를 등록했다가 회원이 원할 때 사물함을 선택할 수 있다고 가정하면 회원이 사물함을 사용하기 전까지는 아직 둘 사이에 관계가 없으므로 MEMBER 테이블의 LOCKER_ID 외래 키에 null을 입력해두어야하는데 외래 키에 null을 허용하는 관계를 선택적 비식별 관계라 함
>>>- 선택적 비식별 관계는 외래 키에 null을 허용하므로 회원과 사물함을 조인할 때 외부 조인을 사용해야하는데 실수로 내부 조인을 사용하면 사물함과 관계가 없는 회원은 조회되지 않는다는 점과 회원과 사물함이 아주 가끔 관계를 맺는다면 외래 키 값 대부분이 null로 저장되는 단점이 있음
>>- 조인 테이블 사용
>>>- 조인 컬럼을 사용하는 방법은 단순히 외래 키 컬럼만 추가해서 연관관계를 맺지만 조인 테이블을 사용하는 방법은 연관관계를 관리하는 조인 테이블을 추가하고 여기서 두 테이블의 외래 키를 가지고 연관관계를 정리하므로 MEMBER 와 LOCKER 에는 연관관계를 관리하기 위한 외래 키 컬럼이 없음
>>>- 회원과 사물함 데이터를 각각 등록했다가 회원이 원할 때 사물함을 선택하면 조인 테이블에만 값을 추가하면 됨
>>>- 테이블을 하나 추가해야 하므로 관리해야 하는 테이블이 늘어나고 회원과 사물함 두 테이블을 조인하려면 조인 테이블까지 추가로 조인해야 함
>>- 기본은 조인 컬럼을 사용하고 필요하다고 판단될 때 조인 테이블을 사용하는 것이 추천됨
>>- 설명될 내용
>>>- 객체와 테이블을 매핑할 때 조인 컬럼은 @JoinColumn 으로 매핑하고 조인 테이블은 @JoinTable 로 매핑함
>>>- 조인 테이블은 주로 다대다 관계를 일대다, 다대일 관계로 풀어내기 위해 사용하지만 일대일, 일대다, 다대일 관계에서도 사용함
>- 일대일 조인 테이블
>>- 일대일 관계를 만들려면 조인 테이블의 외래 키 컬럼 각각에 총 2개의 유니크 제약조건을 걸어야 함 (PARENT_ID 는 기본 키)
>>```java
>>// 부모
>>@Entity
>>public class Parent {
>>  @Id @GeneratedValue
>>  @Column(name = "PARENT_ID")
>>  private Long id;
>>  private String name;
>>
>>  @OneToOne
>>  @JoinTable(name = "PARENT_CHILD",
>>          joinColumns = @JoinColumn(name = "PARENT_ID"),
>>          inverseJoinColumns = @JoinColumn(name = "CHILD_ID")
>>  )
>>  private Child child;
>>  ...
>>}
>>
>>// 자식
>>@Entity
>>public class Child {
>>  @Id @GeneratedValue
>>  @Column(name = "CHILD_ID")
>>  private Long id;
>>  private String name;
>>  // 양방향 매핑
>>  @OneToOne(mappedBy="child")
>>  private Parent parent;
>>  ...
>>}
>>```
>>>- 부모 엔티티에서 @JoinColumn 대신 @JoinTable 을 사용함
>>>- @JoinTable 의 속성
>>>>- name : 매핑할 조인 테이블 이름
>>>>- joinColumns : 현재 엔티티를 참조하는 외래 키
>>>>- inverseJoinColumns : 반대방향 엔티티를 참조하는 외래 키
>- 일대다 조인 테이블
>>- 일대다 관계를 만들려면 조인 테이블의 컬럼 중 다(N)와 관련된 컬럼인 CHILD_ID 에 유니크 제약 조건을 걸어야 함(CHILD_ID 는 기본 키이므로 유니크 제약조건이 걸려 있음)
>>```java
>>// 부모
>>@Entity
>>public class Parent {
>>  @Id @GeneratedValue
>>  @Column(name = "PARENT_ID")
>>  private Long id;
>>  private String name;
>>
>>  @OneToMany
>>  @JoinTable(name = "PARENT_CHILD",
>>          joinColumns = @JoinColumn(name = "PARENT_ID"),
>>          inverseJoinColumns = @JoinColumn(name = "CHILD_ID")
>>  )
>>  private List<Child> child = new ArrayList<Child>();
>>  ...
>>}
>>
>>// 자식
>>@Entity
>>public class Child {
>>  @Id @GeneratedValue
>>  @Column(name = "CHILD_ID")
>>  private Long id;
>>  private String name;
>>  ...
>>}
>>
>>```
>- 다대일 조인 테이블
>>- 다대일, 일대다 양방향 관계 매핑 코드
>>```java
>>// 부모
>>@Entity
>>public class Parent {
>>  @Id @GeneratedValue
>>  @Column(name = "PARENT_ID")
>>  private Long id;
>>  private String name;
>>
>>  @OneToMany(mappedBy = "parent")
>>  private List<Child> child = new ArrayList<Child>();
>>  ...
>>}
>>
>>// 자식
>>@Entity
>>public class Child {
>>  @Id @GeneratedValue
>>  @Column(name = "CHILD_ID")
>>  private Long id;
>>  private String name;
>>
>>  @ManyToOne(optional = false)
>>  @JoinTable(name = "PARENT_CHILD",
>>    joinColumns = @JoinColumn(name = "CHILD_ID"),
>>    inverseJoinColumn = @JoinColumn(name = "PARENT_ID")
>>  )
>>  private Parent parent;
>>  ...
>>}
>>```
>- 다대다 조인 테이블
>>- 다대다 관계를 만들려면 조인 테이블의 두 컬럼을 합해서 하나의 복합 유니크 제약 조건을 걸어야 함
>>```java
>>// 부모
>>@Entity
>>public class Parent {
>>  @Id @GeneratedValue
>>  @Column(name = "PARENT_ID")
>>  private Long id;
>>  private String name;
>>
>>  @ManyToMany
>>  @JoinTable(name = "PARENT_CHILD",
>>          joinColumns = @JoinColumn(name = "PARENT_ID"),
>>          inverseJoinColumns = @JoinColumn(name = "CHILD_ID")
>>  )
>>  private List<Child> child = new ArrayList<Child>();
>>  ...
>>}
>>
>>// 자식
>>@Entity
>>public class Child {
>>  @Id @GeneratedValue
>>  @Column(name = "CHILD_ID")
>>  private Long id;
>>  private String name;
>>  ...
>>}
>>```
>- 조인 테이블에 컬럼을 추가하면 @JoinTable 전략을 사용할 수 없고, 새로운 엔티티를 만들어서 조인 테이블과 매핑해야 함

<br>

[목차로 이동](#목차)

>### 엔티티 하나에 여러 테이블 매핑
>- 잘 사용하지는 않지만 @SecondaryTable 을 사용하면 한 엔티티에 여러 테이블을 매핑할 수 있음
>```java
>@Entity
>@Table(name = "BOARD")
>@SecondaryTable(name = "BOARD_DETAIL",
>  pkJoinColumns = @PrimaryKeyJoinColumn(name = "BOARD_DETAIL_ID"))
>public class Board {
>  @Id @GeneratedValue
>  @Column(name = "BOARD_ID")
>  private Long id;
>
>  private String title;
>
>  @Column(table = "BOARD_DETAIL")
>  private String content;
>}
>```
>>- Board 엔티티는 @Table 을 사용해서 BOARD 테이블과 매핑했고 @SecondaryTable 을 사용해서 BOARD_DETAIL 테이블을 추가로 매핑함
>>- @SecondaryTable 속성
>>>- @SecondaryTable.name : 매핑할 다른 테이블의 이름
>>>- @SecondaryTable.pkJoinColumns : 매핑할 다른 테이블의 기본 키 컬럼 속성
>>- content 필드는 @Column(table = "BOARD_DETAIL") 을 사용해서 BOARD_DETAIL 테이블의 컬럼에 매핑함
>>>- title 필드처럼 테이블을 지정하지 않으면 기본 테이블인 BOARD에 매핑됨
>- 더 많은 테이블을 매핑하려면 @SecondaryTables 를 사용하면 됨
>```java
>@SecondaryTables({
>  @SecondaryTable(name="BOARD_DETAIL"),
>  @SecondaryTable(name="BOARD_FILE")
>})
>```
>>- @SecondaryTables 를 사용해서 두 테이블을 하나의 엔티티에 매핑하는 방법보다는 테이블당 엔티티를 각각 만들어서 일대일 매핑하는 것을 권장함
>>>- 항상 두 테이블을 조회하므로 최적화 하기 어려운 반면에 일대일 매핑은 원하는 부분만 조회할 수 있고 필요하면 둘을 함께 조회하면 됨

<br>

[목차로 이동](#목차)

---

## 프록시와 연관관계 정리

>### 프록시와 연관관계 정리 개요
>- 프록시와 즉시 로딩, 지연 로딩
>>- 객체는 객체 그래프로 연관된 객체들을 탐색하는데, 객체가 DB에 저장되어 있으므로 연관된 객체를 마음껏 탐색하기는 어려움
>>- JPA 구현체들은 이 문제를 해겷하기위해 프록시라는 기술을 사용함
>>- 프록시를 사용하면 연관된 객체를 처음부터 DB에서 조회하는 것이 아니라, 실제 사용하는 시점에 DB에서 조회할 수 있음
>>- 하지만 자주 함께 사용하는 객체들은 조인을 사용해서 함께 조회하는 것이 효과적임
>>- JPA 는 즉시 로딩과 지연 로딩이라는 방법으로 둘을 모두 지원함
>- 영속성 전이와 고아 객체
>>- JPA 는 연관된 객체를 함께 저장하거나 함께 삭제할 수 있는 영속성 전이와 고아 객체 제거라는 편리한 기능을 제공함

<br>

[목차로 이동](#목차)

>### 프록시
>- 개요
>>- 엔티티를 조회할 때 연관된 엔티티들이 항상 사용되는 것은 아님
>>>- 예를 들어 회원 엔티티를 조회할 때 연관된 팀 엔티티는 비즈니스 로직에 따라 사용될 때도 있지만 그렇지 않을 때도 있음
>>```java
>>// 회원 엔티티
>>@Entity
>>public class Member {
>>  private String username;
>>
>>  @ManyToOne
>>  private Team team;
>>
>>  public Team getTeam() {
>>    return team;
>>  }
>>  public String getUsername() {
>>    return username;
>>  }
>>}
>>
>>// 팀 엔티티
>>@Entity
>>public class Team {
>>  private String name;
>>
>>  public String getNmae() {
>>    return name;
>>  }
>>  ...
>>}
>>
>>// 회원과 팀 정보를 출력하는 비즈니스 로직
>>public void printUserAndTeam(String memberId) {
>>  Member member = em.find(Member.class, memberId);
>>  Team team = member.getTeam();
>>  System.out.println("회원 이름: " + member.getUsername());
>>  System.out.println("소속팀: " + team.getName());
>>}
>>
>>// 회원 정보만 출력하는 비즈니스 로직
>>public String printUser(String memberId) {
>>  Member member = em.find(Member.class, memberId);
>>  System.out.println("회원 이름: " + member.getUsername());
>>}
>>```
>>>- printUser() 메서드는 회원 엔티티만 사용하므로 em.find() 로 회원 엔티티를 조회할 때 회원과 연관된 팀 엔티티(Member.team)까지 DB에서 함께 조회해 두는 것은 효율적이지 않음
>>>- JPA는 이런 문제를 해결하려고 엔티티가 실제 사용될 때까지 DB 조회를 지연하는 방법을 제공하는데, 이것을 지연 로딩이라 함
>>>- 지연 로딩 기능을 사용하려면 실제 엔티티 객체 대신에 DB 조회를 지연할 수 있는 가짜 객체가 필요한데 이것을 프록시 객체라 함
>- 프록시 기초
>>- 개요
>>>- JPA에서 식별자로 엔티티 하나를 조회할 때는 EntityManager.find() 를 사용하는데, 이 메서드는 영속성 컨텍스트에 엔티티가 없으면 DB를 조회함
>>>>- 엔티티를 직접 조회하면 조회한 엔티티를 실제 사용하든 사용하지 않든 DB를 조회하게 됨
>>>- 엔티티를 실제 사용하는 시점까지 DB 조회를 미루고 싶으면 EntityManager.getReference() 메서드를 사용하면 됨
>>>>- 이 메서드를 호출하면 JPA는 DB를 조회하지 않고 실제 엔티티 객체도 생성하지 않는 대신에 DB 접근을 위임한 프록시 객체를 반환함
>>- 프록시의 특징
>>>- 프록시 클래스는 실제 클래스를 상속 받아서 만들어져 실제 클래스와 겉 모양이 같으므로 사용하는 입장에서는 진짜 객체인지 프록시 객체인지 구분하지 않고 사용하면 됨
>>>- 프록시 객체는 실제 객체에 대한 참조(target)를 보관하고 프록시 객체의 메서드를 호출하면 프록시 객체는 실제 객체의 메서드를 호출함
>>- 프록시 객체의 초기화
>>>- 프록시 객체는 member.getName() 처럼 실제 사용될 때 DB를 조회해서 실제 엔티티 객체를 생성하는데 이것을 프록시 객체의 초기화라고 함
>>- 프록시 초기화 예제
>>```java
>>//MemberProxy 반환
>>Member member = em.getReference(Member.class "id1");
>>member.getName(); //1. getName();
>>
>>// 프록시 클래스 예상 코드
>>class MemberProxy extends Member {
>>  Member target = null; // 실제 엔티티 참조
>>  public String getName() {
>>    if(target == null) {
>>      // 2. 초기화 요청
>>      // 3. DB 조회
>>      // 4. 실제 엔티티 생성 및 참조 보관
>>      this.target = ...;
>>    }
>>
>>    // 5. target.getName();
>>    return target.getName();
>>  }
>>}
>>```
>>>- 프록시의 초기화 과정 분석
>>>>1. 프록시 객체에 member.getName() 을 호출해서 실제 데이터를 조회함
>>>>2. 프록시 객체는 실제 엔티티가 생성되어 있지 않으면 영속성 컨텍스트에 실제 엔티티 생성을 요청하는데 이것을 초기화라 함
>>>>3. 영속성 컨텍스트는 DB를 조회해서 실제 엔티티 객체를 생성함
>>>>4. 프록시 객체는 생성된 실제 엔티티 객체의 참조를 Member target 멤버변수에 보관함
>>>>5. 프록시 객체는 실제 엔티티 객체의 getName() 을 호출해서 결과를 반환함
>>- 프록시의 특징
>>>- 프록시 객체는 처음 사용할 때 한 번만 초기화됨
>>>- 프록시 객체를 초기화한다고 프록시 객체가 실제 엔티티로 바뀌는 것은 아니고 프록시 객체가 초기화되면 프록시 객체를 통해서 실제 엔티티에 접근할 수 있음
>>>- 프록시 객체는 원본 엔티티를 상속받은 객체이므로 타입 체크 시에 주의해서 사용해야 함
>>>- 영속성 컨텍스트에 찾는 엔티티가 이미 있으면 DB를 조회할 필요가 없으므로 em.getReference() 를 호출해도 프록시가 아닌 실제 엔티티를 반환함
>>>- 초기화는 영속성 컨텍스트의 도움을 받아야 가능하므로 영속성 컨텍스트의 도움을 받을 수 없는 준영속 상태의 프록시를 초기화하면 문제가 발생하는데 하이버네이트는 org.hibernate.LazyInitializationException 예외를 발생시킴
>- 프록시와 식별자
>>- 엔티티를 프록시로 조회할 때 식별자 값을 파라미터로 전달하는데 프록시 객체는 이 식별자 값을 보관함
>>- 프록시 객체는 식별자 값을 가지고 있으므로 식별자 값을 조회하는 team.getId() 를 호출해도 프록시를 초기화하지 않음
>>>- 엔티티 접근 방식을 프로퍼티(@Access(AccessType.PROPERTY))로 설정한 경우에만 초기화하지 앟음
>>- 엔티티 접근 방식을 필드(@Access(AccessType.FILED))로 설정하면 JPA는 getId() 메서드가 id만 조회하는 메서드인지 다른 필드까지 활용해서 어떤 일을 하는 메서드인지 알지 못하므로 프록시 객체를 초기화함
>>```java
>>Member member = em.find(Member.class, "member1");
>>Team team = em.getReference(Team.class, "team1"); // SQL 을 실행하지 않음
>>member.setTeam(team);
>>```
>>- 연관관계를 설정할 때는 식별자 값만 허용하므로 프록시를 사용하면 DB 접근 횟수를 줄일 수 있음
>>- 연관관계를 설정할 때는 엔티티 접근 방식을 필드로 설정해도 프록시를 초기화하지 않음
>- 프록시 확인
>>- JPA가 제공하는 PersistenceUnitUtil.isLoad(Object entity) 메서드를 사용하면 프록시 인스턴스의 초기화 여부를 확인할 수 있음
>>- 아직 초기화되지 않은 프록시 인스턴스는 false 를 반환하며 이미 초기화되었거나 프록시 인스턴스가 아니면 true를 반환함
>>```java
>>boolean isLoad = em.getEntityManagerFactory()
>>                   .getPersistenceUnitUtil().isLoaded(entity);
>>// 또는 boolean isLoad = em.emf.getPersistenceUnitUtil().isLoaded(entity);
>>
>>System.out.println("isLoad = " + isLoad); // 초기화 여부 확인
>>```
>>- 조회한 엔티티가 진짜 엔티티인지 프록시로 조회한 것인지 확인하려면 클래스명을 직접 출력해보면 됨
>>- 클래스 명 뒤에 ...javassist .. 라 되어 있으면 프록시인 것을 확을 확인할 수 있으며 프록시를 생성하는 라이브러리에 따라 출력 결과는 달라질 수 있음

<br>

[목차로 이동](#목차)

>### 즉시 로딩과 지연 로딩
>- 개요
>>- 프록시 객체는 주로 연관된 엔티티를 지연로딩할 때 사용함
>>- JPA는 개발자가 연관된 엔티티의 조회 시점을 선택할 수 있도록 두 가지 방법을 제공함
>>>- 즉시 로딩
>>>>- 엔티티를 조회할 때 연관된 엔티티도 함께 조회함
>>>>- @ManyToOne(fetch = FetchType.EAGER)
>>>- 지연 로딩
>>>>- 연관된 엔티티를 실제 사용할 때 조회함
>>>>- @ManyToOne(fetch = FetchType.LAZY)
>- 즉시 로딩
>>- 즉시 로딩(EAGER LOADING)을 사용하려면 @ManyToOne 의 fetch 속성을 FetchType.EAGER 로 지정함
>>- 즉시 로딩 시 두 테이블을 조회해야 하므로 쿼리를 2번 실행할 것 같지만, 대부분의 JPA 구현체는 즉시 로딩을 최적화하기 위해 가능하면 조인 쿼리를 사용함
>- 지연 로딩
>>- 지연 로딩(LAZY LOADING)을 사용하려면 @ManyToOne 의 fetch 속성을 FetchType.LAZY 로 지정함
>>- em.find(Member.class, "member1") 을 호출하면 회원만 조회하고 팀은 조회하지 않는 대신에 조회한 회원읜 team 멤버변수에 프록시 객체를 넣어둠
>>- Team team = member.getTeam(); 호출 시 반환되는 팀 객체는 프록시 객체이며, 이 프록시 객체는 실제 사용될 떄까지 데이터 로딩을 미루므로 지연 로딩이라 함

<br>

[목차로 이동](#목차)

>### 지연 로딩 활용
>- 프록시와 컬렉션 래퍼
>>- 하이버네이트는 엔티티를 영속 상태로 만들 때 엔티티에 컬렉션이 있으면 컬렉션을 추적하고 관리할 목적으로 원본 컬렉션을 하이버네이트가 제공하는 내장 컬렉션으로 변경하는데 이것을 컬렉션 래퍼라 함
>>- 엔티티를 지연 로딩하면 프록시 객체를 사용해서 지연 로딩을 수행하지만 주문 내역 같은 컬렉션은 컬렉션 래퍼가 지연 로딩을 처리해줌
>>>- 컬렉션 래퍼도 컬렉션에 대한 프록시 역할을 함
>- JPA 기본 페치 전략
>>- fetch 속성의 기본 설정값
>>>- @ManyToOne, @OneToOne : 즉시 로딩(FetchType.EAGER)
>>>- @OneToMany, @ManyToMany : 지연 로딩(FetchType.LAZY)
>>- JPA 의 기본 페치 전략은 연관된 엔티티가 하나면 즉시 로딩을, 컬렉션이면 지연 로딩을 사용함
>>>- 컬렉션을 로딩하는 것은 비용이 많이 들고 잘못하면 너무 많은 데이터를 로딩할 수 있기 때문임
>>- 추천하는 방법으로 모든 연관관계에 지연 로딩을 사용하고 애플리케이션 개발이 어느 정도 완료단계에 왔을 때 실제 사용하는 상황을 보고 꼭 필요한 곳에만 즉시 로딩을 사용하도록 최적화하는것이 추천됨
>>>- SQL 을 직접 사용하면 각각의 테이블을 조회해서 처리하다가 조인으로 한 번에 조회하도록 변경하려면 많은 SQL과 애플리케이션 코드를 수정해야 하므로 JPA 처럼 유연한 최적화가 어려움
>- 컬렉션에 FetchType.EAGER 사용 시 주의점
>>- 컬렉션을 하나 이상 즉시 로딩하는 것은 권장하지 않음
>>>- 컬렉션과 조인한다는 것은 DB 테이블로 보면 일대다 조인이므로 결과 데이터가 다 쪽에 있는 수만큼 증가하게 되는데, 서로 다른 컬렉션을 2개 이상 조인할 때 두 테이블의 데이터 수의 곱만큼 데이터수가 증가하므로 결과적으로 애플리케이션 성능이 저하될 수 있음
>>- 컬렉션 즉시 로딩은 항상 외부 조인을 사용할것
>>>- 다대일 관계인 회원 테이블과 팀 테이블을 조인할 때 회원 테이블의 외래 키에 not null 제약조건을 걸어두면 모든 회원은 팀에 소속되므로 항상 내부 조인을 사용하도됨
>>>- 팀 테이블에서 회원 테이블로 일대다 관계를 조인할 때 회원이 한 명도 없는 팀을 내부 조인하면 팀까지 조회되지 않는 문제가 발생하는데 DB 제약조건으로 이런 상황을 막을 순 없으므로 JPA는 일대다 관계를 즉시 로딩할 때 항상 외부 조인을 사용함
>>- FetchType.EAGER 설정과 조인 전략
>>>- @ManyToOne, @OneToOne
>>>>- (optional = false) : 내부 조인
>>>>- (optional = true)  : 외부 조인
>>>- @OneToMany, @ManyToMany
>>>>- (optional = false) : 외부 조인
>>>>- (optional = true)  : 외부 조인

<br>

[목차로 이동](#목차)

>### 영속성 전이: CASCADE
>- 개요
>```java
>@Entity
>public class Perent {
>  @Id @GeneratedValue
>  private Long id;
>
>  @OneToMany(mappedBy = "parent")
>  private List<Child> children = new ArrayList<Child>();
>  ...
>}
>
>@Entity
>public class Child {
>  @Id @GeneratedValue
>  private Long id;
>
>  @ManyToOne
>  private Parent parent;
>  ...
>}
>
>// 부모 자식 저장
>private static void saveNoCascase(EntityManager em) {
>  // 부모 저장
>  Parent parent = new Parent();
>  em.persist(parent);
>
>  // 1번 자식 저장
>  Child child1 = new Child();
>  child1.setParent(parent); // 자식 -> 부모 연관관계 설정
>  parent.getChildren().add(child1); // 부모 -> 자식
>  em.persist(child1);
>
>  // 2번 자식 저장
>  Child child2 = new Child();
>  child2.setParent(parent); // 자식 -> 부모 연관관계 설정
>  parent.getChildren().add(child2); // 부모 -> 자식
>  em.persist(child2);
>}
>```
>>- 특정 엔티티를 영속 상태로 만들 때 연관된 엔티티도 함께 영속 상태로 만들고 싶으면 영속성 전이기능을 사용함
>>- JPA 는 CASCADE 옵션으로 영속성 전이를 제공하는데, 부모 엔티티를 저장할 때 자식 엔티티도 함께 저장할 수 있음
>>- JPA 에서 엔티티를 저장할 때 연관된 모든 엔티티는 영속 상태여야 하므로 부모 엔티티를 영속 상태로 만들고 자식 엔티티도 각각 영속 상태로 만듬
>>- 영속성 전이를 사용하면 부모만 영속 상태로 만들면 연관된 자식까지 한 번에 영속 상태로 만들 수 있음
>- 영속성 전이: 저장
>```java
>@Entity
>public class Parent {
>  ...
>  @OneToMany(mappedBy = "parent", cascade = CascadeType.PERSIST)
>  private List<Child> children = new ArrayList<Child>();
>  ...
>}
>```
>>- 부모를 영속화할 때 연관된 자식들도 함께 영속화하라고 cascade = CascadeType.PERSIST 옵션을 설정하여 간편하게 부모와 자식 엔티티를 한 번에 영속화할 수 있음
>>```java
>>private static void saveWithCascase(EntityManager em) {
>>
>>  Child child1 = new Child();
>>  Child child2 = new Child();
>>
>>  Parent parent = new Parent();
>>  child1.setParent(parent); // 연관관계 추가
>>  child2.setParent(parent); // 연관관계 추가
>>  parent.getChildren().add(child1);
>>  parent.getChildren().add(child2);
>>
>>  // 부모 설정, 연관된 자식들 저장
>>  em.persist(child2);
>>}
>>```
>>- 부모만 영속화하면 CascadeType.PERSIST 로 설정한 자식 엔티티까지 함께 영속화해서 저장함
>>- 영속성 전이는 연관관계를 매핑하는 것과는 아무 관련이 없고 단지 엔티티를 영속화할 때 연관된 엔티티도 같이 영속화하는 편리함을 제공할 뿐임
>- 영속성 전이: 삭제
>>- 저장한 부모와 자식 엔티티를 모두 제거하려면 각각의 엔티티를 하나씩 제거해야 함
>>```java
>>Parent findParent = em.find(Parent.class, 1L);
>>Child findChild1 = em.find(Child.class, 1L);
>>Child findChild2 = em.find(Child.class, 2L);
>>
>>em.remove(findChild1);
>>em.remove(findChild2);
>>em.remove(findParent);
>>```
>>- CascadeType.REMOVE 로 설정하고 부모 엔티티만 삭제하면 연관된 자식 엔티티도 함께 삭제됨
>>- 삭제 순서는 외래 키 제약조건을 고려해서 자식을 먼저 삭제하고 부모를 삭제함
>- CASCADE 의 종류
>```java
>public enum CascadeType {
>  ALL,      // 모두 적용
>  PERSIST,  // 영속
>  MERGE,    // 병합
>  REMOVE,   // 삭제
>  REFRESH,  // REFRESH
>  DETACH    // DETACH
>}
>
>// 여러 속성 사용법
>cascade = {CascadeType.PERSIST, CascadeType.REMOVE}
>```
>>- CascadeType.PERSIST, CascadeType.REMOVE 는 em.persist(), em.remove() 를 실행할 때 바로 전이가 발생하지 않고 플러시를 호출할 때 전이가 발생함

<br>

[목차로 이동](#목차)

>### 고아 객체
>- 개요
>>- JPA 는 부모 엔티티와 연관관계가 끊어진 자식 엔티티를 자동으로 삭제하는 기능을 제공하는데 이것을 고아 객체(ORPHAN) 제거라 함
>>- 부모 엔티티의 컬렉션에서 자식 엔티티의 참조만 제거하면 자식 엔티티가 자동으로 삭제됨
>>```java
>>@Entity
>>public class Parent {
>>  @Id @GeneratedValue
>>  private Long id;
>>
>>  @OneToMany(mappedBy = "parent", orphanRemoval = true)
>>  private List<Child> childrent = new ArrayList<Child>();
>>  ...
>>}
>>
>>// 샤용 코드
>>Parent parent1 = em.find(Parent.class, id);
>>parent1.getChildrent().remove(0); // 자식 엔티티를 컬렉션에서 제거
>>```
>>- 고아 객체 제거 기능은 영속성 컨텍스트를 플러시할 때 적용되므로 플러시 시점에 DELETE SQL 이 실행됨
>>- 고아 객체 정리
>>>- 고아 객체 제거는 참조가 제거된 엔티티는 다른 곳에서 참조하지 않는 고아 객체로 보고 삭제하는 기능이므로 참조하는 곳이 하나일 때만 사용해야함
>>>- 특정 엔티티가 개인 소유하는 엔티티에만 이 기능을 적용해야 함
>>>>- 만약 삭제한 엔티티를 다른 곳에서도 참조한다면 문제가 발생할 수 있는 이유로 @OneToOne, @OneToMany 에만 사용할 수 있음
>>>- 부모를 제거하면 자식은 고아가 되므로 자식도 같이 제거되는데, CascadeType.REMOVE 를 설정한 것과 같음

<br>

[목차로 이동](#목차)

>### 영속성 전이 + 고아 객체, 생명주기
>- 일반적으로 엔티티는 EntityManager.persist() 를 통해 영속화되고 EntityManager.remove() 를 통해 제거됨
>>- 이것은 엔티티 스스로 생명주기를 관리한다는 의미임
>- CascadeType.ALL + orphanRemoval = true 를 동시에 사용하면 부모 엔티티를 통해서 자식의 생명주기를 관리할 수 있음
>>- 자식을 저장하려면 부모에 등록만 하면 되고, 삭제하려면 부모에서 제거하면 된다는 의미임

<br>

[목차로 이동](#목차)

---

## 값 타입

>### 값 타입 개요
>- JPA 의 데이터 타입을 가장 크게 분류하면 엔티티 타입과 값 타입으로 나눌 수 있음
>>- 엔티티 타입은 @Entity 로 정의하는 객체이고, 값 타입은 int, Integer, String 처럼 단순히 값으로 사용하는 자바 기본 타입이나 객체를 말함
>>- 엔티티 타입은 식별자를 통해 지속해서 추적할 수 있지만, 값 타입은 식별자가 없고 숫자나 문자같은 속성만 있으므로 추적할 수 없음
>- 값 타입의 3가지 타입
>>- 기본값 타입 : 자바가 제공하는 기본 데이터 타입
>>>- 자바 기본 타입(int, double)
>>>- 래퍼 클래스(Integer)
>>>- String
>>- 임베디드 타입(복합 값 타입) : JPA 에서 사용자가 직접 정의한 값 타입
>>- 컬렉션 값 타입

<br>

[목차로 이동](#목차)

>### 임베디드 타입(복합 값 타입)
>- 개요
>>- 새로운 값 타입을 직접 정의해서 사용할 수 있는데 JPA 에서는 이것을 임베디드 타입이라 함
>>- 직접 정의한 임베디드 타입도 값 타입임
>```java
>@Entity
>public class Member {
>  @Id @GeneratedValue
>  private Long id;
>  private String name;
>
>  // 근무 기간
>  @Temporal(TemporalType.DATE) java.util.Date startDate;
>  @Temporal(TemporalType.DATE) java.util.Date endDate;
>
>  // 집 주소 표현
>  private String city;
>  private String street;
>  private String zipcode;
>}
>```
>>- 회원이 상세한 데이터를 그대로 가지고 있는 것은 객체지향적이지 않으며 응집력만 떨어트림
>>- 근무 기간, 주소 같은 타입이 있다면 코드가 더 명확해짐
>```java
>@Entity
>public class Member {
>  @Id @GeneratedValue
>  private Long id;
>  private String name;
>
>  @Embedded Period workPeriod;    // 근무 기간
>  @Embedded Address homeAddress;  // 집 주소 표현
>}
>
>@Embeddable
>public class Period {
>  @Temporal(TemporalType.DATE) java.util.Date startDate;
>  @Temporal(TemporalType.DATE) java.util.Date endDate;
>  //..
>
>  public boolean isWork(Date date) {
>    //.. 값 타입을 위한 메서드를 정의할 수 있음
>  }
>}
>
>@Embeddable
>public class Address {
>  @Column(name="city") // 매핑할 컬럼 정의 가능
>  private String city;
>  private String street;
>  private String zipcode;
>}
>```
>>- 임베디드 타입을 사용하기 위한 2가지 어노테이션, 둘 중 하나는 생략 가능함
>>>- @Embeddable : 값 타입을 정의하는 곳에 표시
>>>- @Embedded : 값 타입을 사용하는 곳에 표시
>>- 임베디드 타입은 기본 생성자가 필수임
>>- 임베디드 타입을 포함한 모든 값 타입은 엔티티의 생명주기에 의존하므로 엔티티와 임베디드 타입의 관계는 컴포지션 관계임
>- 임베디드 타입과 테이블 매핑
>>- 임베디드 타입은 엔티티의 값일 뿐이므로 값이 속한 엔티티의 테이블에 매핑함
>>>- 임베디드 타입을 사용하기 전과 후에 매핑하는 테이블은 같음
>>- 임베디드 타입을 사용하면 객체와 테이블을 아주 세밀하게 매핑하는 것이 가능하고, 잘 설계한 ORM 애플리케이션은 매핑한 테이블의 수보다 클래스의 수가 더 많음
>- 임베디드 타입과 연관관계
>>- 임베디드 타입은 값 타입을 포함하거나 엔티티를 참조할 수 있음
>```java
>@Entity
>public class Member {
>  @Embedded Address address;         // 임베디드 타입 포함
>  @Embedded PhoneNumber phoneNumber; // 임베디드 타입 포함
>  //...
>}
>
>@Embeddable
>public class Address {
>  String street;
>  String city;
>  String state;
>  @Embedded Zipcode zipcode;   // 임베디드 타입 포함
>}
>
>@Embeddable
>public class Zipcode {
>  String zip;
>  String plusFour;
>}
>
>@Embeddable
>public class PhoneNumber {
>  String areaCode;
>  String localNumber;
>  @ManyToOne PhoneServiceProvider provider; // 엔티티 참조
>  ...
>}
>
>@Entity
>public class PhoneServiceProvider {
>  @Id String name;
>  ...
>}
>```
>- @AttributeOverride: 속성 재정의
>>- 임베디드 타입에 정의한 매핑정보를 재정의하려면 엔티티에 @AttributeOverride를 사용함
>```java
>@Entity
>public class Member {
>  @Id @GeneratedValue
>  private Long id;
>  private String name;
>
>  @Embedded Address homeAddress;
>  @Embedded Address companyAddress;
>}
>```
>>- 주소가 하나 더 필요하여 추가한 경우 테이블에 매핑하는 컬럼명이 중복된다는 문제가 있음
>>>- @AttributeOverrides 를 사용하여 매핑정보를 재정의 해야함
>```java
>@Entity
>public class Member {
>  @Id @GeneratedValue
>  private Long id;
>  private String name;
>
>  @Embedded Address homeAddress;
> 
>  @Embedded
>  @AttributeOverrides({
>    @AttributeOverride(name="city", column=@Column(name = "COMPANY_CITY")),
>    @AttributeOverride(name="street", column=@Column(name = "COMPANY_STREET")),
>    @AttributeOverride(name="zipcode", column=@Column(name = "COMPANY_ZIPCODE"))
>  })
>  Address companyAddress;
>}
>```
>>- @AttributeOverrides 를 사용하면 어노테이션을 너무 많이 사용해서 엔티티 코드가 지저분해 짐
>>- 다행히 한 엔티티에 같은 임베디드 타입을 중복해서 사용할 일은 많지 않음
>>- @AttributeOverrides 는 엔티티에 설정해야하며, 임베디드 타입이 임베디드 타입을 가지고 있어도 엔티티에 설정해야 함
>- 임베디드 타입과 null
>>- 임베디드 타입이 null 이면 매핑한 컬럼 값은 모두 null 이 됨

<br>

[목차로 이동](#목차)

>### 값 타입과 불변 객체
>- 값 타입 공유 참조
>>- 임베디드 타입 같은 값 타입을 여러 엔티티에서 공유하면 위험함
>>>- 공유 참조로 인해 발생하는 버그는 정말 찾아내기 어려움
>>>- 뭔가를 수정했는데 전혀 예상치 못한 곳에서 문제가 발생하는 것을 부작용 (side effect) 이라 함
>>>- 부작용을 막으려면 값을 복사해서 사용해야 함
>- 값 타입 복사
>>- 값 타입의 실제 인스턴스인 값을 공유하는 것은 위험하므로 값(인스턴스)을 복사해서 사용해야 함
>>- 항상 값을 복사해서 사용하면 공유 참조로 인해 발생하는 부작용을 피할 수 있음
>>- 임베디드 타입처럼 직접 정의한 값 타입은 자바의 기본 타입이 아니라 객체타입이라는 것이 문제임
>>>- 객체를 대입할 때마다 인스턴스를 복사해서 대입하면 공유 참조를 피할 수 있지만, 복사하지 않고 원본의 참조 값을 직접 넘기는 것을 막을 방법이 없음
>>- 객체의 공유 참조는 피할 수 없으므로 근본적인 해결책으로 수정자 메서드를 모두 제거하여 객체의 값을 수정하지 못하게 막으면 부작용의 발생을 막을 수 있음
>- 불변 객체
>>- 값 타입은 부작용 걱정 없이 사용할 수 있어야 하며, 부작용이 일어나면 값 타입이라 할 수 없음
>>- 객체를 불변하게 만들면 값을 수정할 수 없으므로 부작용을 원천 차단할 수 있으므로 값 타입은 될 수 있으면 불변 객체로 설계해야 함
>>- 한 번 만들면 절대 변경할 수 없는 객체를 불변 객체라 하며 불변 객체의 값은 조회할 수 있지만 수정할 수 없음
>>- 불변 객체를 구현하는 가장 간단한 방법은 생상자로만 값을 설정하고 수정자를 만들지 않으면 됨

<br>

[목차로 이동](#목차)

>### 값 타입 컬렉션
>- 개요
>>- 값 타입을 하나 이상 저장하려면 컬렉션에 보관하고 @ElementCollection, @CollectionTable 어노테이션을 사용하면 됨
>```java
>@Entity
>public class Member {
>  @Id @GeneratedValue
>  private Long id;
>
>  @Embedded
>  private Address homeAddress;
>
>  @ElementCollection
>  @CollectionTable(name = "FAVORITE_FOODS",
>    JoinColumns = @JoinColumn(name = "MEMBER_ID"))
>  @Column(name = "FOOD_NAME")
>  private Set<String> favoriteFoods = new HashSet<String>();
>
>  @ElementCollection
>  @CollectionTable(name = "ADDRESS", JoinColumns = @JoinColumn(name = "MEMBER_ID"))
>  @Column(name = "FOOD_NAME")
>  private List<Address> addressHistory = new ArrayList<Address>();
>  //...
>}
>
>@Embeddable
>public class Address {
>  @Column
>  private String city;
>  private String street;
>  private String zipcode;
>  //...
>}
>```
>- 값 타입 컬렉션 사용
>```java
>Member member = new Member();
>
>// 임베디드 값 타입
>member.setHomeAddress(new Address("통영","몽돌해수욕장","660-123"));
>
>// 기본값 타입 컬렉션
>member.getFavoriteFoods().add("짬뽕");
>member.getFavoriteFoods().add("짜장");
>member.getFavoriteFoods().add("탕수육");
>
>// 임베디드 값 타입 컬렉션
>member.getAddressHistory().add(new Address("서울","강남","123-123"));
>member.getAddressHistory().add(new Address("서울","강북","000-000"));
>
>em.persist(member);
>```
>>- JPA는 member 엔티티만 영속화해도 member 엔티티의 값 타입도 함께 저장함
>>- 실제 DB에 실행되는 INSERT SQL
>>>1. member: INSERT SQL 1번
>>>2. member.homeAddress: 컬렉션이 아닌 임베디드 값 타입이므로 회원테이블을 저장하는 SQL에 포함됨
>>>3. member.favoriteFoods: INSERT SQL 3번
>>>4. member.addressHistory: INSERT SQL 2번
>>- em.persist(member) 한 번 호출로 총 6번의 INSERT SQL을 실행함 (플러시 할 때 SQL 전달)
>>- 값 타입 컬렉션은 영속성 전이(Cascade) + 고아 객체 제거(ORPHAN REMOVE) 기능을 필수로 가잠
>>- 값 타입 컬렉션도 조회할 때 페치 전략을 선택할 수 있는데 LAZY가가 기본임
>>>- @ElementCollection(fetch = FetchType.LAZY)
>>- 조회
>>```java
>>// SQL: SELECT ID, CITY, STREET, ZIPCODE FROM MEMBER WHERE ID = 1
>>Member member = em.find(Member.class, 1L); // 1. member
>>
>>// 2. member.homeAddress
>>Address homeAddress = member.getHomeAddress();
>>
>>// 3. member.favoriteFoods
>>Set(String) favoriteFoods = member.getFavoriteFoods(); // LAZY
>>
>>// SQL: SELECT MEMBER_ID, FOOD_NAME FROM FAVORITE_FOODS WHERE MEMBER_ID=1
>>for(String favoriteFood : favoriteFoods) {
>>  System.out.println("favoriteFood = " + favoriteFood);
>>}
>>
>>// 4. member.addressHistory
>>List<Address> addressHistory = member.getAddressHistory(); // LAZY
>>
>>// SQL: SELECT MEMBER_ID, CITY, STREET, ZIPCODE FROM ADDRESS WHERE MEMBER_ID=1
>>addressHistory.get(0);
>>```
>>- 수정
>>```java
>>Member member = em.find(Member.class, 1L);
>>
>>// 1. 임베디드 값 타입 수정
>>member.setHomeAddress(new Address("새로운도시", "신도시1", "123456"));
>>
>>// 2. 기본값 타입 컬렉션 수정
>>Set<String> favoriteFoods = member.getFavoriteFoods();
>>favoriteFoods.remove("탕수육");
>>favoriteFoods.add("치킨");
>>
>>// 3. 임베디드 값 타입 컬렉션 수정
>>List<Address> addressHistory = member.getAddressHistory();
>>addressHistory.remove(new Address("서울","강남","123-123"));
>>addressHistory.add(new Address("새로운도시","새로운주소","123-456"));
>>```
>>>- 기본값 타입 컬렉션 수정
>>>>- 자바의 String 타입은 수정할 수 없으므로 탕수육을 치킨으로 변경하려면 탕수육을 제거하고 치킨을 추가해야함
>>>- 임베디드 값 타입 컬렉션 수정
>>>>- 값 타입은 불변해야 하므로 컬렉션에서 기존 주소를 삭제하고 새로운 주소를 등록함, 값 타입은 equals, hashcode 를 꼭 구현해야 함
>- 값 타입 컬렉션의 제약사항
>>- 엔티티는 식별자가 있으므로 엔티티의 값을 변경해도 식별자로 DB에 저장된 원본 데이터를 쉽게 찾아서 변경할 수 있는 반면에 값 타입은 식별자라는 개념이 없고 단순한 값들의 모음이므로 값을 변경해버리면 DB에 저장된 원본 데이터를 찾기는 어려움
>>- 특정 엔티티 하나에 소속된 값 타입은 값이 변경되어도 자신이 소속된 엔티티를 DB에서 찾고 값을 변경하면 되지만 값 타입 컬렉션에 보관된 값 타입들은 별도의 테이블에 보관되므로 여기에 보관된 값 타입의 값이 변경되면 DB에 있는 원본 데이터를 찾기 어렵다는 문제가 있음
>>>- JPA 구현체들은 값 타입 컬렉션에 변경 사항이 발생하면, 값 타입 컬렉션이 매핑된 테이블의 연관된 모든 데이터를 삭제하고, 현재 값 타입 컬렉션 객체에 있는 모든 값을 DB에 다시 저장함
>>>- 예를들어 식별자가 100번인 회원이 관리하는 주소 값 타입 컬렉션을 변경하면 테이블에서 회원 100번과 관련된 모든 주소 데이터를 삭제하고 현재 값 타입 컬렉션에 있는 값을 다시 저장함
>>- 실무에서는 값 타입 컬렉션이 매핑된 테이블에 데이터가 많다면 값 타입 컬렉션 대신에 일대다 관계를 고려해야 함
>>- 값 타입 컬렉션을 매핑하는 테이블은 모든 컬럼을 묶어서 기본 키를 구성해야 하므로 DB 기본 키 제약 조건으로 인해 컬럼에 null 을 입력할 수 없고, 같은 값을 중복해서 저장할 수 없는 제약도 있음
>>- 위에 설명된 문제를 해결하려면 값 타입 컬렉션 대신 새로운 엔티티를 만들어서 일대다 관계로 설정하고 영속성 전이(Cascade) + 고아 객체 제거(ORPHAN REMOVE) 기능을 적용하면 값 타입 컬렉션처럼 사용할 수 있음
>>```java
>>@Entity
>>public class AddressEntity {
>>  @Id
>>  @GeneratedValue
>>  private Long id;
>>
>>  @Embedded Address address;
>>  ...
>>}
>>
>>// 설정 코드
>>@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
>>@JoinColumn(name = "MEMBER_ID")
>>private List<AddressEntity> addressHistory = new ArrayList<AddressEntity>();
>>```

<br>

[목차로 이동](#목차)

>### 값 타입 정리
>- 엔티티 타입의 특징
>>- 식별자(@id)가 있음
>>>- 엔티티 타입은 식별자가 있고 식별자로 구별할 수 있음
>>- 생명 주기가 있음
>>>- 생성하고, 영속화하고, 소멸하는 생명 주기가 있음
>>>- em.persist(entity) 로 영속화함
>>>- em.remove(entitiy) 로 제거함
>>- 공유할 수 있음
>>>- 참조 값을 공유할 수 있는데 이를 공유 참조라 함
>>>>- 회원 엔티티가 있다면 다른 엔티티에서 얼마든지 회원 엔티티를 참조할 수 있음
>- 값 타입의 특징
>>- 식별자가 없음
>>- 생명 주기를 엔티티에 의존함
>>>- 스스로 생명주기를 가지지 않고 엔티티에 의존하며, 의존하는 엔티티를 제거하면 같이 제거됨
>>- 공유하지 않는 것이 안전함
>>>- 엔티티 타입과는 다르게 공유하지 않는 것이 안전하며, 대신에 값을 복사해서 사용해야 함
>>>- 오직 하나의 주인만이 관리해야 함
>>>- 불변 객체로 만드는 것이 안전함
>>- 식별자가 필요하고 지속해서 값을 추적하고 구분하고 변경해야 한다면 그것은 값 타입이 아닌 엔티티임
>>>- 값 타입은 정말 값 타입이라 판단될 때만 사용해야 하며 특히 엔티티와 값 타입을 혼동해서 엔티티를 값 타입으로 만들면 안 됨

<br>

[목차로 이동](#목차)

---

## 객체지향 쿼리 언어

>### 객체지향 쿼리 언어 개요
>- 목차
>>- 객체지향 쿼리 소개
>>- JPQL
>>- Criteria
>>- QueryDSL
>>- 네이티브 SQL
>>- 객체지향 쿼리 심화
>- JPA는 복잡한 검색 조건을 사용해서 엔티티 객체를 조회할 수 있는 다양한 쿼리 기술을 지원함
>>- JPQL, Criteria, QueryDSL 과 같은 다양한 쿼리 기술을 다루므로 분량이 많음
>- JPQL 은 가장 중요한 객체지향 쿼리이며 Criteria 나 QueryDSL 은 결국 JPQL 을 편리하게 사용하도록 도와주는 기술이므로 JPA 를 다루는 개발자라면 JPQL 을 필수로 학습해야 함

<br>

[목차로 이동](#목차)

>### 객체지향 쿼리 소개
>- 