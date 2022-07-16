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

>### 엔티티 매니저 팩토리와 엔티티 매니저
>- 개요
>>- 