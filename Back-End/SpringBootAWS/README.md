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


<br>

---

>## 주의사항
>- 문서 작성일 (22/06/25) 기준 gradle 최신버전은 7 버전이지만 참고자료 서적은 gradle 4 버전 기준으로 작성되었고 저자 또한 첫 학습은 4 버전 환경을 추천하여 작성자 본인의 실습 또한 4.10.2 버전으로 다운그레이드하여 진행하였음


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