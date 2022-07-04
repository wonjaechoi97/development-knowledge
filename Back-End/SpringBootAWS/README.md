# **SpringBoot & AWS** ⌨

### 참고자료

- 스프링 부트와 AWS로 혼자 구현하는 웹 서비스

### 학습 프로젝트 저장소

- [링크](https://github.com/water-case/hello_IntelliJ)

## 목차
>- [주의사항](#주의사항)
>- [스프링 부트에서 테스트코드를 작성](#스프링-부트에서-테스트코드를-작성)
>>- [테스트코드 소개](#테스트코드-소개)
>>- [Hello Controller 테스트 코드 작성](#hello-controller-테스트-코드-작성)
>>- [롬복 소개 및 설치](#롬복-소개-및-설치)
>>- [Hello Controller 코드를 롬복으로 전환](#hello-controller-코드를-롬복으로-전환)
>- [JPA로 데이터베이스 다루기](#jpa로-데이터베이스-다루기)
>>- [JPA 소개](#jpa-소개)
>>- [프로젝트에 Spring Data JPA 적용하기](#프로젝트에-spring-data-jpa-적용하기)
>>- [Spring Data JPA 테스트 코드 작성하기](#spring-data-jpa-테스트-코드-작성하기)
>>- [등록 / 수정 / 조회 API 만들기](#등록--수정--조회-api-만들기)
>>- [JPA Auditing 으로 생성시간 / 수정시간 자동화하기](#jpa-auditing-으로-생성시간--수정시간-자동화하기)
>- [머스테치로 화면 구성하기](#머스테치로-화면-구성하기)
>>- [서버 템플릿 엔진과 머스테치 소개](#서버-템플릿-엔진과-머스테치-소개)
>>- [기본 페이지 만들기](#기본-페이지-만들기)
>>- [게시글 등록 화면 만들기](#게시글-등록-화면-만들기)
>>- [전체 조회 화면 만들기](#전체-조회-화면-만들기)
>>- [게시글 수정, 삭제 화면 만들기](#게시글-수정-삭제-화면-만들기)
>- [스프링 시큐리티와 OAuth 2.0 으로 로그인 기능 구현하기](#스프링-시큐리티와-oauth-20-으로-로그인-기능-구현하기)
>>- [스프링 시큐리티와 스프링 시큐리티 Oauth2 클라이언트](#스프링-시큐리티와-스프링-시큐리티-oauth2-클라이언트)
>>- [구글 서비스 등록](#구글-서비스-등록)
>>- [구글 로그인 연동하기](#구글-로그인-연동하기)
>>- [어노테이션 기반으로 개선하기](#어노테이션-기반으로-개선하기)
>>- [세션 저장소로 데이터베이스 사용하기](#세션-저장소로-데이터베이스-사용하기)
>>- [네이버 로그인](#네이버-로그인)
>>- [기존 테스트에 시큐리티 적용하기](#기존-테스트에-시큐리티-적용하기)
>- [AWS 서버 환경을 만들어보자 - AWS EC2](#aws-서버-환경을-만들어보자---aws-ec2)
>>- [AWS EC2 개요](#aws-ec2-개요)
>>- [EC2 인스턴스 생성하기 (Elastic Compute Cloud)](#ec2-인스턴스-생성하기-elastic-compute-cloud)
>>- [EC2 서버에 접속하기](#ec2-서버에-접속하기)
>>- [아마존 리눅스 서버 생성 시 꼭 해야 할 설정들](#아마존-리눅스-서버-생성-시-꼭-해야-할-설정들)
>- [AWS에 데이터베이스 환경을 만들어보자 - AWS RDS](#aws에-데이터베이스-환경을-만들어보자---aws-rds)
>>- [AWS RDS 개요](#aws-rds-개요)
>>- [RDS 인스턴스 생성하기](#rds-인스턴스-생성하기)
>>- [RDS 운영환경에 맞는 파라미터 설정하기](#rds-운영환경에-맞는-파라미터-설정하기)
>>- [내 PC에서 RDS에 접속해 보기](#내-pc에서-rds에-접속해-보기)
>>- [EC2에서 RDS에 접근 확인](#ec2에서-rds에-접근-확인)
>- [EC2 서버에 프로젝트를 배포해보자](#ec2-서버에-프로젝트를-배포해보자)
>>- [EC2에 프로젝트 Clone 받기](#ec2에-프로젝트-clone-받기)
>>- [배포 스크립트 만들기](#배포-스크립트-만들기)
>>- [외부 Security 파일 등록하기](#외부-security-파일-등록하기)
>>- [스프링 부트 프로젝트로 RDS 접근하기](#스프링-부트-프로젝트로-rds-접근하기)
>>- [EC2에서 소셜 로그인하기](#ec2에서-소셜-로그인하기)
>- [코드가 푸시되면 자동으로 배포해보자 - Travis CI 배포 자동화](#코드가-푸시되면-자동으로-배포해보자---travis-ci-배포-자동화)
>>- [CI & CD 소개](#ci--cd-소개)


<br>

---

>## 주의사항
>- 문서 작성일 (22/06/25) 기준 gradle 최신버전은 7 버전이지만 참고자료 서적은 gradle 4 버전 기준으로 작성되었고 저자 또한 첫 학습은 4 버전 환경을 추천하여 작성자 본인의 실습 또한 4.10.2 버전으로 다운그레이드하여 진행하였음
>- Java 8 을 기준으로 작성되어있고, 인텔리제이 커뮤니티 버전을 IDE로 사용함


<br>

[목차로 이동](#목차)

## 스프링 부트에서 테스트코드를 작성

>### 테스트코드 소개
>- TDD != 단위 테스트
>>- TDD (Test Driven Development) [[학습자료링크]((https://repo.yona.io/doortts/blog/issue/1))]
>>>- 테스트가 주도하는 개발로서, 테스트 코드를 먼저 작성하는 것부터 프로젝트를 시작함
>>>- 레드 그린 사이클
>>>>- 항상 실패하는 테스트를 먼저 작성하고 (Red)
>>>>- 테스트가 통과하는 프로덕션 코드를 작성하고 (Green)
>>>>- 테스트가 통과하면 프로덕션 코드를 리팩토링함 (Refactor)
>>- 단위 테스트 (Unit Test)
>>>- TDD의 첫 번째 단계인 기능 단위의 테스트 코드를 작성하는 것
>- 테스트 코드의 장점
>>- 단위 테스트는 개발단계 초기에 문제를 발견하게 도와줌
>>- 단위 테스트는 개발자가 나중에 코드를 리팩토링하거나 라이브러리 업그레이드 등에서 기존 기능이 올바르게 작동하는지 확인할 수 있음 (예. 회귀 테스트)
>>- 단위 테스트는 기능에 대한 불확실성을 감소시킬 수 있음
>>>- 새로운 기능을 추가할 때, 기존 기능이 잘 작동되는 것을 보장함
>>- 단위 테스트는 시스템에 대한 실제 문서를 제공함. 즉, 단위 테스트 자체를 문서로 사용할 수 있음
>>- 테스트 코드를 작성하는 프레임 워크 종류
>>>- JUnit - JAVA
>>>- DBUtil - DB
>>>- CppUnit - C++
>>>- NUnit - .net

<br>

[목차로 이동](#목차)

>### Hello Controller 테스트 코드 작성
>- 프로젝트 생성
>>- 일반적으로 패키지명은 웹 사이트 주소의 역순
>- @SpringBootApplication
>>```java
>>package org.example.springboot;
>>
>>@SpringBootApplication
>>public class Application {
>>    public static void main(String[] args) {
>>        SpringApplication.run(Application.class, args);
>>    }
>>}
>>```
>>- 프로젝트의 메인 클래스
>>- 스프링 부트의 자동 설정, 스프링 Bean 읽기와 생성
>>- 해당 클래스의 위치부터 설정을 읽어가기 때문에, 항상 프로젝트의 최상단에 위치
>>- main 메서드에서 실행하는 SpringApplication.run 으로 인해 내장 WAS를 실행
>>>- 내장 WAS를 실행하므로 War 파일이 아닌 Jaw 파일만으로 실행할 수 있음
>>>- **'언제 어디서나 같은 환경에서 스프링 부트를 배포'** 할 수 있기 때문에 내장 WAS 사용을 권장
>- HelloController
>>```java
>>package org.example.springboot.web;
>>
>>@RestController
>>public class HelloController {
>>    @GetMapping("/hello")
>>    public String hello() {
>>        return "hello";
>>    }
>>}
>>```
>- HelloControllerTest
>>```java
>>package org.example.springboot.web;
>>
>>@RunWith(SpringRunner.class)
>>@WebMvcTest(controllers = HelloController.class)
>>public class HelloControllerTest {
>>    @Autowired
>>    private MockMvc mvc;
>>
>>    @Test
>>    public void hello가_리턴된다() throws Exception {
>>        String hello = "hello";
>>
>>        mvc.perform(get("/hello"))
>>                .andExpect(status().isOk())
>>                .andExpect(content().string(hello));
>>    }
>>}
>>```
>>- @RunWith(SpringRunner.class)
>>>- 테스트를 진행할 때 JUnit에 내장된 실행자가 아닌 SpringRunner 를 실행
>>>- 스프링 부트 테스트와 JUnit 사이에 연결자 역할
>>- @WebMvcTest
>>>- 여러 스프링 테스트 어노테이션 중, Web(Spring MVC)에 집중할 수 있는 어노테이션
>>>- 선언시 @Controller, @ControllerAdvice 등 사용할 수 있음
>>>- @Service, @Component, @Repository 등 사용 불가
>>- private MockMvc mvc
>>>- 웹 API를 테스트할 때 사용하며 스프링 MVC 테스트의 시작점
>>>- 해당 클래스를 통해 HTTP GET, POST 등에 대한 API 테스트를 할 수 있음
>>- mvc.perform(get("/hello"))
>>>- MockMvc를 통해 /hello 주소로 HTTP GET 요청
>>>- 체이닝이 지원되어 여러 검증 기능을 이어서 선언할 수 있음
>>- .andExpect(status().isOk())
>>>- mvc.perform의 결과와 HTTP Header의 status를 검증함
>>>- 흔히 알고 있는 200, 404, 500 등의 상태를 검증하며 isOk는 200인지 아닌지를 검증
>>- .andExpect(content().string(hello))
>>>- mvc.perform의 결과와 응답 본문의 내용을 검증
>>>- Controller에서 "hello"를 리턴하는지 검증

<br>

[목차로 이동](#목차)

>### 롬복 소개 및 설치
>- 소개
>>- 자바 개발에 있어 자주 사용하는 코드 Getter, Setter, 기본 생성자, toString 등을 어노테이션으로 자동생성
>- 설치
>>- 설치 후 Ctrl + Shift + a 에서 Annotation Processor를 검색 후 Enable annotation processing을 체크해야 함

<br>

[목차로 이동](#목차)

>### Hello Controller 코드를 롬복으로 전환
>- Dto 생성
>>```java
>>package org.example.springboot.web.dto;
>>
>>@Getter
>>@RequiredArgsConstructor
>>public class HelloResponseDto {
>>    private final String name;
>>    private final int amount;
>>}
>>```
>>- @RequiredArgsConstructor
>>>- 선언된 모든 final 필드가 포함된 생성자 생성
>>>- final이 없는 필드는 생성자에 포함되지 않음
>- Dto 테스트 코드
>>```java
>>package org.example.springboot.web.dto;
>>
>>import org.junit.Test;
>>import static org.assertj.core.api.Assertions.assertThat;
>>
>>public class HelloResponseDtoTest {
>>    @Test
>>    public void 롬복_기능_테스트() {
>>        //given
>>        String name = "test";
>>        int amount = 1000;
>>
>>        // when
>>        HelloResponseDto dto = new HelloResponseDto(name, amount);
>>
>>        //then
>>        assertThat(dto.getName()).isEqualTo(name);
>>        assertThat(dto.getAmount()).isEqualTo(amount);
>>    }
>>}
>>```
>>- assertThat
>>>- assertj 테스트 검증 라이브러리의 검증 메서드로써, 검증하고 싶은 대상을 메서드 인자로 받음
>>>- 메서드 체이닝이 지원되어 isEqualTo 와 같이 메서드를 이어서 사용할 수 있음
>>- isEqualTo
>>>- assertj 의 동등 비교 메서드
>>>- assertThat 에 있는 값과 isEqualTo 의 값을 비교하여 같을 때만 성공
>- Controller 테스트 코드
>>```java
>>package org.example.springboot.web;
>>
>>import org.example.springboot.web.HelloController;
>>import org.junit.Test;
>>import org.junit.runner.RunWith;
>>import org.springframework.beans.factory.annotation.Autowired;
>>import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
>>import org.springframework.test.context.junit4.SpringRunner;
>>import org.springframework.test.web.servlet.MockMvc;
>>
>>import static org.hamcrest.Matchers.is;
>>import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
>>import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;
>>
>>@RunWith(SpringRunner.class)
>>@WebMvcTest(controllers = HelloController.class)
>>public class HelloControllerTest {
>>
>>    @Autowired
>>    private MockMvc mvc;
>>
>>    @Test
>>    public void hello가_리턴된다() throws Exception {
>>        String hello = "hello";
>>
>>        mvc.perform(get("/hello"))
>>                .andExpect(status().isOk())
>>                .andExpect(content().string(hello));
>>    }
>>
>>    @Test
>>    public void helloDto가_리턴된다() throws Exception {
>>        String name = "hello";
>>        int amount = 1000;
>>
>>        mvc.perform(
>>                    get("/hello/dto")
>>                        .param("name", name)
>>                        .param("amount", String.valueOf(amount)))
>>                .andExpect(status().isOk())
>>                .andExpect(jsonPath("$.name", is(name)))
>>                .andExpect(jsonPath("$.amount", is(amount)));
>>    }
>>}
>>```
>>- .param
>>>- API 테스트시 사용될 요청 파라미터 설정
>>>- 값으로 String만 허용하여 숫자/날짜 등을 등록할 때 문자열로 변경해야 함
>>- jsonPath
>>>- JSON 응답값을 필드별로 검증할 수 있는 메서드
>>>- $ 를 기준으로 필드명을 명시

<br>

[목차로 이동](#목차)

---

## JPA로 데이터베이스 다루기

>### JPA 소개
>- 패러다임 불일치 문제
>>- 관계형 데이터베이스는 어떻게 데이터를 저장할지에 초점이 맞춰진 기술
>>- 객체지향 프로그래밍 언어는 메시지를 기반으로 기능과 속성을 한 곳에서 관리하는 기술
>>- JPA는 서로 지향하는 바가 다른 2개 영역 (객체지향 프로그래밍 언어와 관계형 데이터베이스) 의 중간에서 패러다임 일치를 위한 기술
>>>- 개발자는 객체지향적으로 프로그래밍을 하고, JPA가 이를 관계형 데이터베이스에 맞게 SQL을 대신 생성하여 실행하여 개발자가 더이상 SQL에 종속적인 개발을 하지 않을 수 있음
>- Spring Data JPA
>>- 개요
>>>- JPA는 인터페이스로서 자바 표준명세서
>>>- 인터페이스인 JPA를 사용하기 위해서는 구현체가 필요함
>>>>- 대표적으로 Hibernate, Eclipse Link 등이 있음
>>>- Spring에서 JPA를 사용할 때 구현체들을 직접 다루지 않고 Spring Data JPA 모듈을 이용하여 다룸
>>>>- JPA <- Hibernate <- Spring Data JPA
>>>- Hibernate 와 Spring Data JPA 사이에는 큰 차이가 없지만 사용하는 이유
>>>>1. 구현체 교체의 용이성
>>>>>- 추후에 Hibernate 외에 다른 구현체로 쉽게 교체하기 위함
>>>>2. 저장소 교체의 용이성
>>>>>- RDB 외에 다른 저장소로 쉽게 교체하기 위함

<br>

[목차로 이동](#목차)

>### 프로젝트에 Spring Data JPA 적용하기
>- build.gradle
>>```java
>>dependencies {
>>    implementation('org.springframework.boot:spring-boot-starter-web')
>>    implementation('org.projectlombok:lombok')
>>    implementation('org.springframework.boot:spring-boot-starter-data-jpa')
>>    implementation('com.h2database:h2')
>>    testImplementation('org.springframework.boot:spring-boot-starter-test')
>>}
>>```
>>- h2
>>>- 인메모리 관계형 데이터베이스로서 별도의 설치가 필요 없이 프로젝트 의존성만으로 관리할 수 있음
>>>- 메모리에서 실행되기 때문에 애플리케이션을 재시작할 때마다 초기화된다는 점을 이용하여 테스트 용도로 많이 사용됨
>- domain
>>- 게시글, 댓글, 회원, 정산, 결제 등 소프트웨어에 대한 요구사항 혹은 문제영역
>>- MyBatis 와 같은 쿼리 매퍼에서의 dao 같은 역할
>- Posts
>>```java
>>package org.example.springboot.domain.posts;
>>
>>@Getter
>>@NoArgsConstructor
>>@Entity
>>public class Posts {
>>
>>    @Id
>>    @GeneratedValue(strategy = GenerationType.IDENTITY)
>>    private Long id;
>>
>>    @Column(length = 500, nullable = false)
>>    private String title;
>>
>>    @Column(columnDefinition = "TEXT", nullable = false)
>>    private String content;
>>
>>    private String author;
>>
>>    @Builder
>>    public Posts(String title, String content, String author) {
>>        this.title = title;
>>        this.content = content;
>>        this.author = author;
>>    }
>>    
>>}
>>
>>```
>>- 실제 DB의 테이블과 매칭될 클래스이며 보통 Entity 클래스라고 부름
>>- JPA 에선 DB 데이터에 작업할 경우 실제 쿼리 사용 대신 Entity 클래스의 수정을 통해 작업함
>>- @Entity
>>>- 테이블과 링크될 클래스임을 나타냄
>>>- 기본값으로 클래스의 카멜케이스 이름을 언더스코어 네이밍으로 테이블 이름을 매칭함
>>>>- SalesManager.java -> sales_manager table
>>- @id
>>>- 해당 테이블의 PK 필드를 나타냄
>>- @GeneratedValue
>>>- PK의 생성 규칙을 나타내며 GenerationType.IDNTITY 옵션은 auto_increment를 의미함
>>- @Column
>>>- 테이클의 칼럼을 나타내지만 굳이 선언하지 않아도 해당 클래스의 필드는 모두 칼럼이 됨
>>>- 기본값 외에 추가로 변경이 필요한 옵션이 있을경우 사용함
>>- Entity 클래스에서는 절대 Setter 메서드를 만들지 않고, 대신 해당 필드의 값 변경이 필요하면 목적과 의도를 명확히 나타낼 수 있는 메서드를 추가함
>- PostsRepository
>>```java
>>package org.example.springboot.domain.posts;
>>
>>public interface PostsRepository extends JpaRepository<Posts, Long> {
>>}
>>```
>>- MyBatis 의 Dao 같은 DB Layer 접근자
>>- JPA에선 Repository로 부르며 인터페이스로 생성 후 JpaRepository<Entity 클래스, PK 타입> 를 상속하면 기본 적인 CRUD 메서드가 자동으로 생성됨
>>- @Repository 를 추가할 필요도 없음
>>- 주의사항으로 Entity 클래스와 기본 Entity Repository는 함께 위치해야 함

<br>

[목차로 이동](#목차)

>### Spring Data JPA 테스트 코드 작성하기
>- PostsRepositoryTest
>>```java
>>package org.example.springboot.domain.posts;
>>
>>import static org.assertj.core.api.Assertions.assertThat;
>>
>>@RunWith(SpringRunner.class)
>>@SpringBootTest
>>public class PostsRepositoryTest {
>>
>>    @Autowired
>>    PostsRepository postsRepository;
>>
>>    @After
>>    public void cleanup() {
>>        postsRepository.deleteAll();
>>    }
>>
>>    @Test
>>    public void 게시글저장_불러오기() {
>>        //given
>>        String title = "테스트 게시글";
>>        String content = "테스트 본문";
>>
>>        postsRepository.save(Posts.builder()
>>                .title(title)
>>                .content(content)
>>                .author("wjd30142@naver.com")
>>                .build());
>>
>>        //when
>>        List<Posts> postsList = postsRepository.findAll();
>>
>>        //then
>>        Posts posts = postsList.get(0);
>>        assertThat(posts.getTitle()).isEqualTo(title);
>>        assertThat(posts.getContent()).isEqualTo(content);
>>    }
>>
>>}
>>```
>>- @After
>>>- JUnit 에서 단위 테스트가 끝날 때마다 수행되는 메서드로 지정
>>>- 일반적으로 배포 전 전체 테스트를 수행할 때 테스트간 데이터 침범을 막기 위해 사용됨
>>- postsRepository.save
>>>- 테이블 posts 에 insert / update 쿼리를 실행함
>>>- id 값이 있다면 update, 없다면 insert 쿼리가 실행됨
>>- postsRepository.findAll
>>>- 테이블 posts 에 있는 모든 데이터를 조회하는 메서드
>>- 별다른 설정 없이 @SpringBootTest 를 사용할 경우 H2 DB를 자동으로 실행함
>- 실제로 실행된 쿼리 형태 확인하기
>>- Java 클래스로 구현할 수 있으나 스프링 부트에서는 application.properties 또는 application.yml 등의 파일에 간단하게 설정할 수 있음
>>- src / main / resources 디렉터리에 application.properties 파일 생성
>>>```
>>>spring.jpa.show_sql=true
>>>```
>>- 테스트 실행시 콘솔에서 생성되는 쿼리문 확인 가능
>>- 기본적으로 출력되는 쿼리 로그는 H2 의 쿼리 문법이 적용되어 있으나 MySQL 버전으로 변경할 수 있음
>>>```
>>>spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
>>>```

<br>

[목차로 이동](#목차)

>### 등록 / 수정 / 조회 API 만들기
>- API를 만들기 위한 3개의 클래스
>>- Request 데이터를 받을 Dto
>>- API 요청을 받을 Controller
>>- 트랜잭션, 도메인 기능 간의 순서를 보장하는 Service
>- Service 에 대한 오해
>>- Service 에서 비즈니스 로직을 처리해야 한다고 오해가 있음
>>- Service는 트랜잭션, 도메인 간 순서 보장의 역할만 함
>- Spring 웹 계층
>>1. Web Layer
>>>- 컨트롤러, 뷰 템플릿 영역, 필터, 인터셉터, 컨트롤러 어드바이스 등 외부 요청과 응답에 대한 전반적인 영역
>>2. Service Layer
>>>- @Service 에 사용되는 서비스 영역으로 Controller 와 Dao 의 중간 영역에서 사용됨
>>>- @Transactional 이 사용되어야 하는 영역
>>3. Repository Layer
>>>- 데이터 저장소에 접근하는 영역으로 Dao 와 비슷한 역할
>>4. Dtos
>>>- 계층 간에 데이터 교환을 위한 객체들의 영역
>>5. Domain Model
>>>- 도메인이라 불리는 개발 대상을 모든 사람이 동일한 관점에서 이해할 수 있고 공유할 수 있도록 단순화시킨 것을 도메인 모델이라함
>>>- 택시 앱의 경우 배차, 탑승, 요금 등이 모두 도메인이 될 수 있음
>>>- @Entity 가 사용된 영역 역시 도메인 모델
>>>- VO 와 같은 값 객체들도 이 영역에 해당하기 때문에 무조건 DB의 테이블과 관계가 있어야만 하는 것은아님
>- 비지니스 처리를 담당해야 할 곳은 Domain 임
>>- 기존 서비스에서 처리하던 방식을 트랜잭션 스크립트라고 하며 모든 로직이 서비스 클래스 내부에서 처리되므로 서비스 계층이 무의미하며, 객체란 단순히 데이터 덩어리 역할만 하게 됨
>>- 로직을 도메인 모델에서 처리할 경우 객체들이 각자 본인의 이벤트 처리를 하며, 서비스 메서드는 트랜잭션과 도메인 간의 순서만 보장함
>- PostsApiController
>>```java
>>package org.example.springboot.web;
>>
>>@RequiredArgsConstructor
>>@RestController
>>public class PostsApiController {
>>    private final PostsService postsService;
>>
>>    @PostMapping("/api/v1/posts")
>>    public Long save(@RequestBody PostsSaveRequestDto requestDto) {
>>        return postsService.save(requestDto);
>>    }
>>    
>>    @PutMapping("/api/v1/posts/{id}")
>>    public Long update(@PathVariable Long id, @RequestBody PostsUpdateRequestDto requestDto) {
>>        return postsService.update(id, requestDto);
>>    }
>>
>>    @GetMapping("/api/v1/posts/{id}")
>>    public PostsResponseDto findById(@PathVariable Long id) {
>>        return postsService.findById(id);
>>    }
>>}
>>```
>- PostsService
>>```java
>>package org.example.springboot.PostsService;
>>
>>@RequiredArgsConstructor
>>@Service
>>public class PostsService {
>>    private final PostsRepository postsRepository;
>>
>>    @Transactional
>>    public Long save(PostsSaveRequestDto requestDto) {
>>        return postsRepository.save(requestDto.toEntity()).getId();
>>    }
>>
>>    @Transactional
>>    public Long update(Long id, PostsUpdateRequestDto requestDto) {
>>        Posts posts = postsRepository.findById(id)
>>                .orElseThrow(() -> new IllegalArgumentException("해당 게시글이 없습니다. id=" + id));
>>
>>        posts.update(requestDto.getTitle(), requestDto.getContent());
>>
>>        return id;
>>    }
>>
>>    public PostsResponseDto findById (Long id) {
>>        Posts entity = postsRepository.findById(id)
>>                .orElseThrow(() -> new IllegalArgumentException("해당 게시글이 없습니다. id=" + id));
>>
>>        return new PostsResponseDto(entity);
>>    }
>>}
>>```
>>- JPA의 영속성 컨텍스트 덕분에 update 기능에서 DB에 쿼리를 날릴 필요가 없음
>>- 영속성 컨텍스트
>>>- 엔티티를 영구 저장하는 환경
>>>- JPA 의 엔티티 매니저가 활성화된 상태(Spring Data Jpa를 쓴다면 기본 옵션)로 트랙잭션 안에서 DB에서 데이터를 가져오면 이 데이터는 영속성 컨텍스트가 유지된 상태
>>>- 영속성 컨텍스트가 유지된 상태에서 해당 데이터의 값을 변경하면 트랙잭션이 끝나는 시점에 해당 테이블에 변경분을 반영함 (더티 체킹)
>- PostsSaveRequestDto
>>```java
>>package org.example.springboot.web.dto;
>>
>>@Getter
>>@NoArgsConstructor
>>public class PostsSaveRequestDto {
>>    private String title;
>>    private String content;
>>    private String author;
>>
>>    @Builder
>>    public PostsSaveRequestDto(String title, String content, String author) {
>>        this.title = title;
>>        this.content = content;
>>        this.author = author;
>>    }
>>
>>    public Posts toEntity() {
>>        return Posts.builder()
>>                .title(title)
>>                .content(content)
>>                .author(author)
>>                .build();
>>    }
>>}
>>```
>>- Entity 클래스는 DB와 맞닿은 핵심클래스이므로 거의 유사항 형태이지만 Dto 클래스를 추가로 생성함
>>>- Entity 클래스를 기준으로 테이블이 생성되고, 스키마가 변경되므로 Entity 클래스를 Request/Response 클래스로 사용해서는 안됨
>- PostsResponseDto
>>```java
>>package org.example.springboot.web.dto;
>>
>>@Getter
>>public class PostsResponseDto {
>>
>>    private Long id;
>>    private String title;
>>    private String content;
>>    private String author;
>>
>>    public PostsResponseDto(Posts entity) {
>>        this.id = entity.getId();
>>        this.title = entity.getTitle();
>>        this.content = entity.getContent();
>>        this.author = entity.getAuthor();
>>    }
>>}
>>```
>- PostsUpdateRequestDto
>>```java
>>package org.example.springboot.web.dto;
>>
>>@Getter
>>@NoArgsConstructor
>>public class PostsUpdateRequestDto {
>>
>>    private String title;
>>    private String content;
>>
>>    @Builder
>>    public PostsUpdateRequestDto(String title, String content) {
>>        this.title = title;
>>        this.content = content;
>>    }
>>}
>>```
>- PostsApiControllerTest
>>```java
>>package org.example.springboot.web;
>>
>>import static org.assertj.core.api.Assertions.assertThat;
>>
>>@RunWith(SpringRunner.class)
>>@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
>>public class PostsApiControllerTest {
>>    @LocalServerPort
>>    private int port;
>>
>>    @Autowired
>>    private TestRestTemplate restTemplate;
>>
>>    @Autowired
>>    private PostsRepository postsRepository;
>>
>>    @After
>>    public void tearDown() throws Exception {
>>        postsRepository.deleteAll();
>>    }
>>
>>    @Test
>>    public void Posts_등록된다() throws Exception {
>>        // given
>>        String title = "title";
>>        String content = "content";
>>        PostsSaveRequestDto requestDto = PostsSaveRequestDto.builder()
>>                .title(title)
>>                .content(content)
>>                .author("author")
>>                .build();
>>
>>        String url = "http://localhost:" + port + "/api/v1/posts";
>>
>>        // when
>>        ResponseEntity<Long> responseEntity = restTemplate.postForEntity(url, requestDto, Long.class);
>>
>>        // then
>>        assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
>>        assertThat(responseEntity.getBody()).isGreaterThan(0L);
>>
>>        List<Posts> all = postsRepository.findAll();
>>        assertThat(all.get(0).getTitle()).isEqualTo(title);
>>        assertThat(all.get(0).getContent()).isEqualTo(content);
>>    }
>>
>>    @Test
>>    public void Posts_수정된다() throws Exception {
>>        // given
>>        Posts savedPosts = postsRepository.save(Posts.builder()
>>                .title("title")
>>                .content("content")
>>                .author("author")
>>                .build());
>>
>>        Long updateId = savedPosts.getId();
>>        String expectedTitle = "title2";
>>        String expectedContent = "content2";
>>
>>        PostsUpdateRequestDto requestDto = PostsUpdateRequestDto.builder()
>>                .title(expectedTitle)
>>                .content(expectedContent)
>>                .build();
>>
>>        String url = "http://localhost:" + port + "/api/v1/posts/" + updateId;
>>
>>        HttpEntity<PostsUpdateRequestDto> requestEntity = new HttpEntity<>((requestDto));
>>
>>        // when
>>        ResponseEntity<Long> responseEntity = restTemplate.exchange(url, HttpMethod.PUT, requestEntity, Long.class);
>>
>>        // then
>>        assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
>>        assertThat(responseEntity.getBody()).isGreaterThan(0L);
>>
>>        List<Posts> all = postsRepository.findAll();
>>        assertThat(all.get(0).getTitle()).isEqualTo(expectedTitle);
>>        assertThat(all.get(0).getContent()).isEqualTo(expectedContent);
>>    }
>>}
>>```
>>- @WebMvcTest 는 JPA 기능이 작동하지 않기 때문에 HelloControllerTest 와 달리 사용하지 않음
>>- JPA 기능까지 한번에 테스트 할 때는 @SpringBootTest 와 TestRestTemplate 을 사용함
>- 조회 웹 콘솔 테스트
>>- application.properties 설정
>>>```
>>>spring.h2.console.enabled=true
>>>```
>>- Application 클래스의 main 메서드 실행
>>- http://localhost:8080/h2-console 로 접속
>>- JDBC URL 을 jdbc:h2:mem:testdb 로 변경 후 Connect 버튼 클릭
>>- 쿼리 실행하며 테스트

<br>

[목차로 이동](#목차)

>### JPA Auditing 으로 생성시간 / 수정시간 자동화하기
>- LocalDate 사용
>>- Java8 부터 Java의 기본 날짜 타입인 Date의 문제점을 제대로 고친 LocalDate 와 LocalDateTime 등장
>>>- Date 는 불변 객체가 아니며 Calendar 의 Month 값 중 OCTOBER 의 값이 9로 설정되어 있는 오류가 있음
>- BaseTimeEntity
>>```java
>>package org.example.springboot.domain;
>>
>>@Getter
>>@MappedSuperclass
>>@EntityListeners(AuditingEntityListener.class)
>>public abstract class BaseTimeEntity {
>>    @CreatedDate
>>    private LocalDateTime createDate;
>>
>>    @LastModifiedDate
>>    private LocalDateTime modifiedDate;
>>}
>>```
>>- @MappedSuperclass
>>>- JPA Entity 클래스들이 BaseTimeEntity 를 상속할 경우 필드들도 칼럼으로 인식하도록 함
>>- @EntityListeners(AuditingEntityListener.class)
>>>- BaseTimeEntity 클레스에 Auditing 기능을 포함시킴
>>- @CreatedDate
>>>- Entity 가 생성되어 저장될 때 시간이 자동 저장됨
>>- @LastModifiedDate
>>>- 조회한 Entity 의 값을 변경할 때 시간이 자동 저장됨
>- Posts 클래스가 BaseTimeEntity를 상속받도록 변경
>- JPA Auditing 어노테이션들을 모두 활성화할 수 있도록 Application 클래스ㅔㅇ 활성화 어노테이션 추가
>>```java
>>@EnableJpaAuditing
>>@SpringBootApplication
>>public class Application {
>>    public static void main(String[] args) {
>>        SpringApplication.run(Application.class, args);
>>    }
>>}
>>```
>- PostsRepositoryTest 클레스에 테스트 메서드 추가
>>```java
>>@Test
>>public void BaseTimeEntity_등록() {
>>    // given
>>    LocalDateTime now = LocalDateTime.of(2019,6,4,0,0,0);
>>    postsRepository.save(Posts.builder()
>>            .title("title")
>>            .content("content")
>>            .author("author")
>>            .build());
>>
>>    // when
>>    List<Posts> postsList = postsRepository.findAll();
>>
>>    // then
>>    Posts posts = postsList.get(0);
>>
>>    System.out.println(">>>>>>>>> createDate=" + posts.getCreateDate() + ", modifiedDate=" + posts.getModifiedDate());
>>
>>    assertThat(posts.getCreateDate()).isAfter(now);
>>    assertThat(posts.getModifiedDate()).isAfter(now);
>>}
>>```

<br>

[목차로 이동](#목차)

---

## 머스테치로 화면 구성하기

>### 서버 템플릿 엔진과 머스테치 소개
>- 템플릿 엔진
>>- 웹 개발에 있어 템플릿 엔진이란, 지정된 템플릿 양식과 데이터가 합쳐져 HTML 문서를 출력하는 소프트웨어를 뜻함
>>>- JSP, Freemaker 는 서버 템플릿 엔진
>>>- React, Vue 는 클라이언트 템플릿 엔진
>>>>- V8 엔진 라이브러리를 이용하여 서버 사이드 렌더링이 지원되나 난이도가 높음
>- 머스테치
>>- 수많은 언어를 지원하는 가장 심플한 템플릿 엔진
>>- 자바 진영의 서버 템플릿 엔진
>>>- JSP, Velocity
>>>>- 스프링 부트에서 권장하지 않는 템플릿 엔진
>>>- Freemaker
>>>>- 템플릿 엔진으로는 과하게 많은 기능 지원
>>>>- 높은 자유도로 인해 숙련도가 낮을수록 비즈니스 로직이 추가될 확률이 높음
>>>- Thymeleaf
>>>>- 스프링 진영에서 적극적으로 지원하지만 문법이 어려움
>>>>- 태그 속성 방식으로 템플릿 기능을 사용하는 방식, Vue.js 와 비슷함
>>- 머스테치 장점
>>>- 문법이 다른 템플릿 엔진보다 심플
>>>- 로직 코드를 사용할 수 없어 View와 서버의 역할이 명확하게 분리됨
>>>- Mustache.js 와 Mustache.java 두 가지가 있어서 하나의 문법으로 클라이언트/서버 템플릿을 모두 사용할 수 있음
>- 머스테치 플러그인 설치
>>- 인텔리제이 커뮤니티 버전에서도 사용할 수 있음
>>- 플러그인 사용시 머스테치의 문법 체크, HTML 문법 지원, 자동완성 등이 지원됨

<br>

[목차로 이동](#목차)

>### 기본 페이지 만들기
>>- build.gradle 에 의존성 추가
>>>- implementation('org.springframework.boot:spring-boot-starter-mustache')
>>- src/main/resources/templates/index.mustache
>>>```html
>>><!DOCTYPE HTML>
>>><html>
>>><head>
>>>    <title>스프링부트 웹서비스</title>
>>>    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
>>></head>
>>><body>
>>>    <h1>스프링 부트로 시작하는 웹 서비스</h1>
>>></body>
>>></html>
>>>```
>>- IndexController
>>>```java
>>>package org.example.springboot.web;
>>>
>>>@Controller
>>>public class IndexController {
>>>    @GetMapping("/")
>>>    public String index() {
>>>        return "index";
>>>    }
>>>}
>>>```
>>>- 머스테치 스타터 의존성 추가로 인해 컨트롤러에서 문자열을 반환할 때 ViewResulver 가 처리함
>>>>- src/main/resources/templates/index.mustache
>>- IndexControllerTest
>>>```java
>>>package org.example.springboot.web;
>>>
>>>import static org.assertj.core.api.Assertions.assertThat;
>>>import static org.springframework.boot.test.context.SpringBootTest.WebEnvironment.RANDOM_PORT;
>>>
>>>@RunWith(SpringRunner.class)
>>>@SpringBootTest(webEnvironment = RANDOM_PORT)
>>>public class IndexControllerTest {
>>>    @Autowired
>>>    private TestRestTemplate restTemplate;
>>>
>>>    @Test
>>>    public void 메인페이지_로딩() {
>>>        // when
>>>        String body = this.restTemplate.getForObject("/", String.class);
>>>
>>>        // then
>>>        assertThat(body).contains("스프링 부트로 시작하는 웹 서비스");
>>>    }
>>>}
>>>```

<br>

[목차로 이동](#목차)

>### 게시글 등록 화면 만들기
>>- 머스테치의 레이아웃 방식
>>>```html
>>><!-- header.mustache -->
>>><!DOCTYPE HTML>
>>><html>
>>><head>
>>>    <title>스프링부트 웹서비스</title>
>>>    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
>>>
>>>    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
>>></head>
>>><body>
>>>
>>><!-- index.mustache -->
>>>{{>layout/header}}
>>>
>>><h1>스프링부트로 시작하는 웹 서비스 Ver.2</h1>
>>><div class="col-md-12">
>>>    <div class="row">
>>>        <div class="col-md-6">
>>>            <a href="/posts/save" role="button" class="btn btn-primary">글 등록</a>
>>>        </div>
>>>    </div>
>>></div>
>>>
>>>{{>layout/footer}}
>>>
>>><!-- footer.mustache -->
>>><script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
>>><script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>
>>>
>>><!--index.js 추가-->
>>><script src="/js/app/index.js"></script>
>>></body>
>>></html>
>>>```
>>>- 스프링 부트는 기본적으로 src/main/resources/static 에 위치한 js, css, image 등 정적 파일은 URL 에서 / 로 설정됨
>>- IndexController
>>>```java
>>>package org.example.springboot.web;
>>>
>>>@Controller
>>>public class IndexController {
>>>    // ...
>>>
>>>    @GetMapping("/posts/svae")
>>>    public String postsSave() {
>>>        return "posts-save";
>>>    }
>>>}
>>>```
>>- posts-save.mustache
>>>```html
>>>{{>layout/header}}
>>>
>>><h1>게시글 등록</h1>
>>>
>>><div class="col-md-12">
>>>    <div class="col-md-4">
>>>        <form>
>>>            <div class="form-group">
>>>                <label for="title">제목</label>
>>>                <input type="text" class="form-control" id="title" placeholder="제목을 입력하세요">
>>>            </div>
>>>            <div class="form-group">
>>>                <label for="author"> 작성자 </label>
>>>                <input type="text" class="form-control" id="author" placeholder="작성자를 입력하세요">
>>>            </div>
>>>            <div class="form-group">
>>>                <label for="content"> 내용 </label>
>>>                <textarea class="form-control" id="content" placeholder="내용을 입력하세요"></textarea>
>>>            </div>
>>>        </form>
>>>        <a href="/" role="button" class="btn btn-secondary">취소</a>
>>>        <button type="button" class="btn btn-primary" id="btn-save">등록</button>
>>>    </div>
>>></div>
>>>
>>>{{>layout/footer}}
>>>```
>>- src/main/resources/static/js/app/index.js
>>>```js
>>>var main = {
>>>    init : function () {
>>>        var _this = this;
>>>        $('#btn-save').on('click', function () {
>>>            _this.save();
>>>        });
>>>    },
>>>    save : function () {
>>>        var data = {
>>>            title: $('#title').val(),
>>>            author: $('#author').val(),
>>>            content: $('#content').val()
>>>        };
>>>
>>>        $.ajax({
>>>            type: 'POST',
>>>            url: '/api/v1/posts',
>>>            dataType: 'json',
>>>            contentType:'application/json; charset=utf-8',
>>>            data: JSON.stringify(data)
>>>        }).done(function() {
>>>            alert('글이 등록되었습니다.');
>>>            window.location.href = '/';
>>>        }).fail(function (error) {
>>>            alert(JSON.stringify(error));
>>>        });
>>>    },
>>>};
>>>main.init();
>>>```
>>>- 중복된 함수 이름은 자주 발생할 수 있기 때문에 var main 이란 객체를 만들어 index.js 만의 유효범위를 만들어 사용함

<br>

[목차로 이동](#목차)

>### 전체 조회 화면 만들기
>>- index.mustache 구조 변경
>>>```html
>>>{{>layout/header}}
>>>
>>><h1>스프링부트로 시작하는 웹 서비스 Ver.2</h1>
>>><div class="col-md-12">
>>>    <div class="row">
>>>        <div class="col-md-6">
>>>            <a href="/posts/save" role="button" class="btn btn-primary">글 등록</a>
>>>        </div>
>>>    </div>
>>></div>
>>><div class="col-md-12">
>>>    <div class="row">
>>>        <div class="col-md-6">
>>>            <a href="/posts/save" role="button" class="btn btn-primary">글 등록</a>
>>>        </div>
>>>    </div>
>>>    <br>
>>>    <!-- 목록 출력 영역 -->
>>>    <table class="table table-horizontal table-bordered">
>>>        <thead class="thead-strong">
>>>        <tr>
>>>            <th>게시글번호</th>
>>>            <th>제목</th>
>>>            <th>작성자</th>
>>>            <th>최종수정일</th>
>>>        </tr>
>>>        </thead>
>>>        <tbody id="tbody">
>>>        {{#posts}}
>>>            <tr>
>>>                <td>{{id}}</td>
>>>                <td><a href="/posts/update/{{id}}">{{title}}</a></td>
>>>                <td>{{author}}</td>
>>>                <td>{{modifiedDate}}</td>
>>>            </tr>
>>>        {{/posts}}
>>>        </tbody>
>>>    </table>
>>></div>
>>>
>>>{{>layout/footer}}
>>>```
>>>- {{#posts}}
>>>>- posts 라는 List를 순회
>>- PostsRepository 인터페이스에 쿼리 추가
>>>```java
>>>package org.example.springboot.domain.posts;
>>>
>>>public interface PostsRepository extends JpaRepository<Posts, Long> {
>>>    @Query("SELECT p FROM Posts p ORDER BY p.id DESC")
>>>    List<Posts> findAllDesc();
>>>}
>>>```
>>>- 규모 있는 프로젝트에서 데이터 조회는 Entity 클래스만으로 처리하기 어려워 조회용 프레임워크를 추가로 사용함
>>>>- 대표적으로 Querydsl, jooq, MyBatis 등이 있으나 Querydsl이 많이 사용됨
>>- PostsService 코드 추가
>>>```java
>>>package org.example.springboot.PostsService;
>>>
>>>@RequiredArgsConstructor
>>>@Service
>>>public class PostsService {
>>>    private final PostsRepository postsRepository;
>>>
>>>    //...
>>>
>>>    @Transactional(readOnly = true)
>>>    public List<PostsListResponseDto> findAllDesc() {
>>>        return postsRepository.findAllDesc().stream()
>>>                .map(PostsListResponseDto::new)
>>>                .collect(Collectors.toList());
>>>    }
>>>}
>>>```
>>>- .map(PostsListResponseDto::new)
>>>>- .map(posts -> new PostsListResponsesDto(posts)) 와 같음
>>>>- postsRepository 결과로 넘어온 Posts의 Stream을 map을 통해 PostsListResponseDto 변환 -> List로 반환하는 메서드
>>- PostsListResponseDto
>>>```java
>>>package org.example.springboot.web.dto;
>>>
>>>@Getter
>>>public class PostsListResponseDto {
>>>    private Long id;
>>>    private String title;
>>>    private String author;
>>>    private LocalDateTime modifiedDate;
>>>
>>>    public PostsListResponseDto(Posts entity) {
>>>        this.id = entity.getId();
>>>        this.title = entity.getTitle();
>>>        this.author = entity.getAuthor();
>>>        this.modifiedDate = entity.getModifiedDate();
>>>    }
>>>}
>>>```
>>- IndexController 수정
>>>```java
>>>package org.example.springboot.web;
>>>
>>>@RequiredArgsConstructor
>>>@Controller
>>>public class IndexController {
>>>    private final PostsService postsService;
>>>
>>>    @GetMapping("/")
>>>    public String index(Model model) {
>>>        model.addAttribute("posts", postsService.findAllDesc());
>>>        return "index";
>>>    }
>>>
>>>    //...
>>>}
>>>```
>>>- Model
>>>>- 서버 템플릿 엔진에서 사용할 수 있는 객체를 저장

<br>

[목차로 이동](#목차)

>### 게시글 수정, 삭제 화면 만들기
>>- PostsApiController
>>>```java
>>>package org.example.springboot.web;
>>>
>>>@RequiredArgsConstructor
>>>@RestController
>>>public class PostsApiController {
>>>    private final PostsService postsService;
>>>
>>>    // ...
>>>
>>>    @PutMapping("/api/v1/posts/{id}")
>>>    public Long update(@PathVariable Long id, @RequestBody PostsUpdateRequestDto requestDto) {
>>>        return postsService.update(id, requestDto);
>>>    }
>>>
>>>    // ...
>>>
>>>    @DeleteMapping("/api/v1/posts/{id}")
>>>    public Long delete(@PathVariable Long id) {
>>>        postsService.delete(id);
>>>        return id;
>>>    }
>>>}
>>>```
>>- posts-update.mustache
>>>```java
>>>{{>layout/header}}
>>>
>>><h1>게시글 수정</h1>
>>>
>>><div class="col-md-12">
>>>    <div class="col-md-4">
>>>        <form>
>>>            <div class="form-group">
>>>                <label for="title">글 번호</label>
>>>                <input type="text" class="form-control" id="id" value="{{post.id}}" readonly>
>>>            </div>
>>>            <div class="form-group">
>>>                <label for="title">제목</label>
>>>                <input type="text" class="form-control" id="title" value="{{post.title}}">
>>>            </div>
>>>            <div class="form-group">
>>>                <label for="author"> 작성자 </label>
>>>                <input type="text" class="form-control" id="author" value="{{post.author}}" readonly>
>>>            </div>
>>>            <div class="form-group">
>>>                <label for="content"> 내용 </label>
>>>                <textarea class="form-control" id="content">{{post.content}}</textarea>
>>>            </div>
>>>        </form>
>>>        <a href="/" role="button" class="btn btn-secondary">취소</a>
>>>        <button type="button" class="btn btn-primary" id="btn-update">수정 완료</button>
>>>        <button type="button" class="btn btn-danger" id="btn-delete">삭제</button>
>>>    </div>
>>></div>
>>>
>>>{{>layout/footer}}
>>>```
>>- index.js
>>>```js
>>>var main = {
>>>    init : function () {
>>>        var _this = this;
>>>        // ...
>>>
>>>        $('#btn-update').on('click', function () {
>>>            _this.update();
>>>        });
>>>
>>>        $('#btn-delete').on('click', function () {
>>>            _this.delete();
>>>        });
>>>    },
>>>    save : function () {
>>>        // ...
>>>    },
>>>    update : function () {
>>>        var data = {
>>>            title: $('#title').val(),
>>>            content: $('#content').val()
>>>        };
>>>
>>>        var id = $('#id').val();
>>>
>>>        $.ajax({
>>>            type: 'PUT',
>>>            url: '/api/v1/posts/'+id,
>>>            dataType: 'json',
>>>            contentType:'application/json; charset=utf-8',
>>>            data: JSON.stringify(data)
>>>        }).done(function() {
>>>            alert('글이 수정되었습니다.');
>>>            window.location.href = '/';
>>>        }).fail(function (error) {
>>>            alert(JSON.stringify(error));
>>>        });
>>>    },
>>>    delete : function () {
>>>        var id = $('#id').val();
>>>
>>>        $.ajax({
>>>            type: 'DELETE',
>>>            url: '/api/v1/posts/'+id,
>>>            dataType: 'json',
>>>            contentType:'application/json; charset=utf-8'
>>>        }).done(function() {
>>>            alert('글이 삭제되었습니다.');
>>>            window.location.href = '/';
>>>        }).fail(function (error) {
>>>            alert(JSON.stringify(error));
>>>        });
>>>    }
>>>};
>>>main.init();
>>>```
>>- index.mustache
>>>```html
>>>{{>layout/header}}
>>>
>>><h1>스프링부트로 시작하는 웹 서비스 Ver.2</h1>
>>><div class="col-md-12">
>>>    <div class="row">
>>>        <div class="col-md-6">
>>>            <a href="/posts/save" role="button" class="btn btn-primary">글 등록</a>
>>>        </div>
>>>    </div>
>>>    <br>
>>>    <!-- 목록 출력 영역 -->
>>>    <table class="table table-horizontal table-bordered">
>>>        <thead class="thead-strong">
>>>        <tr>
>>>            <th>게시글번호</th>
>>>            <th>제목</th>
>>>            <th>작성자</th>
>>>            <th>최종수정일</th>
>>>        </tr>
>>>        </thead>
>>>        <tbody id="tbody">
>>>        {{#posts}}
>>>            <tr>
>>>                <td>{{id}}</td>
>>>                <td><a href="/posts/update/{{id}}">{{title}}</a></td>
>>>                <td>{{author}}</td>
>>>                <td>{{modifiedDate}}</td>
>>>            </tr>
>>>        {{/posts}}
>>>        </tbody>
>>>    </table>
>>></div>
>>>
>>>{{>layout/footer}}
>>>```
>>- IndexController
>>>```java
>>>package org.example.springboot.web;
>>>
>>>@RequiredArgsConstructor
>>>@Controller
>>>public class IndexController {
>>>    private final PostsService postsService;
>>>
>>>    // ...
>>>
>>>    @GetMapping("/posts/update/{id}")
>>>    public String postsUpdate(@PathVariable Long id, Model model) {
>>>        PostsResponseDto dto = postsService.findById(id);
>>>        model.addAttribute("post", dto);
>>>
>>>        return "posts-update";
>>>    }
>>>}
>>>```
>>- PostsService
>>>```java
>>>package org.example.springboot.PostsService;
>>>
>>>@RequiredArgsConstructor
>>>@Service
>>>public class PostsService {
>>>    private final PostsRepository postsRepository;
>>>
>>>    // ...
>>>
>>>    @Transactional
>>>    public Long update(Long id, PostsUpdateRequestDto requestDto) {
>>>        Posts posts = postsRepository.findById(id)
>>>                .orElseThrow(() -> new IllegalArgumentException("해당 게시글이 없습니다. id=" + id));
>>>
>>>        posts.update(requestDto.getTitle(), requestDto.getContent());
>>>
>>>        return id;
>>>    }
>>>
>>>    // ...
>>>
>>>    @Transactional
>>>    public void delete(Long id) {
>>>        Posts posts = postsRepository.findById(id)
>>>                .orElseThrow(() -> new IllegalArgumentException("해당 게시글이 없습니다. id=" + id));
>>>
>>>        postsRepository.delete(posts);
>>>    }
>>>}
>>>```

<br>

[목차로 이동](#목차)

---

## 스프링 시큐리티와 OAuth 2.0 으로 로그인 기능 구현하기

>### 스프링 시큐리티와 스프링 시큐리티 Oauth2 클라이언트
>- 스프링 시큐리티 개요
>>- 막강한 인증 (Authentication) 과 인가 (Authorization, 혹은 권한 부여) 기능을 가진 프레임워크
>>- 사실상 스프링 기반의 애플리케이션에서의 보안을 위한 표준
>>- 인터셉터, 필터 기반의 보안 기능을 구현하는 것보다 스프링 시큐리티를 통해 구현하는 것을 적극적으로 권장
>- 많은 서비스에서 소셜 로그인 기능을 사용하는 이유
>>- 직접 로그인을 구현하면 구현해야하는 것들
>>>- 로그인시 보안
>>>- 회원가입 시 이메일 혹은 전화번호 인증
>>>- 비밀번호 찾기
>>>- 비밀번호 변경
>>>- 회원정보 변경
>>- OAuth 로그인 구현 시 소셜에 맡기면 되므로 서비스 개발에 집중할 수 있음
>- 스프링 부트 1.5 VS 스프링 부트 2.0
>>- 스프링 부트 1.5 에서의 OAuth2 연동 방법이 스프링 부트 2.0에서는 크게 변경되었으나 spring-security-oauth2-autoconfigure 라이브러리덕에 설정 방법에는 크게 차이가 없음
>>- 기존에 안전하게 작동하던 코드인 1.5버전을 사용하는 것이 확실하므로 많이 사용
>>- 신규 기능은 부트 2.0 버전 라이브러리에서만 추가되고 starter 라이브러리가 출시됨
>>- 기존 방식은 확장 포인트가 적절하게 오픈되어 있지 않아 직접 상속하거나 오버라이딩 해야하지만 신규 라이브러리의 경우 확장 포인트를 고려하여 설계되어 있음
>>- 부트 1.5 application.properties에는 url 주소를 모두 명시, 부트 2.0 에는 client 인증 정보만 입력하고 CommonOAuth2Provider라는 enum 을 따로 작성

<br>

[목차로 이동](#목차)

>### 구글 서비스 등록
>- 구글 서비스에서 신규 서비스 생성
>>1. 구글 클라우드 플랫폼 ([링크](https://console.cloud.google.com)) 에서 상단의 [프로젝트 선택] 을 클릭하고 새 프로젝트를 생성
>>2. 생성된 프로젝트를 선택하고 왼쪽 메뉴 탭에서 API 및 서비스 카테고리로 이동
>>3. 사용자 인증 정보를 클릭하고 사용자 인증 정보 만들기 버튼 클릭 후 OAuth 클라이언트 ID 선택
>>4. 동의 화면 구성에서 동의를 요청하는 앱의 이름과 Google API의 범위에서 email, profile, openid 를 선택 후 저장
>>5. OAuth 클라이언트 ID 만들기 화면에서 웹 애플리케이션 유형을 선택하고 승인된 리디렉션 URI 항목에 http://localhost:8080/login/oauth2/code/google 입력 후 생성
>>>- 스프링 부트 2버전의 시큐리티에서는 기본적으로 {도메인}/login/oauth2/code/{소셜서비스코드}로 리다이렉트 URL을 지원하므로 따로 Controller를 만들 필요가 없음
>>6. 생성된 애플리케이션에서 클라이언트 ID와 클라이언트 보안 비밀을 프로젝트에 설정
>- 프로젝트에 내용 추가
>>- src/main/resources/application-oauth.properties 추가 생성
>>>```properties
>>>spring.security.oauth2.client.registration.google.client-id=클라이언트 ID
>>>spring.security.oauth2.client.registration.google.client-secret= 클라이언트 보안 비밀
>>>spring.security.oauth2.client.registration.google.scope=profile,email
>>>```
>>>- scope를 설정하는 이유는 openid 가 포함되어 있으면 OpenId Provider인 서비스와 그렇지 않은 서비스로 나눠서 각각 OAuth2Service를 만들어야하기 때문에 일부러 제외하고 등록
>>>- 해당 내용은 외부에 노출될 경우 개인정보 취약점이 될 수 있으므로 .gitignore 등록을 필수로 해야함
>>- src/main/resources/application.properties 내용 추가
>>>```properties
>>>spring.profiles.include=oauth
>>>```
>>>- 스프링 부트에서는 application-xxx.properties로 만들면 xxx 이름의 profile이 생성되어 이를 통해 관리할 수 있음

<br>

[목차로 이동](#목차)

>### 구글 로그인 연동하기
>- User Entity
>>- src/main/java/.../springboot/domain/user/User.java
>>>```java
>>>@Getter
>>>@NoArgsConstructor
>>>@Entity
>>>public class User extends BaseTimeEntity {
>>>    @Id
>>>    @GeneratedValue(strategy = GenerationType.IDENTITY)
>>>    private Long id;
>>>
>>>    @Column(nullable = false)
>>>    private String name;
>>>
>>>    @Column(nullable = false)
>>>    private String email;
>>>
>>>    @Column
>>>    private String picture;
>>>
>>>    @Enumerated(EnumType.STRING)
>>>    @Column(nullable = false)
>>>    private Role role;
>>>
>>>    @Builder
>>>    public User(String name, String email, String picture, Role role) {
>>>        this.name = name;
>>>        this.email = email;
>>>        this.picture = picture;
>>>        this.role = role;
>>>    }
>>>
>>>    public User update(String name, String picture) {
>>>        this.name = name;
>>>        this.picture = picture;
>>>
>>>        return this;
>>>    }
>>>
>>>    public String getRoleKey() {
>>>        return this.role.getKey();
>>>    }
>>>}
>>>```
>>- src/main/java/.../springboot/domain/user/Role.java
>>>```java
>>>@Getter
>>>@RequiredArgsConstructor
>>>public enum Role {
>>>    GUEST("ROLE_GUEST", "손님"),
>>>    USER("ROLE_USER", "일반 사용자");
>>>
>>>    private final String key;
>>>    private final String title;
>>>}
>>>```
>>>- 스프링 시큐리티에서는 권한 코드에 항상 ROLE_ 이 앞에 있어야함
>>- src/main/java/.../springboot/domain/user/UserRepository.java
>>>```java
>>>public interface UserRepository extends JpaRepository<User, Long> {
>>>    Optional<User> findByEmail(String email);
>>>}
>>>```
>- 스프링 시큐리티 설정
>>- build.gradle 에 의존성 추가
>>>```gradle
>>>implementation('org.springframework.boot:spring-boot-starter-oauth2-client')
>>>```
>>- src/main/java/.../springboot/config/auth 패키지 생성
>>>- 시큐리티 관련 클래스는 모두 이곳에서 관리
>>- src/main/java/.../springboot/config/auth/SecurityConfig.java
>>>```java
>>>@RequiredArgsConstructor
>>>@EnableWebSecurity
>>>public class SecurityConfig extends WebSecurityConfigurerAdapter {
>>>    private final CustomOAuth2UserService customOAuth2UserService;
>>>
>>>    @Override
>>>    protected void configure(HttpSecurity http) throws Exception {
>>>        http
>>>                .csrf().disable()
>>>                .headers().frameOptions().disable()
>>>                .and()
>>>                    .authorizeRequests()
>>>                    .antMatchers("/", "/css/**", "/images/**", "/js/**", "/h2-console/**").permitAll()
>>>                    .antMatchers("/api/v1/**").hasRole(Role.USER.name())
>>>                    .anyRequest().authenticated()
>>>                .and()
>>>                    .logout()
>>>                        .logoutSuccessUrl("/")
>>>                .and()
>>>                    .oauth2Login()
>>>                        .userInfoEndpoint()
>>>                            .userService(customOAuth2UserService);
>>>    }
>>>}
>>>```
>>>- @EnableWebSecurity
>>>>- Spring Security 설정들을 활성화함
>>>- csrf().disable().headers().frameOptions().disable()
>>>>- h2-console 화면을 사용하기 위해 해당 옵션들을 disable
>>>- authorizeRequests
>>>>- URL 별 권한 관리를 설정하는 옵션의 시작점으로 이것이 선언되어야 antMatchers 옵션을 사용할 수 있음
>>>- antMatchers
>>>>- 권한 관리 대상을 지정하는 옵션으로 URL, HTTP 메서드별로 관리할 수 있음
>>>>- "/" 등 지정된 URL은 전체 열람 권한 지정, "/api/v1/**" 주소를 가진 API는 USER 권한을 가진 사람만 가능
>>>- anyRequest
>>>>- 설정된 값 이외 나머지 URL들은 authenticated(), 즉 인증된 (로그인한) 사용자들에게만 허용
>>- src/main/java/.../springboot/config/auth/CustomOAuth2UserService.java
>>>```java
>>>@RequiredArgsConstructor
>>>@Service
>>>public class CustomOAuth2UserService implements OAuth2UserService<OAuth2UserRequest, OAuth2User> {
>>>    private final UserRepository userRepository;
>>>    private final HttpSession httpSession;
>>>
>>>    @Override
>>>    public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException {
>>>        OAuth2UserService delegate = new DefaultOAuth2UserService();
>>>        OAuth2User oAuth2User = delegate.loadUser(userRequest);
>>>
>>>        String registrationId = userRequest.getClientRegistration().getRegistrationId();
>>>        String userNameAttributeName = userRequest.getClientRegistration().getProviderDetails().getUserInfoEndpoint().getUserNameAttributeName();
>>>
>>>        OAuthAttributes attributes = OAuthAttributes.of(registrationId, userNameAttributeName, oAuth2User.getAttributes());
>>>
>>>        User user = saveOrUpdate(attributes);
>>>        httpSession.setAttribute("user", new SessionUser(user));
>>>
>>>        return new DefaultOAuth2User(
>>>                Collections.singleton(new SimpleGrantedAuthority(user.getRoleKey())),
>>>                attributes.getAttributes(),
>>>                attributes.getNameAttributeKey());
>>>    }
>>>
>>>    private User saveOrUpdate(OAuthAttributes attributes) {
>>>        User user = userRepository.findByEmail(attributes.getEmail())
>>>                .map(entity -> entity.update(attributes.getName(), attributes.getPicture()))
>>>                .orElse(attributes.toEntity());
>>>
>>>        return userRepository.save(user);
>>>    }
>>>}
>>>```
>>- src/main/java/.../springboot/config/auth/dto/OAuthAttributes.java
>>>```java
>>>@Getter
>>>public class OAuthAttributes {
>>>    private Map<String, Object> attributes;
>>>    private String nameAttributeKey;
>>>    private String name;
>>>    private String email;
>>>    private String picture;
>>>
>>>    @Builder
>>>    public OAuthAttributes(Map<String, Object> attributes, String nameAttributeKey, String name, String email, String picture) {
>>>        this.attributes = attributes;
>>>        this.nameAttributeKey = nameAttributeKey;
>>>        this.name = name;
>>>        this.email = email;
>>>        this.picture = picture;
>>>    }
>>>
>>>    public static OAuthAttributes of(String registrationId, String userNameAttributeName, Map<String, Object> attributes) {
>>>        if("naver".equals(registrationId)) {
>>>            return ofNaver("id", attributes);
>>>        }
>>>
>>>        return ofGoogle(userNameAttributeName, attributes);
>>>    }
>>>
>>>
>>>    private static OAuthAttributes ofGoogle(String userNameAttributeName, Map<String, Object> attributes) {
>>>        return OAuthAttributes.builder()
>>>                .name((String) attributes.get("name"))
>>>                .email((String) attributes.get("email"))
>>>                .picture((String) attributes.get("picture"))
>>>                .attributes(attributes)
>>>                .nameAttributeKey(userNameAttributeName)
>>>                .build();
>>>    }
>>>
>>>    private static OAuthAttributes ofNaver(String userNameAttributeName, Map<String, Object> attributes) {
>>>        Map<String, Object> response = (Map<String, Object>) attributes.get("response");
>>>
>>>        return OAuthAttributes.builder()
>>>                .name((String) response.get("name"))
>>>                .email((String) response.get("email"))
>>>                .picture((String) response.get("profile_image"))
>>>                .attributes(response)
>>>                .nameAttributeKey(userNameAttributeName)
>>>                .build();
>>>    }
>>>
>>>    public User toEntity() {
>>>        return User.builder()
>>>                .name(name)
>>>                .email(email)
>>>                .picture(picture)
>>>                .role(Role.GUEST)
>>>                .build();
>>>    }
>>>}
>>>```
>>- src/main/java/.../springboot/config/auth/dto/SessionUser.java
>>>```java
>>>@Getter
>>>public class SessionUser implements Serializable {
>>>    private String name;
>>>    private String email;
>>>    private String picture;
>>>
>>>    public SessionUser(User user) {
>>>        this.name = user.getName();
>>>        this.email = user.getEmail();
>>>        this.picture = user.getPicture();
>>>    }
>>>}
>>>```
>- 로그인 테스트
>>- index.mustache
>>>```html
>>>{{>layout/header}}
>>>
>>><h1>스프링부트로 시작하는 웹 서비스 Ver.2</h1>
>>><div class="col-md-12">
>>>    <div class="row">
>>>        <div class="col-md-6">
>>>            <a href="/posts/save" role="button" class="btn btn-primary">글 등록</a>
>>>            {{#loginUserName}}
>>>                Logged in as: <span id="user">{{loginUserName}}</span>
>>>                <a href="/logout" class="btn btn-info active" role="button">Logout</a>
>>>            {{/loginUserName}}
>>>            {{^loginUserName}}
>>>                <a href="/oauth2/authorization/google" class="btn btn-success active" role="button">Google Login</a>
>>>                <a href="/oauth2/authorization/naver" class="btn btn-secondary active" role="button">Naver Login</a>
>>>            {{/loginUserName}}
>>>        </div>
>>>    </div>
>>>    <br>
>>>    <!-- 목록 출력 영역 -->
>>>    <table class="table table-horizontal table-bordered">
>>>        <thead class="thead-strong">
>>>        <tr>
>>>            <th>게시글번호</th>
>>>            <th>제목</th>
>>>            <th>작성자</th>
>>>            <th>최종수정일</th>
>>>        </tr>
>>>        </thead>
>>>        <tbody id="tbody">
>>>        {{#posts}}
>>>            <tr>
>>>                <td>{{id}}</td>
>>>                <td><a href="/posts/update/{{id}}">{{title}}</a></td>
>>>                <td>{{author}}</td>
>>>                <td>{{modifiedDate}}</td>
>>>            </tr>
>>>        {{/posts}}
>>>        </tbody>
>>>    </table>
>>></div>
>>>
>>>{{>layout/footer}}
>>>```
>>- src/main/java/.../springboot/web/IndexController.java
>>>```java
>>>@RequiredArgsConstructor
>>>@Controller
>>>public class IndexController {
>>>    private final PostsService postsService;
>>>    private final HttpSession httpSession;
>>>
>>>    @GetMapping("/")
>>>    public String index(Model model, @LoginUser SessionUser user) {
>>>        model.addAttribute("posts", postsService.findAllDesc());
>>>
>>>        if(user != null) {
>>>            model.addAttribute("loginUserName", user.getName());
>>>        }
>>>        return "index";
>>>    }
>>>
>>>    // ...
>>>}
>>>```

<br>

[목차로 이동](#목차)

>### 어노테이션 기반으로 개선하기
>- 세션값을 가져오는 부분을 메서드 인자로 세션값을 바로 받을 수 있도록 개선
>>- src/main/java/.../springboot/config/auth/LoginUser.java
>>>```java
>>>@Target(ElementType.PARAMETER)
>>>@Retention(RetentionPolicy.RUNTIME)
>>>public @interface LoginUser {
>>>}
>>>```
>>>- @Target(ElementType.PARAMETER)
>>>>- 해당 어노테이션이 생성될 수 있는 위치를 지정, PARAMETER로 지정했으므로 메서드의 파라미터로 선언된 객체에서만 사용할 수 있음
>>>- @interface
>>>>- 해당 파일은 어노테이션 클래스로 지정하여 LoginUser 라는 이름을 가진 어노테이션이 생성된것과 같음
>>- src/main/java/.../springboot/config/auth/LoginUserArgumentResolver.java
>>>```java
>>>@RequiredArgsConstructor
>>>@Component
>>>public class LoginUserArgumentResolver implements HandlerMethodArgumentResolver {
>>>    private final HttpSession httpSession;
>>>
>>>    @Override
>>>    public boolean supportsParameter(MethodParameter parameter) {
>>>        boolean isLoginUserAnnotation = parameter.getParameterAnnotation(LoginUser.class) != null;
>>>        boolean isUserClass = SessionUser.class.equals(parameter.getParameterType());
>>>        return isLoginUserAnnotation && isUserClass;
>>>    }
>>>
>>>    @Override
>>>    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
>>>        return httpSession.getAttribute("user");
>>>    }
>>>}
>>>```
>>>- supportsParameter()
>>>>- 컨트롤러 메서드의 특정 파라미터를 지원하는지 판단
>>>>- 해당 경우에는 파라미터에 @LoginUser 어노테이션이 붙어 있고, 파라미터 클래스 타입이 SessionUser.class인 경우 true 반환
>>>- resolveArgument()
>>>>- 파라미터에 전달할 객체를 생성
>>>>- 해당 경우에는 세션에서 객체를 가져옴
>>- src/main/java/.../springboot/config/WebConfig.java
>>>```java
>>>@RequiredArgsConstructor
>>>@Configuration
>>>public class WebConfig implements WebMvcConfigurer {
>>>    private final LoginUserArgumentResolver loginUserArgumentResolver;
>>>
>>>    @Override
>>>    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
>>>        argumentResolvers.add(loginUserArgumentResolver);
>>>    }
>>>}
>>>```

<br>

[목차로 이동](#목차)

>### 세션 저장소로 데이터베이스 사용하기
>- build.gradle 에 spring-session-jdbc 의존성 추가
>>```gradle
>>implementation('org.springframework.session:spring-session-jdbc')
>>```
>- application.properties 설정 추가
>>```properties
>>spring.session.store-type=jdbc
>>```
>- 세션테이블
>>- http://localhost:8080/h2-console 로 접속시 SPRING_SESSION, SPRING_SESSION_ATTRIBUTES 두 개의 세션을 위한 테이블이 JPA에 의해 자동으로 생섣되어 있음

<br>

[목차로 이동](#목차)

>### 네이버 로그인
>- 네이버 API 등록 순서
>>1. https://developers.naver.com/apps/#/register?api=nvlogin 접속
>>2. 애플리케이션 이름과 사용 API에 네이버 아이디로 로그인으로 선택, 제공 정보로 회원이름, 이메일, 프로필 사진 클릭
>>3. 로그인 오픈 API 서비스 환경에서 PC 웹, 서비스 URL 로 http://localhost:8080/ , 네이버아이디로 로그인 CallbackURL로 http://localhost:8080/login/oauth2/code/naver 입력
>>4. 등록된 네이버 서비스에서 애플리케이션 정보의 Client ID와 Cliend Secret 을 사용하여 로그인 구현
>>>- application-oauth.properties
>>>>```properties
>>>># ...
>>>>
>>>># registraion
>>>>spring.security.oauth2.client.registration.naver.client-id=클라이언트 ID
>>>>spring.security.oauth2.client.registration.naver.client-secret=클라이언트 Secret
>>>>spring.security.oauth2.client.registration.naver.redirect-uri={baseUrl}/{action}/oauth2/code/{registrationId}
>>>>spring.security.oauth2.client.registration.naver.authorization_grant_type=authorization_code
>>>>spring.security.oauth2.client.registration.naver.scope=name,email,profile_image
>>>>spring.security.oauth2.client.registration.naver.client-name=Naver
>>>>
>>>># provider
>>>>spring.security.oauth2.client.provider.naver.authorization_uri=https://nid.naver.com/oauth2.0/authorize
>>>>spring.security.oauth2.client.provider.naver.token_uri=https://nid.naver.com/oauth2.0/token
>>>>spring.security.oauth2.client.provider.naver.user-info-uri=https://openapi.naver.com/v1/nid/me
>>>>spring.security.oauth2.client.provider.naver.user_name_attribute=response
>>>>```

<br>

[목차로 이동](#목차)

>### 기존 테스트에 시큐리티 적용하기
>- 기존 테스트의 문제점과 개선방향
>>- 기존 테스트에선 API를 바로 호출해도 문제가 없었으나, 시큐리티 옵션이 활성화되면 인증된 사용자만 API를 호출할 수 있음
>>- 기존의 API 테스트 코드들에 인증한 사용자가 호출한 것처럼 작동하도록 수정
>- CustomOAuth2UserService 를 찾을 수 없음
>>1. hello가_리턴된다 의 No qualifying bean of type 'com.~~.CusomOAuth2UserService 메시지는 생성 시 소셜 로그인 관련 설정값들이 없기 때문에 발생
>>2. src/main 의 application.properties가 src/test 에서 적용은 되지만 application-oauth.properties는 적용되지 않아 생기는 문제로, test환경에 맞는 properties를 작성해야함
>>- src/test/resources/application.properties
>>>```properties
>>>spring.jpa.show_sql=true
>>>spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
>>>spring.h2.console.enabled=true
>>>
>>>spring.session.store-type=jdbc
>>>
>>># Test OAuth
>>>
>>>spring.security.oauth2.client.registration.google.client-id=test
>>>spring.security.oauth2.client.registration.google.client-secret=test
>>>spring.security.oauth2.client.registration.google.client-scope=profile,email
>>>```
>- 302 Status Code (리다이렉션 응답)
>>1. 인증되지 않은 사용자의 요청은 이동시키기 때문에 생기는 결과로 임의로 인증된 사용자를 추가하여 API 테스트 진행해야함
>>- build.gradle 에 spring-security-test 의존성 추가
>>>```gradle
>>>testImplementation('org.springframework.security:spring-security-test')
>>>```
>>- src/test/java/.../web/PostsApiControllerTest.java
>>>```java
>>>import static org.assertj.core.api.Assertions.assertThat;
>>>import static org.springframework.security.test.web.servlet.setup.SecurityMockMvcConfigurers.springSecurity;
>>>import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
>>>import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.put;
>>>import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
>>>
>>>@RunWith(SpringRunner.class)
>>>@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
>>>public class PostsApiControllerTest {
>>>    @LocalServerPort
>>>    private int port;
>>>
>>>    @Autowired
>>>    private TestRestTemplate restTemplate;
>>>
>>>    @Autowired
>>>    private PostsRepository postsRepository;
>>>
>>>    @Autowired
>>>    private WebApplicationContext context;
>>>
>>>    private MockMvc mvc;
>>>
>>>    @Before
>>>    public void setup() {
>>>        mvc = MockMvcBuilders
>>>                .webAppContextSetup(context)
>>>                .apply(springSecurity())
>>>                .build();
>>>    }
>>>
>>>    @After
>>>    public void tearDown() throws Exception {
>>>        postsRepository.deleteAll();
>>>    }
>>>
>>>    @Test
>>>    @WithMockUser(roles="USER")
>>>    public void Posts_등록된다() throws Exception {
>>>        // given
>>>        String title = "title";
>>>        String content = "content";
>>>        PostsSaveRequestDto requestDto = PostsSaveRequestDto.builder()
>>>                .title(title)
>>>                .content(content)
>>>                .author("author")
>>>                .build();
>>>
>>>        String url = "http://localhost:" + port + "/api/v1/posts";
>>>
>>>        // when
>>>        mvc.perform(post(url)
>>>                .contentType(MediaType.APPLICATION_JSON_UTF8)
>>>                .content(new ObjectMapper().writeValueAsString(requestDto)))
>>>                .andExpect(status().isOk());
>>>
>>>        // then
>>>        List<Posts> all = postsRepository.findAll();
>>>        assertThat(all.get(0).getTitle()).isEqualTo(title);
>>>        assertThat(all.get(0).getContent()).isEqualTo(content);
>>>    }
>>>
>>>    @Test
>>>    @WithMockUser(roles="USER")
>>>    public void Posts_수정된다() throws Exception {
>>>        // given
>>>        Posts savedPosts = postsRepository.save(Posts.builder()
>>>                .title("title")
>>>                .content("content")
>>>                .author("author")
>>>                .build());
>>>
>>>        Long updateId = savedPosts.getId();
>>>        String expectedTitle = "title2";
>>>        String expectedContent = "content2";
>>>
>>>        PostsUpdateRequestDto requestDto = PostsUpdateRequestDto.builder()
>>>                .title(expectedTitle)
>>>                .content(expectedContent)
>>>                .build();
>>>
>>>        String url = "http://localhost:" + port + "/api/v1/posts/" + updateId;
>>>
>>>        HttpEntity<PostsUpdateRequestDto> requestEntity = new HttpEntity<>((requestDto));
>>>
>>>        // when
>>>        mvc.perform(put(url)
>>>                .contentType(MediaType.APPLICATION_JSON_UTF8)
>>>                .content(new ObjectMapper().writeValueAsString(requestDto)))
>>>                .andExpect(status().isOk());
>>>
>>>        // then
>>>        List<Posts> all = postsRepository.findAll();
>>>        assertThat(all.get(0).getTitle()).isEqualTo(expectedTitle);
>>>        assertThat(all.get(0).getContent()).isEqualTo(expectedContent);
>>>    }
>>>}
>>>```
>- @WebMvcTest 에서 CustomOAuth2UserService 를 찾을 수 없음
>>- @WebMvcTest 는 WebSecurityConfigurerAdapter, WebMvcConfigurer 를 비롯한 @ControllerAdvice, @Controller를 읽고, @Repository, @Service, @Component는 스캔대상이 아님
>>- SecurityConfig 는 읽었지만 SecurityConfig 를 생성하기 위한 CustomOAuth2UserService 는 읽을 수 없어 발생
>>- 스캔 대상에서 SecurityConfig 를 제거하여 해결하며, 여기서도 @WithMockUser로 테스트용 인증된 사용자를 생성
>>>```java
>>>@RunWith(SpringRunner.class)
>>>@WebMvcTest(controllers = HelloController.class,
>>>    excludeFilters = {
>>>        @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = SecurityConfig.class)
>>>    })
>>>public class HelloControllerTest {
>>>    @Autowired
>>>    private MockMvc mvc;
>>>
>>>    @WithMockUser(roles="USER")
>>>    @Test
>>>    public void hello가_리턴된다() throws Exception {
>>>        String hello = "hello";
>>>
>>>        mvc.perform(get("/hello"))
>>>                .andExpect(status().isOk())
>>>                .andExpect(content().string(hello));
>>>    }
>>>
>>>    @WithMockUser(roles="USER")
>>>    @Test
>>>    public void helloDto가_리턴된다() throws Exception {
>>>        String name = "hello";
>>>        int amount = 1000;
>>>
>>>        mvc.perform(
>>>                    get("/hello/dto")
>>>                        .param("name", name)
>>>                        .param("amount", String.valueOf(amount)))
>>>                .andExpect(status().isOk())
>>>                .andExpect(jsonPath("$.name", is(name)))
>>>                .andExpect(jsonPath("$.amount", is(amount)));
>>>    }
>>>}
>>>```
>>- 이후 @EnableJpaAuditing 으로 인해 At least one JPA metamodel must be present 에러가 발생
>>>- @EnableJpaAuditing 을 사용하려면 최소 하나의 @Entity 클래스가 필요한데 @WebMvcTest이기 때문에 없어서 생기는 문제
>>>- @EnableJpaAuditing 과 @SpringBootApplication 을 분리함
>>>- src/main/java/.../springboot/Application.java
>>>>```java
>>>>@SpringBootApplication
>>>>public class Application {
>>>>    public static void main(String[] args) {
>>>>        SpringApplication.run(Application.class, args);
>>>>    }
>>>>}
>>>>```
>>>- src/main/java/.../springboot/config/JpaConfig.java
>>>>```java
>>>>@Configuration
>>>>@EnableJpaAuditing
>>>>public class JpaConfig {
>>>>}
>>>>```

<br>

[목차로 이동](#목차)

---

## AWS 서버 환경을 만들어보자 - AWS EC2

>### AWS EC2 개요
>- AWS (Amazon Web Service) 라는 클라우드 서비스를 이용하여 서버 배포를 할 수 있음
>- 외부에 서비스하려면 24시간 작동하는 서버가 필요한데 3가지 선택지가 있음
>>- 집 PC 24시간 구동 (비용 저렴)
>>- 호스팅 서비스 이용 (Cafe 24, 코리아 호스팅 등)
>>- 클라우드 서비스 이용 (AWS, AZURE, GCP 등) (특정 시간에 트래픽이 몰릴 경우 유동적으로 사양을 늘릴 수 있어 유리)
>- AWS의 EC2는 서버 장비를 대여하면서 해당 장비의 로그 관리, 모니터링, 하드웨어 교체, 네트워크 관리 등을 기본적으로 지원함
>- 클라우드의 형태
>>- Infrastructure as a Service (IaaS)
>>>- 기존 물리 장비를 미들웨어와 함께 묶어둔 추상화 서비스
>>>- 가상머신, 스토리지, 네트워크, 운영체제 등의 IT 인프라를 대여해주는 서비스
>>>- AWS의 EC3, S3 등
>>- Platform as a Service (PaaS)
>>>- IaaS에서 한 단계 더 추상화하여 많은 기능이 자동화됨
>>>- AWS의 Beanstalk, Heroku 등
>>- Software as a Service (SaaS)
>>>- 소프트웨어 서비스를 뜻하며 구글 드라이브, 드랍박스, 와탭 등이 있음

<br>

[목차로 이동](#목차)

>### EC2 인스턴스 생성하기 (Elastic Compute Cloud)
>- 일반적으로 AWS에서 리눅스 서버 혹은 윈도우 서버를 사용한다면 EC2에 해당함
>- 프리티어 제한
>>- t2.micro 사양만 사용할 수 있음
>>>- vCPU (가상 CPU) 1Core
>>>>- (물리 CPU 사양의 절반 정도의 성능)
>>>- 메모리 1GB
>>>- 월 750시간 초과 시 비용 부과
>>>>- (1대의 t2.micro만 사용한다면 24*31=744 이므로 24시간 사용 가능)
>- 기본 세팅
>>- 리전 서울로 변경
>>- 상단 검색창에서 EC2 검색
>>- EC2 대시보드에서 인스턴스 시작 클릭
>>- AMI (Amazon Machine Image) 로 Amazon Linux 선택
>>- 인스턴스 유형에서 t2.micro 선택
>>>- t2는 요금 타입, micro는 사양을 뜻함
>>>- t2, t3 등 T 시리즈들은 범용 시리즈로 불리며 다양한 사양을 사용할 수 있음
>>>- T 시리즈에는 크레딧이라는 일종의 CPU를 사용할 수 있는 포인트개념이 있어 크래딧을 축적 및 사용하여 유동적으로 트래픽을 처리할 수 있음
>>- 기업에서 사용 시 세부정보 구성 중 VPC, 서브넷 등 세세하게 설정하지만 1대의 서버만을 사용하므로 기본 설정으로 진행
>>- 스토리지는 프리티어일 경우 기본적으로 30GB까지 지원함
>>- 이름 및 태그 입력
>>- 보안 그룹은 방화벽을 뜻하며 보안 그룹 관리를 제대로 하지 않을 시 해당 서버에서 가상화폐 채굴등 초대량의 트래픽이 발생하여 감당못할 수준의 비용이 청구되므로 pem 키 관리와 지정된 IP 에서만 ssh 접속이 가능하도록 구성하는것이 안전함
>>>- ssh 이면서 22번 포트는 AWS EC2에 터미널로 접속할때를 의미하므로 보안 관리에 철저해야함
>>>- 사용자 지정 TCP 의 8080 포트는 전체 오픈 (0.0.0.0/0 과 ::/0)
>>>- HTTPS 의 443 포트도 전체 오픈(0.0.0.0/0 과 ::/0)
>>- 인스턴스로 접근하기 위해서는 pem키(비밀키)가 필요
>>>- 인스턴스는 지정된 pem 키와 매칭되는 공개키를 가지고 있어 해당 pem 키 외에는 접근을 허용하지 않음
>>>- 일종의 마스터키이기 때문에 절대 유출되면 안되고 이후 EC2 서버로 접속할 때 필수 파일이므로 잘 관리할 수 있는 디렉토리로 따로 저장
>>- 인스턴스 생성버튼 클릭 후 인스턴스 id를 클릭하여 EC 목록으로 이동
>>- 인스턴스에는 IP와 도메인이 할당되는데 인스턴스 중지 후 다시 시작하면 새 IP가 할당되므로 고정 IP를 할당해야함
>>- EIP 할당 (Elastic IP, 탄력적 IP)
>>>- 인스턴스 페이지의 좌측 카테고리에서 네트워크 및 보안 탭에서 탄력적IP 클릭를 클릭하고 새 주로 할당 버튼 클릭후 바로 할당 클릭
>>>- 새 주소 할당이 완료되면 상단의 작업 버튼 -> 주소 연결 선택 후 EC2 이름 과 IP를 선택하고 연결버튼 클릭
>>>- 좌측의 인스턴스 탭을 클릭하여 인스턴스 목록 페이지로 돌아와서 퍼블릭, 탄력적 IP가 잘 연결되었는지 확인
>>>- 탄력적 IP는 생성 후 EC2 서버에 연결하지 않으면 비용이 발생하므로 무조건 EC2에 연결해야 함
>>>- 더 이상 사용할 인스턴스가 없을 때도 탄력적 IP를 삭제하지 않으면 비용이 청구 되므로 꼭 잊지 말고 삭제

<br>

[목차로 이동](#목차)

>### EC2 서버에 접속하기
>- 오랜 시간 접속이 안 되거나, 권한이 없는 메시지가 나올 경우
>>- HostName 값이 정확히 탄력적 IP로 되어있는지 확인
>>- EC2 인스턴스가 running 상태인지 확인
>>- EC2 인스턴스의 보안그룹 -> 인바운드 규칙에서 현재 본인의 IP가 등록되어 있는 지확인
>- Mac & Linux - 터미널
>>1. AWS와 같은 외부 서버로 SSH 접속을 하려면 입력해야하는 명령
>>>- ssh -i pem 키 위치 EC의 탄력적 IP 주소
>>2. 매번 입력하기 힘들 수 있으므로 쉽게 ssh 접속을 할 수 있도록 설정
>>>- 키페어 pem 파일을 ~/.ssh/ 로 복사하면 ssh 실행 시 pem 키 파일을 자동으로 읽어 접속을 진행함
>>>- cp pem 키를 내려받은 위치 ~/.ssh/
>>3. pem 키의 권한 변경
>>>- chmod 600 ~/.ssh/pem키이름
>>4. pem 키가 있는 ~/.ssh 디렉토리에 config 파일 생성
>>>- vim ~/.ssh/config
>>>>```
>>>># 주석
>>>>Host 본인이 원하는 서비스 명
>>>>  HostName 탄력적 IP 주소
>>>>  User ec2-user
>>>>  IdentityFile ~/.ssh/pem키이름
>>>>```
>>>- chmod 700 ~/.ssh/config
>>5. 실행
>>>- ssh config에 등록한 서비스명
>- Windows - putty
>>1. pem 키를 가지고 있다면 ppk 파일로 변경
>>2. putty의 Session 에서 항목 작성
>>>- HostName : username(Amazon Linux는 ec2-user)@public_Ip(탄력적 IP 주소)
>>>- Port : ssh 접속 포트인 22
>>>- Connection type : SSH
>>3. 좌측 사이드바의 Connection -> SSH -> Auth 에서 Private key file for authentication 의 Browse... 를 클릭하여 ppk 파일 로드
>>4. Session 탭으로 이동하여 Saved Sessions에 현재 설정을 저장할 이름을 작성하고 Save
>>5. 저장 후 open을 클릭하면 SSH 접속 알림이 뜨는데 예 클릭

<br>

[목차로 이동](#목차)

>### 아마존 리눅스 서버 생성 시 꼭 해야 할 설정들
>- 자바 기반의 웹 애플리케이션 (톰캣과 스프링부트) 가 작동해야 하는 서버들에선 필수로 해야하는 설정들
>>- Java 8 설치
>>- 타임존 변경
>>>- 기본 서버의 시간은 미국 시간
>>- 호스트네임 변경
>>>- 실무에서는 수십 대의 서버가 작동되는 데, IP만으로 어떤 서버가 어떤 역할을 하는지 알 수없어 구분하기 위해 호스트 네임을 필수로 등록함
>- Java 8 설치
>>- sudo yum install -y java-1.8.0-openjdk-devel.x86_64
>- 타임존 변경
>>- sudo rm /etc/localtime
>>- sudo ln -s /usr/share/zoneinfo/Asia/Seoul etc/localtime
>- Hostname 변경
>>1. sudo vi /etc/cloud/cloud.cfg
>>2. preserve_hostname: true 설정이 없다면 추가
>>3. 아마존 리눅스 버전에 따른 설정 차이
>>- Amazon Linux 1 의경우
>>>1. sudo vim /etc/sysconfig/network
>>>2. HOSTNAME=원하는호스트명 으로 변경
>>>3. sudo reboot
>>- Amazon Linux 2 의경우
>>>1. sudo hostnamectl set-hostname 원하는호스트명
>>>2. sudo reboot
>>4. 호스트 주소를 찾을 때 가장 먼저 검색해보는 /etc/hosts에 변경한 hostname 등록
>>>- sudo vim /etc/hosts
>>>- 127.0.0.1 등록한 HOSTNAME 입력
>>>- curl 등록한 HOSTNAME 검색시 80 포트로 실행된 서비스가 없을 경우 접근이 안 된다는 에러 발생

<br>

[목차로 이동](#목차)

---

## AWS에 데이터베이스 환경을 만들어보자 - AWS RDS

>### AWS RDS 개요
>- 데이터베이스 구축
>>- 직접 데이터베이스를 설치해서 다루게 되면 처음 구축시 며칠이 걸릴 수 있는 일인 모니터링, 알람, 백업, HA 구성 등을 모두 직접 해야함
>>- AWS 에서는 언급된 작업을 모두 지원하는 관리형 서비스인 RDS(Relational Database Service)를 제공함
>- RDS
>>- AWS에서 지원하는 클라우드 기반 관계형 데이터베이스임
>>- 하드웨어 프로비저닝, 데이터베이스 설정, 패치 및 백업과 같이 잦은 운영 작업을 자동화하여 개발자가 개발에 집중할 수 있게 지원하는 서비스
>>- 조정 가능한 용량을 지원하여 예상치 못한 양의 데이터가 쌓여도 비용만 추가로 내면 정상적으로 서비스가 가능함

<br>

[목차로 이동](#목차)

>### RDS 인스턴스 생성하기
>- 생성 순서
>>1. 상단 검색창에 rds 검색 후 선택하여 RDS 대시보드로 진입해서 데이터베이스 생성 버튼 클릭
>>2. Amazon Aurora로 교체가 용이한 MariaDB를 DB 엔진으로 선택
>>3. 프리티어 템플릿을 선택하고 스토리지 용량을 20GB로 설정
>>4. DB 인스턴스와 마스터 사용자 정보를 등록
>>5. 퍼블릭 액세스를 예로 변경 (이후 보안 그룹에서 지정된 IP만 접근하도록 막을 예정)
>>6. 데이터베이스 이름 입력 후 비용청구를 방지하기 위해 자동 백업 기능을 비활성화
>>7. 생성

<br>

[목차로 이동](#목차)

>### RDS 운영환경에 맞는 파라미터 설정하기
>- RDS 처음 생성시 필수로 해야 하는 설정
>>1. RDS 좌측 카테고리에서 파라미터 그룹 탭 클릭 후 파라미터 그룹 생성 클릭
>>2. 파라미터 그룹 패밀리에서 RDS의 MariaDB와 같은 버전으로 설정 후 생성
>>3. 생성된 파라미터 그룹을 클릭후 파라미터 편집 클릭
>>4. time_zone 을 검색하여 Asia/Seoul 설정
>>5. character 항목들은 모두 utf8mb4로, collation 항목들은 utf8mb4_general_ci로 설정
>>6. max_connections 의 기본값은 프리티어 사양에서는 약 60개의 커넥션만 가능하므로 150개로 지정 (추후 RDS 사양을 높이면 기본값으로 복구)
>>7. 변경 사항 저장후 데이터베이스의 수정창으로 접근하여 DB 파라미터 그룹을 방금 생성한 신규 파라미터 그룹으로 변경후 즉시 적용 및 재부팅을 진행

<br>

[목차로 이동](#목차)

>### 내 PC에서 RDS에 접속해 보기
>- RDS의 보안 그룹에 본인 PC의 IP 추가
>>1. RDS의 세부정보 페이지에서 보안 그룹 항목을 클릭
>>2. 새 창을 열어서 본인의 IP와 EC2에 사용된 보안 그룹의 그룹 ID를 복사하여 RDS의 보안 그룹의 인바운드로 추가 (규칙 유형은 MYSQL/Aurora)
>- Database 플러그인
>>- 책에서는 인텔리제이의 Database Navigator에서는 MariaDB 를 사용하는데 오류가 발생하여 Mysql Workbench 를 사용하여 테스트함
>>- RDS의 엔드 포인트를 호스트로 이용하여 마스터아이디와 비밀번호로 RDS에 연결함
>- RDS 테스트
>>1. use AWS_RDS_웹콘솔에서지정한데이터베이스명
>>2. show variables like 'c%'
>>>```SQL
>>>ALTER DATABASE 데이터베이스명
>>>CHARACTER SET = 'utf8mb4'
>>>COLLATE = 'utf8mb4_general_ci';
>>>```
>>3. select @@time_zone, now();

<br>

[목차로 이동](#목차)

>### EC2에서 RDS에 접근 확인
>- $ sudo yum install mysql
>- $ mysql -u 마스터계정 -p -h Host주소
>>- 이후 비밀번호 입력하여 접속
>- $ show databases;

<br>

[목차로 이동](#목차)

---

## EC2 서버에 프로젝트를 배포해보자

>### EC2에 프로젝트 Clone 받기
>- 깃 설치
>>1. $ sudo yum install git
>>2. $ mkdir ~/app && mkdir ~/app/step1
>- 프로젝트 clone
>>1. $ cd ~/app/step1
>>2. $ git clone 프로젝트 주소
>- 테스트
>>1. $ ./gradlew test
>>>- 권한 오류 발생시 $ chmod +x ./gradlew

<br>

[목차로 이동](#목차)

>### 배포 스크립트 만들기
>- 배포란?
>>- git clone 혹은 git pull을 통해 새 버전의 프로젝트를 받음
>>- Gradle이나 Maven을 통해 프로젝트 테스트와 빌드
>>- EC2 서버에서 해당 프로젝트 실행 및 재실행
>- 쉘 스크립트 작성
>>- 배포할 때마다 개발자가 하나하나 명령어를 실행하는것은 불편함이 많음
>>- $ vim ~/app/step1/deploy.sh
>>>```sh
>>>#!/bin/bash
>>>
>>>REPOSITORY=/home/ec2-user/app/step1
>>>PROJECT_NAME=hello_IntelliJ
>>>
>>>cd $REPOSITORY/$PROJECT_NAME/
>>>
>>>echo "> Git Pull"
>>>
>>>git pull
>>>
>>>echo "> 프로젝트 Build 시작"
>>>
>>>./gradlew build
>>>
>>>echo "> step1 디렉토리로 이동"
>>>
>>>cd $REPOSITORY
>>>
>>>echo "> Build 파일 복사"
>>>
>>>cp $REPOSITORY/$PROJECT_NAME/build/libs/*.jar $REPOSITORY/
>>>
>>>echo "> 현재 구동중인 애플리케이션 pid 확인"
>>>
>>>CURRENT_PID=$(pgrep -f ${PROJECT_NAME}.*.jar)
>>>
>>>echo "현재 구동중인 애플리케이션 pid: $CURRENT_PID"
>>>
>>>if [ -z "$CURRENT_PID" ]; then
>>>        echo "> 현재 구동 중인 애플리케이션이 없으므로 종료하지 않습니다."
>>>else
>>>        echo "> kill -15 $CURRENT_PID"
>>>        kill -15 $CURRENT_PID
>>>        sleep 5
>>>fi
>>>
>>>echo "> 새 애플리케이션 배포"
>>>
>>>JAR_NAME=$(ls -tr $REPOSITORY/ | grep jar | tail -n 1)
>>>
>>>echo "> JAR Name: $JAR_NAME"
>>>
>>>nohup java -jar \
>>>        -Dspring.config.location=classpath:/application.properties,/home/ec2-user/app/application-oauth.properties,/home/ec2-user/app/application-real-db.properties,classpath:/application-real.properties \
>>>        -Dspring.profiles.active=real \
>>>        $REPOSITORY/$JAR_NAME 2>&1 &
>>>```
>>- $ chmod +x ./deploy.sh
>>- $ ./deploy.sh
>>- $ vim nohup.out
>>>- application-oauth.properties 가 존재하지 않으므로 오류 발생

<br>

[목차로 이동](#목차)

>### 외부 Security 파일 등록하기
>- .gitignore 로 제외 대상인 설정파일들의 부재로 원활한 배포가 불가능하므로 서버에 직접 설정파일을 가지고 있도록 세팅함
>- step1이 아닌 app 디렉토리에 .properties 파일을 생성
>>- $ vim /home/ec2-user/app/application-oauth.properties
>>>- 로컬의 application-oauth.properties 내용을 붙여넣기
>- deploy.sh 파일에 application-oauth.properties를 쓰도록 수정함
>- $ ./deploy.sh 
>>- 정상적으로 실행됨

<br>

[목차로 이동](#목차)

>### 스프링 부트 프로젝트로 RDS 접근하기
>- RDS에서 스프링부트 프로젝트를 실행하기 위해 필요한 작업
>>- 테이블 생성
>>>- H2에서 자동 생성해주던 테이블들을 MariaDB에서 직접 쿼리로 생성
>>- 프로젝트 설정
>>>- 자바 프로젝트가 MariaDB에 접근하려면 데이터베이스 드라이버가 필요하므로 MariaDB에서 사용 가능한 드라이버를 프로젝트에 추가
>>- EC2 설정
>>>- DB 접속 정보를 외부에 공개하면 해킹당할 위험이 크므로 EC2 서버 내부에서 접속 정보를 관리하도록 설정
>- RDS 테이블 생성
>>- JPA 테스트 수행시 발생하는 로그의 쿼리를 이용하여 RDS에 반영함
>>>```sql
>>>create table posts (id bigint not null auto_increment, create_date datetime, modified_date datetime, author varchar(255), content TEXT not null, title varchar(500) not null, primary key (id)) engine=InnoDB;
>>>
>>>create table user (id bigint not null auto_increment, create_date datetime, modified_date datetime, email varchar(255) not null, name varchar(255) not null, picture varchar(255), role varchar(255) not null, primary key (id)) engine=InnoDB;
>>>```
>>- 스프링 세션 테이블은 schema-mysql.sql 파일에서 확인 (Ctrl+Shift+N으로 파일찾기)
>>>```sql
>>>CREATE TABLE SPRING_SESSION (
>>>	PRIMARY_ID CHAR(36) NOT NULL,
>>>	SESSION_ID CHAR(36) NOT NULL,
>>>	CREATION_TIME BIGINT NOT NULL,
>>>	LAST_ACCESS_TIME BIGINT NOT NULL,
>>>	MAX_INACTIVE_INTERVAL INT NOT NULL,
>>>	EXPIRY_TIME BIGINT NOT NULL,
>>>	PRINCIPAL_NAME VARCHAR(100),
>>>	CONSTRAINT SPRING_SESSION_PK PRIMARY KEY (PRIMARY_ID)
>>>) ENGINE=InnoDB ROW_FORMAT=DYNAMIC;
>>>
>>>CREATE UNIQUE INDEX SPRING_SESSION_IX1 ON SPRING_SESSION (SESSION_ID);
>>>CREATE INDEX SPRING_SESSION_IX2 ON SPRING_SESSION (EXPIRY_TIME);
>>>CREATE INDEX SPRING_SESSION_IX3 ON SPRING_SESSION (PRINCIPAL_NAME);
>>>
>>>CREATE TABLE SPRING_SESSION_ATTRIBUTES (
>>>	SESSION_PRIMARY_ID CHAR(36) NOT NULL,
>>>	ATTRIBUTE_NAME VARCHAR(200) NOT NULL,
>>>	ATTRIBUTE_BYTES BLOB NOT NULL,
>>>	CONSTRAINT SPRING_SESSION_ATTRIBUTES_PK PRIMARY KEY (SESSION_PRIMARY_ID, ATTRIBUTE_NAME),
>>>	CONSTRAINT SPRING_SESSION_ATTRIBUTES_FK FOREIGN KEY (SESSION_PRIMARY_ID) REFERENCES SPRING_SESSION(PRIMARY_ID) ON DELETE CASCADE
>>>) ENGINE=InnoDB ROW_FORMAT=DYNAMIC;
>>>```
>- 프로젝트 설정
>>- build.gradle 에 MariaDB 드라이버 의존성 추가
>>>- implementation('org.mariadb.jdbc:mariadb-java-client')
>>- src/main/resources/ 에 application-real.properties 추가
>>>```properties
>>>spring.profiles.include=oauth,real-db
>>>spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
>>>spring.session.store-type=jdbc
>>>```
>>>- 실제 운영될 환경이기 때문에 보안/로그상 이슈가 될 만한 설정들을 모두 제거한 RDS 환경 profile 설정을 추가함
>- EC2 설정
>>- EC2 서버에도 app/application-real-db.properties 설정파일을 추가함
>>>- $ vim ~/app/application-real-db.properties
>>>```properties
>>>spring.jpa.hibernate.ddl-auto=none
>>>spring.datasource.url=jdbc:mariadb://rds주소:포트명(기본은3306)/database이름
>>>spring.datasource.username=마스터유저네임
>>>spring.datasource.password=마스터비밀번호
>>>spring.datasource.driver-class-name=org.mariadb.jdbc.Driver
>>>```
>>- deploy.sh 가 real profile을 쓸 수 있도록 작성함
>>>- 처음 입력한 deploy.sh에 모든 내용이 작성되어 있음 참고할것
>>- $ ./deploy.sh를 실행후 nohup.out 파일을 확인하여 성공 로그 확인
>>>- 성공하지 않았다면 gredlew 의 실행권한을 확인할것!! 실행권한이 없어 빌드되지않아 정상적으로 배포되지 않는 상황을 경험함
>>- $ curl localhost:8080 에서 index.mustache의 내용이 출력되면 성공

<br>

[목차로 이동](#목차)

>### EC2에서 소셜 로그인하기
>- AWS EC2 도메인으로 접속
>>- EC2의 인스턴스메뉴로 들어가 본인이 생성한 EC2 인스턴스를 선택하면 퍼블릭 DNS를 확인할 수 있음
>>- 브라우저에서 퍼블릭DNS:포트번호 로 접속시 index.mustache 화면이 출력됨
>>- 구글 및 네이버 서비스에 EC2 도메인을 등록하지 않았기 때문에 로그인이 동작하지 않음
>- 구글에 EC2 주소 등록
>>- [구글 웹 콘솔](https://console.cloud.google.com/home/dashboard) 에 접속하여 프로젝트로 이동한 후 API 및 서비스 -> 사용자 인증 정보 로 이동하여 OAuth 동의 화면 탭에서 승인된 도메인에 http:// 없이 EC2의 퍼블릭 DNS를 등록함
>>- 사용자 인증 정보 탭에서 서비스의 이름을 클릭하고 승인된 리디렉션 URI에 http:// 퍼블릭 DNS 주소:8080/login/oauth2/code/google 등록
>- 네이버에 EC2 주소 등록
>>- [네이버 개발자 센터](https://developers.naver.com/apps/#/myapps) 로 접속해서 본인의 프로젝트로 이동
>>- PC 웹 항목에서 서비스 URL와 Callback URL을 http:// 퍼블릭DNS 로 수정함
>- 현재 방식의 문제점
>>- 수동 실행되는 Test
>>>- 본인이 짠 코드가 다른 개발자의 코드에 영향을 끼치지 않는지 확인하기 위해 전체 테스트를 수행해야함
>>>- 현재 상태에선 항상 개발자가 작업ㅇ르 진행할 떄마다 수동으로 전체 테스트를 수행해야함
>>- 수동 Build
>>>- 다른 사람이 작성한 브랜치와 본인이 작성한 브랜치가 합쳐졌을 때(Merge) 이상이 없는지는 Build를 수행해야만 알 수 있음
>>>- 이를 매번 개발자가 직접 실행해야만 함

<br>

[목차로 이동](#목차)

---

## 코드가 푸시되면 자동으로 배포해보자 - Travis CI 배포 자동화

>### CI & CD 소개
>- 개요
>>- 여러 개발자의 코드가 실시간으로 병합되고, 테스트가 수행되는 환경, master 브랜치가 푸시되면 배포가 자동으로 이루어지는 환경을 구축하지 않으면 실수할 여지가 너무나 많음
>>- 