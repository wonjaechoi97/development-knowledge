# **Spring** 🐍

### 참고자료

- 토비의 스프링 3.1


## 목차
  - [개요](#개요)
    - [**Spring**의 정의](#spring의-정의)
    - [**POJO**](#pojo)
    - [**IoC/DI**](#iocdi)
    - [**AOP**](#aop)
    - [**PSA**](#psa)
  - [특징](#특징)
    - [IoC](#ioc)
    - [Container](#container)
    - [DI](#di)
  - [MVC(Model-View-Controller)](#mvcmodel-view-controller)
    - [Spring MVC](#spring-mvc)
    - [Spring MVC 실행 순서](#spring-mvc-실행-순서)
    - [Spring MVC 구현](#spring-mvc-구현)
  - [Controller](#controller)
    - [@Controller](#controller-1)
    - [@RequestMapping](#requestmapping)
    - [HTTP method](#http-method)
    - [Controller method의 parameter type](#controller-method의-parameter-type)
    - [@RequestBody parameter type](#requestbody-parameter-type)
    - [Servlet API 사용시기](#servlet-api-사용시기)
    - [Controller Class에서 method의 return type 종류](#controller-class에서-method의-return-type-종류)
  - [View](#view)
  - [Model](#model)
  - [Spring Web Application의 동작순서](#spring-web-application의-동작순서)

<br>

---

## 개요

### **Spring**의 정의

- **자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크**

  - 애플리케이션 프레임워크

    - 특정 계층이나 기술, 업무 분야에 국한되지 않고 애플리케이션의 전 영역을 포괄하는 범용적인 프레임워크

    - 애플리케이션의 전 영역을 관통하는 일관된 프로그래밍 모델과 핵심 기술을 바탕으로 각 분야의 특성에 맞는 필요를 채워주고 있기 때문에 빠르고 효율적으로 개발할 수 있음

  - 경량급

    - 불필요하게 무겁지 않다는 의미로써 기존 EJB과 비교하여 개발환경과 기술수준이 비슷하더라도 훨씬 빠르고 간편하게 작성할 수 있어 생산성과 품질 면에서 유리함

  - 자바 엔터프라이즈 개발을 편하게

    - 개발자가 복잡하고 실수하기 쉬운 로우레벨 기술에 많은 신경을 쓰지 않으면서도 애플리케이션의 핵심인 사용자의 요구사항, 즉 비즈니스 로직을 빠르고 효과적으로 구현하는 것

### **POJO**

- Plain Old Java Object

  - Spring의 POJO는 IoC/DI + AOP + PSA(Portable Service Abstraction) 를 통해 구현됨

  - 간단한 자바오브젝트라는 의미로써 특정 환경과 기술에 종속되지 않고 필요에 따라 재활용될 수 있는 방식으로 설계된 오브젝트

- 장점

  - 특정한 기술과 환경에 종속되지 않는 오브젝트는 깔끔한 코드가 될 수 있음

  - 유연한 방식으로 원하는 레벨에서 빠르고 명확하게 테스트할 수 있어 자동화된 테스트에 매우 유리함

### **IoC/DI**

- Inversion of Control / Dependency Injection

- 확장에는 열려 있고 변경에는 닫혀 있는 OCP(개방 폐쇄 원칙)으로 설명할 수 있음

- 유연하게 확장 가능한 객체를 만들어 두고 객체 간의 의존관계는 외부에서 다이나믹하게 설정

### **AOP**

- Aspect Oriented Programming

- 프록시를 통해 구현되며 관심사의 분리를 통해 소프트웨어의 모듈성을 향상시켜 공통 모듈을 여러 코드에 쉽게 적용할 수 있음

### **PSA**

- Portable Service Abstraction

- 기술의 변경과 환경에 관계없이 일관된 방식으로 서비스할 수 있게 하는 설계 원칙

<br>

[목차로 이동](#목차)

---

## 특징

- 경량컨테이너

  - 스프링은 자바객체를 담고 있는 컨테이너

  - 스프링 컨테이너는 자바 객체의 라이프사이클을 관리함

- POJO 지원

  - 특정 인터페이스를 구현하거나 또는 클래스를 상속하지 않는 일반 자바 객체 지원

- IoC (Inversion of Control - 제어의 역전)

  - 개발자에게 있던 객체 생성 및 의존 관계 제어권을 컨테이너에게 맡김

  - 객체의 생성과 생명주기를 관리 할 수 있어 스프링 컨테이너라고 부름

- DI 패턴 지원

  - 스프링은 설정 파일이나 어노테이션을 통해 객체 간 의존 관계를 설정할 수 있으므로 직접 생성하거나 검색할 필요가 없음

- AOP 지원

  - 문제를 바라보는 관점을 기준으로 프로그래밍하는 기법

  - 문제를 해결하기 위한 핵심관심 사항과 전체에 적용되는 공통관심 사항을 나누어 프로그래밍

  - 프록시 기반의 AOP로써 트랜잭션, 로깅, 보안과 같은 여러 모듈에서 공통으로 필요로 하지만 모듈의 핵심이 아닌 기능들을 분리하여 각 모듈에 적용

  - JMS, 메일, 스케쥴링 등 엔터프라이즈 애플리케이션 개발에 필요한 다양한 API를 설정 파일과 어노테이션을 통해 손쉽게 사용할 수 있음

<br>

[목차로 이동](#목차)

---

## IoC

- IoC 개념

  - 객체 간의 강한 결합을 다형성으로 인터페이스를 호출하여 결합도를 낮춤

  - 객체 간의 강한 결합을 팩토리를 호출하여 결합도를 낮춤

  - 객체 간의 강한 결합을 Assembler를 통해 결합도를 낮춤

- IoC 방법

  - Dependency Lookup

    > 컨테이너가 lookup context를 통해 필요한 리소스나 오브젝트를 얻는 방식

    - JNDI Lookup

  - Dependency Injection

    > 컨테이너가 직접 의존 구조를 오브젝트에 설정할 수 있도록 지정하는 방식

    - Setter Injection

    - Constructor Injection

    - Method Injection

<br>

[목차로 이동](#목차)

---

## Container

> 객체의 라이프사이클과 애플리케이션 사용에 필요한 주요 기능 제공

- 기능

  - 라이프사이클 관리

  - 의존 객체 제공

  - 스레드 관리

  - 애플리케이션 실행에 필요한 환경

- 필요성

  - 비즈니스 로직 외 부가 기능들에 대해서는 독립적으로 관리

  - 서비스 look up이나 Configuration에 대한 일관성

  - 서비스 객체를 사용하기 위해 각각 Factory 또는 Singleton 패턴을 직접 구현하지 않아도 됨

- IoC Container

  - 오브젝트의 생성과 관계설정, 사용, 제거 등의 작업을 애플리케이션 코드 대신 독립된 컨테이너가 담당

  - 스프링에서 IoC를 담당하는 컨테이너에는 BeanFactory, ApplicationContext가 있음

- DI Container

  - DI Container가 관리하는 객체를 빈(Bean)이라 하고, 빈들의 생명주기를 관리하는 의미로 BeanFactory라 함

    - 일반적으로 BeanFactory 보다는 이를 확장한 ApplicationContext를 사용함

    - getBean() 메서드가 정의되어 있음

  - BeanFactory에 여러 가지 컨테이너 기능을 추가하여 ApplicationContext라 함

    - Spring이 제공하는 ApplicationContext 구현 클래스는 여러가지 종류가 있음

<br>

[목차로 이동](#목차)

---

## DI

- 빈 생성범위

  범위|설명
  :--|:--
  singleton|컨테이너당 인스턴스 생성 (default)
  prototype|빈 요청시마다 인스턴스 생성
  request|HTTP Request별 인스턴스 생성
  session|HTTP Session별 인스턴스 생성

  ```java
  @Scope("prototype")
  public class UserServiceImpl implements UserService {
  }
  ```

  ```xml
  <bean id="userService" class="com.user.service.UserServiceImpl" scope="prototype"/>
  ```

- 스프링 빈 설정 메타정보

  - IoC Container

    - BeanDefinition meta data

      - XML Document

      - Annotation

      - Java Code

- 빈 설정 방법

  - XML
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi-schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

      <bean id="userDao" class="com.user.dao.UserDaoImpl"/>
      
      <bean id="userService" class="com.user.service.UserServiceImpl" scope="prototype">
        <property name="userDao" ref="userDao"/>
      </bean>
    </beans>
    ```
    ```java
    ApplicationContext context=new ClassPathXmlApplicationContext("com/user/applicationContext.xml");
    CommonService userService=context.getBean("userServcie", UserService.class);

    // Resource resource=new ClassPathResouce("com/user/applicationContext.xml");
    // BeanFactory factory=new XmlBeanFactory(resource);
    // CommonService userService=(UserService)factory.getBean("userService");
    ``` 

  - Annotation
    ```java
    @Component
    public class UserServiceImpl implements UserService {
      @Autowired
      private UserDao userDao;
    }
    ```
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi-schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

      <context:component-scan base-package="com.user.*"/>
    </beans>
    ``` 
    
    Stereotype|대상
    :--|:--
    @Repository|Data Access Layer의 DAO 또는 Repository 클래스에 사용
    @Service|Service Layer의 클래스에 사용
    @Controller|Presentation Layer의 MVC Controller에 사용
    @Component|Layer 구분을 적용하기 어려운 일반적인 경우에 사용

    - annotation을 구분하는 이유

      - 계층별로 빈의 특성이나 종류를 구분

      - AOP Pointcut 표현식을 사용하면 특정 annotation이 달린 클래스만 설정 가능

      - 특정 계층의 빈에 부가기능을 부여할 수 있음

<br>

[목차로 이동](#목차)

---

> ## MVC(Model-View-Controller)
>- MVC란?
>>- Application의 원활한 확장을 위해 Model, View, Controller 3가지 영역으로 분리함
>>- Component의 변경이 다른 영역 Component에 영향을 미치지 않아 유지보수에 용이함
>>- 컴포넌트 간의 결합성이 낮아 프로그램 수정이 용이하여 확장성이 뛰어남
>- 장점
>>- 화면과 비즈니스 로직을 분리하여 작업 가능 
>>- 영역별 개발로 인하여 확장성이 뛰어남
>>- 표준화된 코드를 사용하므로 공동작업이 용이하고 유지보수성이 좋음
>- 단점
>>- 개발과정이 다소 복잡하여 초기 개발속도가 늦음
>>- 초보자가 이해하고 개발하기에 다소 어려움
>- Model 역할
>>- Application 캡슐화
>>- 상태 쿼리에 대한 응답
>>- 어플리케이션의 기능 표현
>>- 변경을 view에 통지
>- View 역할
>>- Model을 시각적으로 표현
>>- Model에게 업데이트 요청
>>- 사용자의 입력을 Controller에 전달
>>- Controller가 View를 선택하도록 허용
>- Controller
>>- Application의 행위 정의
>>- 사용자 액션을 Model로 업데이트하고 mapping
>>- 응답에 대한 View 선택   
>- Model2 요청 흐름 시각화
> ![Model2 요청 흐름 이미지](./imgs/Model2_Architecture.PNG)

<br>

[목차로 이동](#목차)

> ### Spring MVC
>- 특징
>>- Spring은 DI, AOP기능 뿐만 아니라, Servlet 기반의 WEB 개발을 위한 MVC Framework를 제공함
>>- Model2 Architecture와 Front Controller Pattern을 Framework 차원에서 제공함
>>- Spring MVC Framework는 Spring을 기반으로 하고 있어 Spring이 제공하는 Transaction 처리나 DI, AOP등을 손쉽게 사용할 수 있음
>- 구성요소
>>- DispatcherServlet (Front Controller)
>>>- 모든 클라이언트의 요청을 처리함
>>>- Controller에게 클라이언트의 요청을 전달하고 Controller가 리턴한 결과값을 View에게 전달하여 알맞은 응답을 생성함
>>- HandlerMapping
>>>- 클라이언트의 요청 URL을 어떤 Controller가 처리할지 결정함
>>>- URL과 요청 정보를 기준으로 어떤 핸들러 객체를 사용할지 결정하는 객체이며, DispatcherServlet은 하나 이상의 핸들러 매핑을 가질 수 있음
>>- Controller
>>>- 클라이언트의 요청을 처리한 뒤, Model을 호출하고 그 결과를 DispatcherServlet에 알려줌
>>- ModelAndView
>>>- Controller가 처리한 데이터 및 화면에 대한 정보를 보유한 객체
>>- ViewResolver
>>>- Controller가 리턴한 View 이름을 기반으로 Controller의 처리 결과를 보여줄 View를 결정
>>- View
>>>- Controller의 처리결과를 보여줄 응답화면을 생성

<br>

[목차로 이동](#목차)

> ### Spring MVC 실행 순서
>> ![Spring MVC 요청 흐름](./imgs/SpringMVC_Request_Flow.PNG)
>>1. DispatcherServlet 이 요청을 수신
>>>- 단일 Front Controller Servlet 
>>>- 요청을 수신하여 처리를 다른 컴포넌트에 위임
>>>- 어느 Controller에 요청을 전송할지 결정
>>2. DispatherServlet이 HandlerMapping에 어느 Controller를 사용할 것인지 문의
>>>- URL과 Mapping
>>3. DispatcherServlet 은 요청을 Controller에게 전송하고 Controller는 요청을 처리한 후 결과 리턴
>>>- Business Logic 수행 후 결과 정보(Model)가 생성되어 JSP와 같은 View에서 사용됨
>>4. ModelAndView Object에 수행결과가 포함되어 DispatcherServelt에 리턴
>>5. ModelAndView 는 실제 JSP 정보를 갖고 있지 않으며, ViewResolver가 논리적 이름을 실제 JSP이름으로 변환함
>>6. View는 결과정보를 사용하여 화면을 표현함

<br>

[목차로 이동](#목차)

> ### Spring MVC 구현
>- Spring MVC를 이용한 Application 구현 순서
>>1. web.xml에 DispatcherServlet 등록 및 Spring 설정파일 등록
>>2. 설정 파일에 HandlerMapping 설정
>>3. Controller 구현 및 servlet-context.xml에 등록
>>4. Controller와 JSP의 연결을 위해 View Resolver 설정
>>5. JSP 코드 작성
>- Controller 작성
>>- Controller가 많은 일을 하지 않고 Service에 처리를 위임하도록 작성한다
>- web.xml -  DispatcherServlet 설정
>> ```xml
>> <servlet>
>>   <servlet-name>appServlet</servlet-name>
>>   <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
>>   <init-param>
>>     <param-name>contextConfigLocation</param-name>
>>     <param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
>>   </init-param>
>>   <load-on-startup>1</load-on-startup>
>> </servlet-name>
>> 
>> <servlet-mapping>
>>   <servlet-name>appServlet</servlet-name>
>>   <url-pattern>/</url-pattern>
>> </servlet-mapping>
>> ```
>>- \<init-param>을 설정하지 않으면 "\<servlet-name>-servlet.xml" 에서 applicationContext의 정보를 로드함
>>- 스프링 컨테이너는 설정파일의 내용을 읽고 ApplicationContext객체를 생성함
>>- \<url-pattern>에 DispatcherServlet이 처리하는 URL Mappling pattern을 정의함
>> ```xml
>> <servlet>
>>   <servlet-name>appServlet</servlet-name>
>>   <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
>>   <init-param>
>>     <param-name>contextConfigLocation</param-name>
>>     <param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
>>   </init-param>
>>   <load-on-startup>1</load-on-startup>
>> </servlet-name>
>> <servlet>
>>   <servlet-name>appServlet2</servlet-name>
>>   <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
>>   <init-param>
>>     <param-name>contextConfigLocation</param-name>
>>     <param-value>/WEB-INF/spring/appServlet/servlet-context2.xml</param-value>
>>   </init-param>
>>   <load-on-startup>1</load-on-startup>
>> </servlet-name>
>> 
>> <servlet-mapping>
>>   <servlet-name>appServlet</servlet-name>
>>   <url-pattern>*.do</url-pattern>
>> </servlet-mapping>
>> <servlet-mapping>
>>   <servlet-name>appServlet</servlet-name>
>>   <url-pattern>*.action</url-pattern>
>> </servlet-mapping>
>> ```
>>- 서블릿이므로 1개 이상의 DispatcherServlet 설정 가능
>>>- 각 DispatcherServlet마다 각각의 ApplicationContext 생성
>>- \<load-on-startup>1\</load-on-startup> 설정 시 WAS startup 될때 초기화 작업이 진행됨
>- web.xml - 최상위 Root ContextLoader 설정
>>- Context 설정 파일들을 로드하기 위해 web.xml 파일에 리스너 설정
>> ```xml
>> <listener>
>>   <listener-class>org.springframework.context.ContextLoaderListener</listener-class>
>> </listener>
>> ```
>>- 리스너 설정시 /WEB-INF/spring/root-context.xml 파일을 읽어 공통적으로 사용되는 최상위 Context를 생성
>>- 그 외의 다른 컨텍스트 파일들을 최상위 어플리케이션 컨텍스트에 로드하는 방법
>>> ```xml
>>> <context-param>
>>>   <param-name>contextConfigLocation</param-name>
>>>   <param-value>
>>>     /WEB-INF/spring/root-context.xml
>>>     classpath:com/test/web/application.xml <!-- 해당 클래스 패스에 위치한 설정파일로부터 다른 컨텍스트 파일 로드 -->
>>>   </param-value>
>>> </context-param>
>>> ```
>>> ```xml
>>> <!-- com/test/web/application.xml -->
>>> <context-param>
>>>   <param-name>contextConfigLocation</param-name>
>>>   <param-value>
>>>     /WEB-INF/spring/root-*.xml
>>>   </param-value>
>>> </context-param>
>>> ```
>- Application Context 분리
>>- 어플리케이션 레이어에 따라 어플리케이션 컨텍스트를 분리
>>> Layer|설정파일
>>> :--|:--
>>> Security|board-security.xml
>>> Web|board-servlet.xml
>>> Service|board-service.xml
>>> Persistence|board-dao.xml
>- Controller Class 작성
>> ```java
>> @Slf4j
>> @Controller
>> public class HomeController{
>>   //@GetMapping({"/", "/index"})
>>   @RequestMapping(value={"/", "/index"}, method=RequestMethod.GET)
>>   public String home(Model m) {
>>     log.info("방문요청");
>>     m.addAttribute("msg","방문환영");
>>     return "index";
>>   }
>> }
>> ```
>- Context 설정파일에 Controller 등록 (servlet-context.xml)
>> ```xml
>> <beans:bean class="com.text.web.HomeController" />
>> ```
>- Controller와 response page 연결을 위한 ViewResolver 설정 (servlet-context.xml)
>> ```xml
>> <beans:bean class="com.text.web.HomeController" />
>> 
>> <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
>>   <beans:property name="prefix" value="/WEB-INF/views/" />
>>   <beans:property name="suffix" value=".jsp" />
>> </beans:bean>
>> ```

<br>

[목차로 이동](#목차)

---

>## Controller
>- @Controller와 @RequestMapping 어노테이션
>>- 메서드 단위의 mapping 가능
>>- DefaultAnnotationHandlerMapping과 AnnotationHandlerAdapter를 사용
>>>- Spring 3.0부터 기본 설정이므로 별도의 설정없이 사용가능

<br>

[목차로 이동](#목차)

>### @Controller
>- Controller class는 Client의 요청을 처리함
>- Class 타입에 적용되며 Spring 3.0부터 @Controller 사용이 권장됨
>- 세부 적용
>>- Controller Class를 <bean>에 등록
>>> ```xml
>>> <beans:bean id="userController" class="com.movie.user.controller.UserController">
>>>  <beans:property name="userService" ref="userService"/>
>>> </beans:bean>
>>> ```
>>- Controller Class 자동스캔
>>> ```xml
>>> <context:component-scan base-package="com.movie.user.controller" />
>>> ```
>>>- context:component-scan 의 base-package에 설정된 패키지내의 class중 @Controller가 적용된 클래스는 자동스캔 대상이 됨

<br>

[목차로 이동](#목차)

>### @RequestMapping
>>- 요청 URL mapping 정보를 설정하며 클래스타입과 메소드에 설정할 수 있음
>> ```java
>> @Controller
>> @RequestMapping("/user")
>> public class UserController {
>>  private UserService userService;
>>  
>>  //@RequestMapping("/user/regist.do")
>>  @RequestMapping("/regist.do")
>>  public String regist() {
>>    return "user/regist";
>>  }
>> }
>> ```

<br>

[목차로 이동](#목차)

>### HTTP method
>>- 같은 URL 요청에 대하여 HTTP method(GET, POST..)에 따라 다른 메서드 mapping 가능
>> ```java
>> @Controller
>> @RequestMapping("/user")
>> public class UserController {
>>  private UserService userService;
>>  
>>  @RequestMapping("/regist.do", method=RequestMethod.GET)
>>  public String regist() {
>>    return "user/regist";
>>  }
>>
>>  @RequestMapping("/regist.do", method=RequestMethod.POST)
>>  public String regist2() {
>>    return "user/regist";
>>  }
>> }
>> ```
>>- Controller class에 매핑이 되어있는데 메서드에 매핑설정이 없으면 HTTP ERROR 404
>> ```java
>> @Controller
>> @RequestMapping("/user")
>> public class UserController {
>>  private UserService userService;
>>  
>>  @RequestMapping(method=RequestMethod.GET) // HTTP ERROR 404 (Not Found)
>>  public String regist() {
>>    return "user/regist";
>>  }
>> }
>> ```

<br>

[목차로 이동](#목차)

>### Controller method의 parameter type
>- Controller method의 parameter로 다양한 Object를 받을 수 있음
>> Parameter Type|설명
>> :--|:--
>> HttpServletRequest|
>> HttpServletResponse|필요시 Servlet API를 사용할 수 있음
>> HttpSession|
>> Java.util.Locale|현재 요청에 대한 위치관련정보를 사용할 수 있음
>> InputStream, Reader|요청 컨텐츠에 직접 접근할 때 사용
>> OutputStream, Writer|응답 컨텐츠를 생성할 때 사용
>> @PathVariable annotation 적용 파라미터|URI 템플릿 변수에 접근할 때 사용
>> @RequestParam annotation 적용 파라미터|HTTP 요청 파라미터를 매핑
>> @RequestHeader annotation 적용 파라미터|HTTP 요청 헤더를 매핑
>> @CookieValue annotation 적용 파라미터|HTTP 쿠키 매핑
>> @RequestBody annotation 적용 파라미터|HTTP 요청의 body 내용에 접근할 때 사용
>> Map, Model, ModelMap|view에 전달할 model data를 설정할 때 사용
>> 커맨드 객체(DTO)|HTTP 요청 parameter를 저장한 객체
>> &nbsp;|기본적으로 클래스 이름을 모델명으로 사용
>> &nbsp;|@ModelAttribute annotation 설정으로 모델명을 설정할 수 있음
>> Errors, BindingResult|HTTP 요청 파라미터를 커맨더 객체에 저장한 결과
>> &nbsp;|커맨드 객체를 위한 파라미터 바로 다음에 위치
>> SessionStatue|폼 처리를 완료 했음을 처리하기 위해 사용
>> &nbsp;|@SessionAttributes annotation을 명시한 session 속성을 제거하도록 이벤트 발생
>- @RequestParam annotation을 이용한 parameter mapping
>> ```java
>> @Controller
>> public class UserController {
>>  private UserService userService;
>>  
>>  // URL 호출 : http://localhost/user/regist?name=홍길동&age=30
>>
>>  @RequestMapping(value="/regist" method=RequestMethod.GET)
>>  //public String regist(@RequestParam("name") String name, @RequestParam("age") int age, Model model) {
>>  public String regist(@RequestParam(value="name", required=false) String name, @RequestParam(value="age", defaultValue="25") int age, Model model) {
>>    model.addAttribute("msg", name+"("+age+")님 환영합니다");
>>    return "index";
>>  }
>> }
>> ```
>- form에 입력한 data를 JavaBean 객체를 이용하여 전송할 수 있고 List로도 가능함
>> ```html
>> <form method="POST" action="${root}/user/regist">
>>   이름: <input type="text" name="name"><br>
>>   나이: <input type="nubmer" name="age"><br>
>>   <input type="submit" value="회원가입">
>></form>
>> ```
>> ```java
>> @Controller
>> @RequestMapping("/user")
>> public class UserController {
>>   @RequestMapping(value="/regist", method=RequestMethod.POST)
>>   public String regist(UserDto userDto) {
>>     return "user/regist_result";
>>   }
>> }
>> ```
>> ```java
>> public class UserDto {
>>   private String name;
>>   private int age;
>> 
>>   //setters
>> }
>> ```
>- View에서 Command 객체에 접근
>>- Command 객체는 자동으로 반환되는 Model에 추가되고, view에서 접근이 가능함
>- @CookieValue annotation을 이용한 Cookie mapping
>> ```java
>> @Controller
>> public class HomeController {
>>   //public String hello(@CookieValue("author") String authorValue) {
>>   public String hello(@CookieValue(value="author", required=false, defaultValue="user") String authorValue) {
>>     return "ok";
>>   }
>> }
>> ```
>- @RequestHeader annotation을 이용한 header mapping
>> ```java
>> @Controller
>> public class HomeController {
>>   public String hello(@RequestHeader("Accept-Language") String authorValue) {
>>     return "ok";
>>   }
>> }
>> ```

<br>

[목차로 이동](#목차)

>### @RequestBody parameter type
>- HTTP 요청 Body가 그대로 객체에 전달됨
>- AnnotationMethodHandlerAdapter에는 HttpMessageConverter 타입의 메시지 변환기가 기본으로 여러개 등록되어 있음
>- @RequestBody가 붙은 parameter가 있으면 해당 미디어 타입을 확인 후 처리 가능한 변환기가 자동으로 객체로 변환시켜 줌
>- 주로 @ResponseBody와 함께 사용되며 비동기처리됨

<br>

[목차로 이동](#목차)

>### Servlet API 사용시기
>- HttpSession의 생성을 직접 제어해야 하는 경우
>- Controller가 Cookie를 생성해야 하는 경우
>- Servlet API를 선호하는 경우
>- Servlet API의 종류
>>- javax.servlet.ServletRequest / javax.servlet.http.HttpServletRequest
>>- javax.servlet.ServletResponse / javax.servlet.http.HttpServletResponse
>>- javax.servlet.http.HttpSession
> ```java
> @Controller
> public class HomeController {
>   public String hello(HttpServletRequest request, HttpServletResponse response) {
>     return "ok";
>   }
> }
> ```

<br>

[목차로 이동](#목차)

>### Controller Class에서 method의 return type 종류
> Return Type|설명
> :--|:--
> ModelAndView|model 정보 및 view 정보를 담고있는 객체
> Model|view에 전달할 객체 정보를 담고있는 Model을 반환한다. view이름은 요청 URL로부터 결정됨(RequestToViewNameTranslator)
> Map|view에 전달할 객체 정보를 담고있는 Map을 반환한다. view이름은 요청 URL로부터 결정됨(RequestToViewNameTranslator)
> String|view의 이름을 반환함
> View|view 객체를 직접 리턴, 해당 View 객체를 이용해서 view를 생성함
> void|method가 ServletResponse나 HttpServletResponse타입의 parameter를 갖는 경우 method가 직접 응답을 처리한다고 가정한다. 그렇지 않을 경우 요청 URL로부터 결정된 View를 보여준다 (RequestToViewNameTranslator)
> @ResponseBody Annotation 적용|method에서 @ResponseBody annotation이 적용된 경우, 리턴 객체를 HTTP 응답으로 전송함. HttpMessageConverter를 이용해서 객체를 HTTP 응답 스트림으로 변환함

<br>

[목차로 이동](#목차)

---

>## View
>- View 지정
>>- Controller에서는 처리결과를 보여줄 View 이름이나 객체를 리턴하고, DispatcherServlet은 View이름이나 View객체를 이용하여 view를 생성
>- ViewResolver
>>- 논리적 view와 실제 JSP파일 mapping
>>- servlet-context.xml
>>>```xml
>>><beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
>>>  <beans:property name="prefix" value="/WEB-INF/views/" />
>>>  <beans:property name="suffix" value=".jsp" />
>>></beans:bean>
>>>```
>- View 이름 명시적 지정
>>- ModelAndView 와 String 리턴 타입
>>>```java
>>> @Controller
>>> public class HomeController {
>>>   @RequestMapping("/hello")
>>>   public ModelAndView hello(){
>>>     ModelAndView mav=new ModelAndView("hello");
>>>     return mav;
>>>   }
>>>   @RequestMapping("/hello")
>>>   public ModelAndView hello(){
>>>     ModelAndView mav=new ModelAndView();
>>>     mav.setViewNmae("hello");
>>>     return mav;
>>>   }
>>>   @RequestMapping("/hello")
>>>   public String hello(){
>>>     return "hello";
>>>   }
>>> }
>>>```
>- View 이름 자동 지정
>>- RequestToViewNameTranslator를 이용하여 URL로 부터 view이름을 결정
>>- 자동 지정 유형
>>>- return type이 Model이나 Map인 경우
>>>- return type이 void이면서 ServletResponse나 HttpServletResponse 타입의 parameter가 없는 경우
>>>```java
>>> @Controller
>>> public class HomeController {
>>>   @RequestMapping("/hello") // hello가 view 이름이 됨
>>>   public Map<String, Object> hello(){
>>>     Map<String, Object> model=new HashMap<String, Object>();
>>>     return model;
>>>   }
>>> }
>>>```
>- redirect view
>>- View 이름에 "redirect:" 접두어를 붙이면 지정한 페이지로 리다이랙트
>>>```java
>>> @Controller
>>> public class HomeController {
>>>   @RequestMapping("/hello")
>>>   public Map<String, Object> hello(){
>>>     return "redirect:index";
>>>   }
>>> }
>>>```

<br>

[목차로 이동](#목차)

---

>## Model
>- View에 전달하는 데이터
>>- @RequestMapping annotation이 적용된 method의 Map, Model, ModelMap
>>> ```java
>>> @Controller
>>> public class HomeController {
>>>   @RequestMapping("/hello")
>>>   public String hello(Map model){
>>>   //public String hello(ModelMap model){
>>>   //public String hello(Model model){
>>>     model.put("msg", "어서오세요");
>>>     return "hello";
>>>   }
>>> }
>>> ```
>>>- Model Interface 주요 method
>>>>- Model addAttribute(String name, Object value);
>>>>- Model addAttribute(Object value);
>>>>- Model addAllAttributes(Collection<?> values);
>>>>- Model addAllAttributes(Map<String, ?> attributes);
>>>>- Model mergeAttributes(Map<String, ?> attributes);
>>>>- boolean containsAttribute(String name);
>>- @RequestMapping method가 return하는 ModelAndView
>>>- Controller에서 처리결과를 보여줄 view와 view에 전달할 값(model)을 저장하는 용도로 사용
>>>>```java
>>>> @Controller
>>>> public class HomeController {
>>>>   @RequestMapping("/hello")
>>>>   public ModelAndView hello(){
>>>>     ModelAndView mav=new ModelAndView();
>>>>     mav.setViewName("hello");
>>>>     mav.addObject("msg", "어서오세요");
>>>>     return mav;
>>>>   }
>>>> }
>>>>```
>>- @ModelAttribute annotation이 적용된 method가 return 한 객체
>>>- @RequestMapping annotation이 적용되지 않은 별도 method로 모델이 추가될 객체를 생성
>>>> ```java
>>>> @Controller
>>>> public class HomeController {
>>>>   @ModelAttribute("modelmsg")
>>>>   public String msg() {
>>>>     return "bye";
>>>>   }
>>>>   
>>>>   @RequestMapping("/hello")
>>>>   public String hello(Map model){
>>>>     model.put("msg", "어서오세요");
>>>>     return "hello";
>>>>   }
>>>> }
>>>> ```
>>>> ```html
>>>> <div align="center">
>>>> <h3>hello</h3>
>>>> ${msg}<br>
>>>> ${modelmsg}<br>
>>>> <a href="${root}/user/regist">회원가입</a>
>>>> </div>
>>>> ```
>- 요청 URL 매칭
>>- 전체 경로와 Servlet 기반 경로 매칭
>>>- DispatcherServlet은 DefaultAnnotationHandlerMapping Class를 기본으로 HandlerMapping 구현체로 사용
>>>- Default로 Context내의 경로가 아닌 Servlet 경로를 제외한 나머지 경로에 대해 mapping
>>>```xml
>>><servlet>
>>>  <servlet-name>dispatcher</servlet-name>
>>>  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
>>>  <load-on-startup>1</load-on-startup>
>>></servlet>
>>><servlet-mapping>
>>>  <servlet-name>dispatcher</servlet-name>
>>>  <url-pattern>*.html</url-pattern>
>>>  <url-pattern>/product/*</url-pattern>
>>></servlet-mapping>
>>>```
>>- Servlet 기반 경로 매칭
>>>- Servlet 경로를 포함한 전체 경로를 이용해서 매칭하려는 경우
>>>- @RequestMapping("/user/regist")
>>>>```xml
>>>><bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping">
>>>>  <property name="alwaysUseFullPath" value="true" />
>>>></bean>
>>>>
>>>><bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
>>>>  <property name="alwaysUseFullPath" value="true" />
>>>></bean>
>>>>```
>>- @PathVariable annotation을 이용한 URI 템플릿
>>>- RESTful 방식
>>>>- **http**://localhost/users/troment
>>>>- **http**://localhost/product/
>>>>- **http**://localhost/forum/board/10
>>>- @RequestMapping Annotation 값으로 {템플릿변수}를 사용
>>>- @PathVariable Annotation을 이용해서 {템플릿변수}와 동일한 이름을 갖는 parameter를 추가
>>>>```java
>>>> @Controller
>>>> public class BoardViewController {
>>>>   @RequestMapping("/{id}/board/{num}")
>>>>   public String view(@PathVariable String id, @PathVariable int num, Model model){
>>>>     Board board=boardService.getBoard(id, num);
>>>>     return "view";
>>>>   }
>>>> }
>>>>```
>>>>```java
>>>> @Controller
>>>> @RequestMapping("/{id}")
>>>> public class BoardViewController {
>>>>   @RequestMapping("/board/{num}")
>>>>   public String view(@PathVariable String id, @PathVariable int num, Model model){
>>>>     Board board=boardService.getBoard(id, num);
>>>>     return "view";
>>>>   }
>>>> }
>>>>```
>>>- @RequestMapping Annotation의 추가설정 방법
>>>>- Ant 스타일의 URI패턴 지원
>>>>>- ? : 하나의 문자열과 대치
>>>>>- * : 하나 이상의 문자열과 대치
>>>>>- ** : 하나 이상의 디렉토리와 대치
>>>>>```java
>>>>>@RequestMapping("/member/*.do")
>>>>>@RequestMapping("/*/board/{num}")
>>>>>```

<br>

[목차로 이동](#목차)

---

>## Spring Web Application의 동작순서
>1. 웹 어플리케이션이 실행되면 WAS(Tomcat)에 의해 web.xml이 로딩
>2. web.xml에 등록되어 있는 ContextLoaderListener (Java Class)가 생성되고 ContextLoaderListener class는 ServletContextListener interface를 구현하고 있으며, ApplicationContext를 생성하는 역할을 수행
>3. 생성된 ContextLoaderListener는 root-context.xml을 로딩
>4. root-context.xml에 등록되어 있는 스프링 컨테이너가 구동되어 개발자가 작성한 비지니스 로직(Service)에 대한 부분과 DAO, VO 객체들이 생성
>5. 클라이언트로부터 request
>6. DispatcherServlet이 생성되며 FrontController의 역할을 수행
>>- 클라이언트의 요청을 분석하여 알맞은 컨트롤러에게 전달하고 응답을 받아 요청에 따른 응답을 어떻게 할 지 결정
>>- HandlerMapping, ViewResolver Class라고 부름
>7. DispatcherServlet은 servlet-context.xml을 로딩
>8. 두번째 스프링 컨테이너가 구동되며 응답에 맞는 컨트롤러들이 동작
>>- 첫번쨰 스프링 컨테이너가 구동되면서 생성된 Service, DAO, VO 클래스들과 협업하여 작업처리

<br>

[목차로 이동](#목차)

---