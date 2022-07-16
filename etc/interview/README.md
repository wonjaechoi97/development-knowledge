# **기술 면접 빈출 질문**

### 참고자료

- [CS study Repo](https://github.com/SSAFY-CS-STUDY/Tech_interview)

## 목차
>- [Java](#java)
>- [Operating System](#operating-system)
>- [Network](#network)
>- [Database](#database)


<br>

---

## Java

>### Java문제
>[[답변]](#java답변)
>- 객체지향 프로그래밍과 절차지향 프로그래밍의 차이에 대해 설명하시오
>- 객체지향 프로그래밍의 특징에 대해 설명하시오
>- 객체지향 설계 5원칙에 대해 설명하시오
>- 오버로딩과 오버라이딩에 대해 설명하시오
>- 클래스와 객체의 차이를 설명하시오
>- 객체와 인스턴스의 차이를 설명하시오
>- 자바의 특징에 대해 설명하시오
>- Java SE와 EE의 차이를 설명하시오
>- 참조형 변수와 기본형 변수의 차이를 설명하시오
>- 변수의 3가지 타입에 대해 설명하시오
>- Wrapper Class에 대해 설명하시오
>- public 접근 제어자와 private 접근 제어자의 차이를 설명하시오
>- Boxing, Unboxing에 대해 설명하시오
>- non-static 멤버와 static 멤버의 차이를 설명하시오
>- main 메서드가 public static 인 이유를 설명하시오
>- final 키워드의 용도에 대해 설명하시오
>- Generic에 대해 설명하시오
>- ==과 equals()의 차이를 설명하시오
>- Call by Reference와 Call by Value에 대해 설명하시오
>- 추상 클래스와 인터페이스의 차이에 대해 설명하시오
>- java reflection에 대해 설명하시오
>- StringBuilder와 StringBuffer의 차이를 설명하시오
>- Java 8에 추가된 기능을 설명하시오
>- Lambda란 무엇이고 어떠한 장점이 있는지 설명하시오
>- Stream API 특징이나 장점에 대해 설명하시오
>- Garbage Collector(GC)에 대해 설명하시오
>- GC에 의해 변수가 초기화되는 시점을 설명하시오
>- JAVA의 바이트코드에 대해 설명하시오
>- 예외처리 방법을 설명하시오
>- JAVA 동작 원리에 대해 설명하시오
>- 자바에서 쓰레드를 구현하기 위한 2가지 방법을 간단하게 설명하시오
>- Collection 정의와 종류를 설명하시오
>- ArrayList와 LinkedList의 차이를 설명하시오
>- CheckedException과 UnCheckedException의 차이를 설명하시오
>- Synchronized(동기화)를 하기 위한 방법을 설명하시오
>- try-with-resource에 대해 설명하시오
>- Functional Interface에 대해 설명하시오
>- Method Reference에 대해 설명하시오
>- Optional 클래스에 대해 설명하시오
>- 업캐스팅과 다운캐스팅의 차이를 설명하시오
>- this 키워드는 언제 사용되는지 설명하시오

<br>

[목차로 이동](#목차)

>### Java답변
>[[문제]](#java문제)
>- 객체지향 프로그래밍과 절차지향 프로그래밍의 차이에 대해 설명하시오
>>- 절차지향은 데이터 중심의 함수를 구현하여 프로그램을 순차적으로 처리하는 방식이며 속도가 빠른 장점이 있으나 코드의 변경이 이루어질때 프로젝트의 규모가 클수록 유지보수에 어려움이 있습니다
>>- 객체지향은 기능 중심으로 데이터와 기능을 묶어서 개발하는 방식이며 코드의 재활용성이 높다는 장점이 있으나 설계하는 데 시간이 오래걸립니다
>- 객체지향 프로그래밍의 특징에 대해 설명하시오
>>- 캡슐화, 상속, 추상화, 다형성 4개의 특징이 있음
>>- 캡슐화는 연관 있는 변수와 함수를 클래스로 묶는 작업을 의미함
>>- 상속은 자식 클래스가 부모 클래스의 특성과 기능을 물려받아 재사용성을 높이는 역할을 함
>>- 추상화란 인터페이스로 공통적인 특성들을 묶어 표현하는 것을 의미함
>>- 다형성은 같은 메서드명인데 여러 상황에 맞추어 다른 기능을 수행하게 하는것으로 오버로딩과 오버라이딩이 대표적인 예시임
>- 객체지향 설계 5원칙에 대해 설명하시오
>>- SOLID, SRP(단일 책임 원칙), OCP(개방-폐쇄 원칙), LSP(리스코프 치환 원칙), ISP(인터페이스 분리 원칙), DIP(의존 역전 원칙), 시간이 지나도 유지 보수와 확장이 쉬운 소프트웨어를 만드는데 이 원칙을 적용함
>>- Single Responsibility Principle
>>>- 객체는 단 하나의 책임만 가져야한다
>>>- 시스템에 변화가 생기더라도 영향을 최소화할 수 있음
>>- Open-Close Principle
>>>- 기존의 코드를 변경하지 않으면서, 기능을 추가할 수 있도록 설계되어야 한다
>>>- 확장에 대해서는 개방적이고 수정에 대해서는 폐쇄적으로 설계되어야 한다는 의미
>>- Liskov's Substitution Principle
>>>- 자식 클래스는 최소한 자신의 부모 클래스에서 가능한 행위는 수행할 수 있어야 한다는 설계 원칙
>>- Interface Segregation Principle
>>>- 자신이 사용하지 않는 인터페이스는 구현하지 말아야 한다는 설계 원칙
>>- Dependency Inversion Principle
>>>- 객체들이 서로 정보를 주고 받을 때 의존 관계가 형성되는데, 이 때 객체들은 나름대로의 원칙을 갖고 정보를 주고 받아야 한다는 설계 원칙
>- 오버로딩과 오버라이딩에 대해 설명하시오
>>- 오버로딩은 메서드의 이름은 같고, 매개변수의 개수나 타입이 다른 함수를 정의하는 것
>>- 오버라이딩은 상위 클래스의 메서드를 하위 클래스에서 재정의 하는것
>- 클래스와 객체의 차이를 설명하시오
>>- 클래스는 설계도, 객체는 설계도로 구현한 모든 대상을 의미함
>- 객체와 인스턴스의 차이를 설명하시오
>>- 클래스의 타입으로 선언되었을 때 객체라고 부르고, 그 객체가 메모리에 할당되어 실제 사용될 때 인스턴스라고 함
>- 자바의 특징에 대해 설명하시오
>>- 
>- Java SE와 EE의 차이를 설명하시오
>- 참조형 변수와 기본형 변수의 차이를 설명하시오
>- 변수의 3가지 타입에 대해 설명하시오
>- Wrapper Class에 대해 설명하시오
>- public 접근 제어자와 private 접근 제어자의 차이를 설명하시오
>- Boxing, Unboxing에 대해 설명하시오
>- non-static 멤버와 static 멤버의 차이를 설명하시오
>- main 메서드가 public static 인 이유를 설명하시오
>- final 키워드의 용도에 대해 설명하시오
>- Generic에 대해 설명하시오
>- ==과 equals()의 차이를 설명하시오
>- Call by Reference와 Call by Value에 대해 설명하시오
>- 추상 클래스와 인터페이스의 차이에 대해 설명하시오
>- java reflection에 대해 설명하시오
>- StringBuilder와 StringBuffer의 차이를 설명하시오
>- Java 8에 추가된 기능을 설명하시오
>- Lambda란 무엇이고 어떠한 장점이 있는지 설명하시오
>- Stream API 특징이나 장점에 대해 설명하시오
>- Garbage Collector(GC)에 대해 설명하시오
>- GC에 의해 변수가 초기화되는 시점을 설명하시오
>- JAVA의 바이트코드에 대해 설명하시오
>- 예외처리 방법을 설명하시오
>- JAVA 동작 원리에 대해 설명하시오
>- 자바에서 쓰레드를 구현하기 위한 2가지 방법을 간단하게 설명하시오
>- Collection 정의와 종류를 설명하시오
>- ArrayList와 LinkedList의 차이를 설명하시오
>- CheckedException과 UnCheckedException의 차이를 설명하시오
>- Synchronized(동기화)를 하기 위한 방법을 설명하시오
>- try-with-resource에 대해 설명하시오
>- Functional Interface에 대해 설명하시오
>- Method Reference에 대해 설명하시오
>- Optional 클래스에 대해 설명하시오
>- 업캐스팅과 다운캐스팅의 차이를 설명하시오
>- this 키워드는 언제 사용되는지 설명하시오

<br>

[목차로 이동](#목차)

---

## Operating System

>### Operating System문제
>[[답변]](#operating-system답변)
>- dd

<br>

[목차로 이동](#목차)

>### Operating System답변
>[[문제]](#operating-system문제)
>- dd

<br>

[목차로 이동](#목차)

---

## Network

>### Network문제
>[[답변]](#network답변)
>- dd

<br>

[목차로 이동](#목차)

>### Network답변
>[[문제]](#network문제)
>- dd

<br>

[목차로 이동](#목차)

---

## Database

>### Database문제
>[[답변]](#database답변)
>- dd

<br>

[목차로 이동](#목차)

>### Database답변
>[[문제]](#database문제)
>- dd

<br>

[목차로 이동](#목차)

---