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