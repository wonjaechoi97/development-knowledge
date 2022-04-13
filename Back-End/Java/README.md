# **Java** 🔠

### 참고자료

- Effective JAVA 3/E


## 목차

  - [객체 생성과 파괴](#객체-생성과-파괴)
    - [생성자 대신 정적 팩토리 메서드를 고려하기](#생성자-대신-정적-팩토리-메서드를-고려하기)
    - [생성자에 매개변수가 많다면 빌더를 고려하기](#생성자에-매개변수가-많다면-빌더를-고려하기)

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

<br>

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

---

