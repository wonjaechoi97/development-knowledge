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

>### 이클립스 설치와 프로젝트 불러오기