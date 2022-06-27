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

>###
>- 
