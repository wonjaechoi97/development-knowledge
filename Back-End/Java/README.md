# **Java** 🔠

### 참고자료

- Effective JAVA 3/E


## 목차

  - [객체 생성과 파괴](#객체-생성과-파괴)
    - [생성자 대신 정적 팩토리 메서드를 고려하기](#생성자-대신-정적-팩토리-메서드를-고려하기)
    - [생성자에 매개변수가 많다면 빌더를 고려하기](#생성자에-매개변수가-많다면-빌더를-고려하기)
    - [private 생성자나 열거 타입으로 싱글턴임을 보증하기](#private-생성자나-열거-타입으로-싱글턴임을-보증하기)
    - [인스턴스화를 막을땐 private 생성자를 사용하기](#인스턴스화를-막을땐-private-생성자를-사용하기)
    - [자원을 직접 명시하지 말고 의존 객체 주입을 사용하기](#자원을-직접-명시하지-말고-의존-객체-주입을-사용하기)
    - 

<br>

---

## 객체 생성과 파괴

### 생성자 대신 정적 팩토리 메서드를 고려하기

- 장점

  - 이름을 가질 수 있어 반환될 객체의 특성을 쉽게 묘사할 수 있음

  - 호출마다 인스턴스를 새로 생성하지 않아 성능 향상

  - 반환 타입의 하위 타입 객체를 반환할 수 있어 유연함

  - 입력 매개변수에 따라 다양한 객체를 반환할 수 있음

  - 정적 팩토리 메서드 작성 시점에 반환할 객체의 클래스가 존재하지 않아도 됨

- 단점

  - 정적 팩토리 메서드만 제공 시 상속을 할 수 없음

  - 이름만 나타나므로 원하는 메서드를 찾기 힘듬

    - 잘 알려진 규약으로 메서드 명명하여 완화

      > from, of, valueOf, getInstance, newInstance, getType, newType 등


[목차로 이동](#목차)

<br>

### 생성자에 매개변수가 많다면 빌더를 고려하기

- 객체 생성자 패턴

  - 점층적 생성자 패턴

    ```java
    public class book {
      private final String title;  // 필수
      private final String author; // 선택
      private final int price;     // 선택
      
      public book(String title) { this(title, "", 0); }
      public book(String title, String author) { this(title, author, 0); }
      public book(String title, String author, int price){
        this.title=title;
        this.author=author;
        this.price=price;
      }
    }
    ```

    - 매개변수의 개수가 많아지면 코드를 작성하거나 읽기 어려움

  - 자바빈즈 패턴

    ```java
    public class book {
      private String title="";  // 필수
      private String author=""; // 선택
      private int price=0;      // 선택
      
      public book() { }
      public void setTitle(String val) { title=val; }
      public void setAuthor(String val) { author=val; }
      public void setPrice(int val) { price=val; }
    }
    ```

    - 객체를 만들려면 메서드를 여러 개 호출해야해야함

    - 객체가 완전히 생성되기 전까지는 일관성이 무너진 상태에 놓이게 되므로 불변으로 만들 수 없음

    - 스레드에 안전적이지 않음

<br>

  - 빌더 패턴

    ```java
    public class book {
      private final String title;
      private final String author;
      private final int price;
      
      public static class Builder {
        private final String title;  // 필수
        private String author; // 선택
        private int price;     // 선택

        public Builder(String title) { this.title=title; }
        public Builder author(String val) { author=val; return this; }
        public Builder price(int val) { price=val; return this; }
        public Book build() { return new Book(this); }
      }

      private Book(Builder builder) {
        title=builder.title;
        author=builder.author;
        price=builder.price;
      }
    }
    ```

    - 빌더의 세터가 빌더 자신을 반환하기 때문에 메서드 체이닝 방식으로 연쇄적 호출 가능

      > Book book=new Book.Builder("홍길동전").author("옛날사람").price(400).build();

    ```java
    public abstract class Book {
      public enum BookType { NOVEL, ESSAY, CARTOON, TRAVELS }
      final Set<BookType> bookTypes;

      abstract static class Builder<T extends Builder<T>> {
        EnumSet<BookType> bookTypes=EnumSet.noneOf(BookType.class);
        public T addBookType(BookType bookType) {
          bookTypes.add(Objects.requireNonNull(bookType));
          return self();
        }
        abstract Book build();
        protected abstract T self(); // 형 변환 없이 메서드 체이닝을 위한 셀프타입관용구
      }
      Book(Builder<?> builder) { bookTypes=builder.bookTypes.clone(); }
    }

    public class NationalBook extend Book {
      public enum Nation { KOREA, AMERICA, CANADA, GERMANY }
      private final Nation nation;

      public static class Builder extends Book.Builder<Builder> {
        private final Nation nation;

        public Builder(Nation nation) { this.nation=Object.requireNonNull(nation); }
        @Override
        public NationBook build() { return new NationBook(this); }
        @Override
        protected Builder self() { return this; }
      }
      private NationBook(Builder builder) {
        super(builder);
        nation=builder.nation;
      }
    }

    public class SaleBook extend Book {
      public final boolean outOfPrint;

      public static class Builder extends Book.Builder<Builder> {
        private boolean outOfPrint=false; // 기본값

        public Builder outOfPrint() {
          outOfPrint=true;
          return this;
        }

        @Override
        public SaleBook build() { return new SaleBook(this); }
        @Override
        protected Builder self() { return this; }
      }
      private SaleBook(Builder builder) {
        super(builder);
        outOfPrint=builder.outOfPrint;
      }
    }
    ```

    - 계층적으로 설계된 클래스에서 함께 쓰기 좋음

    - 빌더를 이용하면 생성자로는 할 수 없는 가변인수 매개변수를 여러개 사용 가능
      
      > NationalBook nBook=new NationalBook.Builder(KOREA).addBookType(NOVEL).addBookType(CARTOON).build();   
      SaleBook sBook=new SaleBook.Builder().addBookType(GERMANY).outOfPrint().build();
    
    - 빌더 생성 비용이 크지는 않지만 객체 생성을 위해 빌더를 먼저 만들어야하므로, 성능에 민감한 경우 문제가 될 수 있음

    - 생성자나 정적 팩토리가 처리해야 할 매개변수가 많다면 빌더 패턴을 선택하는 것이 나음


[목차로 이동](#목차)

<br>

### private 생성자나 열거 타입으로 싱글턴임을 보증하기

- 싱글턴(singleton)

  > 인스턴스를 오직 하나만 생성할 수 있는 클래스

- 주의사항

  - 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기 어려워질 수 있음

    > 싱글턴 인스턴스는 mock 구현으로 대체할 수 없기 때문

- 구현 방식

  - public static 멤버가 final 필드인 방식

    ```java
    public class BookSystem {
      public static final BookSystem INSTANCE= new BookSystem();
      private BookSystem() { ... } // 생성자 private

      public void leaveTheBuilding() { ... }
    }
    ```

    - 리플렉션 API인 AccessibleObject.setAccessible을 사용해 private 생성자를 호출할 수 있는 예외가 있음

      > 생성자에서 두 번째 객체 생성 시도시 예외를 던지게하여 방어함

    - 장점

      - public static 필드가 final로 해당 클래스가 싱글턴임이 API에 명백히 드러남

      - 간결함

  - 정적 팩토리 메서드를 public static 멤버로 제공하는 방식

    ```java
    public class BookSystem {
      private static final BookSystem INSTANCE= new BookSystem();
      private BookSystem() { ... } // 생성자 private
      private static BookSystem getInstance() { return INSTANCE; }

      public void leaveTheBuilding() { ... }
    }
    ```

    - 리플렉션 API인 AccessibleObject.setAccessible을 사용해 private 생성자를 호출할 수 있는 예외가 있음

      > 생성자에서 두 번째 객체 생성 시도시 예외를 던지게하여 방어함

    - 장점

      - API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있음

      - 정적 팩토리를 제네릭 싱글턴 팩토리로 만들 수 있음

      - 정적 팩토리의 메서드 참조를 공급자로 사용할 수 있음

  - 위 두 가지 방식의 문제점

    - 두 가지 방식의 싱글턴 클래스를 직렬화하려면 Serializable 인터페이스를 구현해야 할 뿐만 아니라 모든 인스턴스 필드를 transient 로 선언하고 readResolve 메서드를 제공해야 함

    > Serializable 인터페이스만 구현할 경우 직렬화된 인스턴스를 역직렬화할 때마다 새로운 인스턴스가 생성됨

  - 원소가 하나인 열거 타입을 선언하는 방식

    ```java
    public enum BookSystem {
      INSTANCE;

      public void leaveTheBuilding() { ... }
    }
    ```

    - 리플렉션 공격에서 자유로움

    - 직렬화에 추가 노력이 필요없음

    - 대부분 상황에서 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법임

    - 단점

      - 만들고자하는 싱글턴이 Enum 외의 클래스를 상속해야 한다면 사용할 수 없음

        > 모든 Enum 은 java.lang.enum 클래스를 상속하고 있기 때문임   
        다른 인터페이스를 구현하는 것은 가능함

[목차로 이동](#목차)

<br>

### 인스턴스화를 막을땐 private 생성자를 사용하기

- private 생성자를 추가하면 컴파일러가 기본 생성자를 만들지 않으므로 클래스의 인스턴스화를 막을 수 있음

  ```java
  public class UtilClass {
    // 기본 생성자가 만들어지는 것을 막는다 (인스턴스화 방지용)
    private UtilClass() {
      throw new AssertionError();
    }
  }
  ```

  - 생성자를 private으로 선언하면 하위 클래스가 상위 클래스의 생성자에 접근할 길이 막히므로 상속을 불가능하게 하는 효과도 있음

[목차로 이동](#목차)

<br>

### 자원을 직접 명시하지 말고 의존 객체 주입을 사용하기

- 

[목차로 이동](#목차)

<br>

---


