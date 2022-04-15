# **Junit** ✅

### 참고하면 좋은자료

- Junit 4 Wiki [[링크](https://github.com/junit-team/junit4/wiki)]

- Unit Testing with JUnit4 Tutorial [[링크](https://www.vogella.com/tutorials/JUnit4/article.html)]

- JUnit 5 User Guide [[링크](https://junit.org/junit5/docs/current/user-guide/)]

## 목차

  - [JUnit4 개요](#junit4-개요)

  - [테스트 용어](#테스트-용어)

  - [테스트 정의방법과 예시](#테스트-정의방법과-예시)

  - [테스트 메서드 어노테이션](#테스트-메서드-어노테이션)

  - [테스트 결합](#테스트-결합)

  - [테스트 비활성화](#테스트-비활성화)

  - [매개변수화된 테스트](#매개변수화된-테스트)

  - [JUnit 규칙](#junit-규칙)

  - [정적인 JUnit 사용](#정적인-junit-사용)

<br>

---

## JUnit4 개요

- JUnit 이란
  
  - Java 유닛 테스트 프레임워크

  
<br>

[목차로 이동](#목차)

---

## 테스트 용어

- 픽스처 (fixture)
  
  > 테스트 실행을 위한 개체 집합의 고정 상태

- 테스트의 종류

  - 단위 테스트 (UnitTest)

    > 특정기능, 클래스 등 작은단위의 코드를 대상으로 한다   
    인터페이스나 컴포넌트 관계를 테스트하기에는 적합하지 않으므로 Mock 객체로 외부 의존성을 끊음

  - 통합 테스트 (IntegrationTest)

    > 컴포넌트들을 통합테스트한다   
    시스템이 의도한대로 작동하는지 테스트함

  - 성능 테스트 (PerfomanceTest)

    > 성능 테스트, 부하를 견딜수 있는지 테스트함

- 행동 및 상태 테스트

  - 행동 테스트 (BehaviorTest)

    - 특정 메서드가 올바른 입력 매개변수로 호출되었는지 확인함

    - 테스트 중인 애플리케이션의 동작을 테스트함

    - 메서드 호출의 결과를 검증하지 않음

  - 상태 테스트 (StateTest)

    - 메서드 호출의 결과를 검증함

- 테스트 클래스 및 메서드 명명 규칙

  - 클래스 명명 규칙

    - 클래스의 이름 끝에 Test 또는 Tests 접미사를 사용

  - 메서드 명명 규칙

    - 메서드의 이름에 should를 사용하여 어떤 일이 일어나야하는 지에 대한 힌트를 제공

      > ordersShouldBeCreated 또는 menuShouldGetActive

    - 메서드의 이름을 given[입력설명]When[행동설명]Then[기대되는결과] 양식으로 작성

    - JUnit5 에선 @DisplyName 어노테이션으로 추가설명이 가능하기 때문에 덜 중요한 규칙임

- 모의 객체 (Mock Object)

  - 단위 테스트에서 특정 기능을 격리하여 테스트할때 인터페이스 또는 클래스에 대한 더미 구현으로써 특정 메서드의 호출의 출력을 정의할 수 있음

<br>

[목차로 이동](#목차)

---

## 테스트 정의방법과 예시

- JUnit 테스트 정의 방법

  - 메서드의 이름에 @Test 어노테이션을 사용

    ```java
    public class Calculator {
      public int multiply(int a, int b){ return a*b; }
    }
    ```

    ```java
    import static org.junit.Assert.assertEquals;

    import org.junit.Before;
    import org.junit.Test;

    public class CalculatorTest {
      private Calculator calculator;

      @Before
      void setUp() thorws Exception {
        calculator=new Calculator();
      }

      @Test
      void testMultiply() {
        assertEquals("Regular multiplication should work", calculator.multiply(4,5), 20);
      }

      @Test
      void testMultiplyWithZero() {
        assertEquals("Multiple with zero should be zero", 0, calculator.multiply(0,5));
        assertEquals("Multiple with zero should be zero", 0, calculator.multiply(5,0));
      }
    }
    ```

<br>

[목차로 이동](#목차)

---

## 테스트 메서드 어노테이션

- 테스트 메서드 정의

  JUnit 4|설명
  :--|:--
  import org.junit.*|JUnit 어노테이션을 사용하기위한 import 문
  @Test|테스트 메서드 설정
  @Before|각 테스트 실행 전 실행되는 메서드 설정 (픽스처 초기화, 입력 데이터 읽기 등)
  @After|각 테스트 실행 후 실행되는 메서드 설정 (픽스처 초기화, 임시 데이터 삭제 등)
  @BeforeClass|모든 테스트가 시작되기 전 딱 한번 실행되는 메서드 설정 (DB연결) static으로 정의해야함
  @AfterClass|모든 테스트가 완료된 후 딱 한번 실행되는 메서드 설정 (DB연결해제) static으로 정의해야함
  @Ignore / @Ignore("이유")|테스트 비활성화 설정
  @Test(expected=Exception.class)|설정된 예외를 throw 하지않으면 실패
  @Test(timeout=100)|100밀리초보다 오래걸리면 실패

- 조건 검사

  메서드|설명
  :--|:--
  fail([message])|실패리턴
  assertTrue([message], boolean condition)|조건이 참이면 성공
  assertFalse([message], boolean condition)|조건이 거짓이면 성공
  assertEquals([message], expected, actual)|두 값이 같으면 성공
  assertEquals([message], expected, actual, 허용 오차)|float 또는 double 허용오차 내로 같으면 성공
  assertNull([message], object)|null일경우 성공
  assertNotNull([message], object)|null이 아니면 성공
  assertSame([message], expected, actual)|같은 개체를 참조하면 성공
  assertNotSame([message], expected, actual)|다른 개체를 참조하면 성공

<br>

[목차로 이동](#목차)

---

## 테스트 결합

- 여러 테스트 클래스를 하나의 테스트 모음으로 결합할 수 있음

  >- 테스트를 결합하면 해당 결합의 모든 테스트 클래스가 지정된 순서대로 실행

  ```java
  import org.junit.runner.RunWith;
  import org.junit.runnes.Suite;
  import org.junit.runnes.Suite.SuiteClasses;

  @RunWith(Suite.class)
  @SuiteClasses({
    MyClassTest.class,
    MySecondClassTest.class })
  public class AllTests {

  }

  ```

## 테스트 비활성화

- @Ignore 주석을 사용하여 테스트를 정적으로 무시할 수 있음

  > 테스트 조건 정의 방법
  >>- Assume.assumeFalse
  >>- Assume.assumeTrue

  ```java
  Assume.assumeFalse(System.getProperty("os.name").contains("Linus"));
  ```

  > 위 코드는 Linux에서 테스트를 비활성화

<br>

[목차로 이동](#목차)

---

## 매개변수화된 테스트

- 테스트 클래스에서 매개변수를 사용할 수 있음

  > 하나의 테스트 메서드를 포함핧 수 있으며 이 메서드는 제공된 다른 매개변수로 실행됨
  >>- @RunWith(Parameterized.class) 는 매개변수화된 테스트 클래스임을 명시
  >>- 매개변수화된 테스트 클래스는 @Parameters 어노테이션이 달린 static 메서드를 포함해야 함
  >>>- 이 메서드는 배열 컬렉션을 생성하고 반환하고, 각 항목은 테스트의 매개변수로 사용됨
  
  ```java
  import org.junit.Test;
  import org.junit.runner.RunWith;
  import org.junit.runners.Parameterized;
  import org.junit.runners.Parameterized.Parameters;

  import static org.junit.Assert.assertEquals;
  import static org.junit.runners.Parameterized.*;

  @RunWith(Parameterized.class)
  public class ParameterizedTestFields {
    @Parameter(0)
    public int m1;
    @Parameter(1)
    public int m2;
    @Parameter(2)
    public int result;

    // @Parameter 대신 생성자도 가능함
    /*
    public ParameterizedTestUsingConstructor(int p1, int p2){
      m1=p1;
      m2=p2;
    }
    */

    @Parameters
    public static Collection<Object[]> data() {
      Object[][] data=new Object[][] {{1,2,2},{5,3,15},{121,4,484}};
      return Arrays.asList(data);
    }

    @Test
    public void testMultiplyException() {
      MyClass tester=new MyClass();
      assertEquals("Result", result, tester.multiply(m1,m2));
    }

    class MyClass {
      public int multyply(int i, int j){
        return i*j;
      }
    }
  }
  ```

  > 위의 예에서 테스트 메서드는 데이터셋의 개수만큼 3번 실행됨

[더 유연하고 쓰기 쉬운 접근 방식 링크 참조](https://github.com/Pragmatists/JUnitParams)

<br>

[목차로 이동](#목차)

---

## JUnit 규칙

- @Rule 어노테이션을 통해 테스트 클래스의 각 테스트에 동작을 추가할 수 있음

  > 테스트에서 사용하고 구성할 수 있는 개체를 만들 수 있음   
  >- 위의 예시에선 테스트 코드를 실행하는 동안 예상되는 예외 메시지를 지정할 수 있음

  ```java
  import org.junit.Rule;
  import org.junit.Test;
  import org.junit.rules.ExpectedException;

  public class RuleExceptionTesterExample {
    @Rule
    public ExceptedException exception=ExpectedException.none();

    @Test
    public void throwsIllegalArgumentExceptionIfIconIsNull() {
      exception.expect(IllegalArgumentException.class);
      exception.expectMessage("Negative value not allowed");
      ClassToBeTested t=new ClassToBeTested();
      t.methodToBeTest(-1);
    }
  }
  ```

  >- 각 테스트 실행 후 자동으로 제거되는 파일과 폴더를 설정할 수 있음

  ```java
  import static org.junit.Assert.assertTrue;

  import java.io.File;
  import java.io.IOException;

  import org.junit.Rule;
  import org.junit.Test;
  import org.junit.rules.TemporaryFolder;

  public class RuleTester {
    @Rule
    public TemporaryFolder folder=new TemporaryFolder();

    @Test
    public void testUsingTempFolder() thorws IOException {
      File createdFolder=folder.newFolder("newfolder");
      File createdFile=folder.newFile("myfilefile.txt");
      assertTrue(createdFile.exists());
    }
  }
  ```

[더 많은 예시 링크 참조](https://github.com/junit-team/junit4/wiki/Rules)

<br>

[목차로 이동](#목차)

---

## 정적인 JUnit 사용

- 클래스를 지정하지 않고 필드 및 메서드를 그대로 사용할 수 있어 테스트의 길이를 간결하게 함

  ```java
  Assert.assertEquals("10*5 is 50", 50, tester.multiply(10, 5));

  import static org.junit.Assert.assertEquals;

  assertEquals("10*5 is 50", 50, tester.multiply(10, 5));
  ```

<br>

[목차로 이동](#목차)

---