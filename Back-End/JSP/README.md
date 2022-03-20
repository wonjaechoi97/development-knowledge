# **Java Server Pages** 🔄

## 목차
  - [Web Architecture](#web-architecture)
  - [Dynamic Web Project 구조](#dynamic-web-project-구조)
  - [Java Servlet 개요](#java-servlet-개요)
  - [Servlet Class 구조](#servlet-class-구조)
  - [Servlet Life-Cycle](#servlet-life-cycle)
  - [Servlet Parameter 전송 방식](#servlet-parameter-전송-방식)
  - [JSP (Java Server Page) 개요](#jsp-java-server-page-개요)
  - [JSP Scripting Element](#jsp-scripting-element)
    - [선언 (Declaration)](#선언-declaration)
    - [스크립트릿 (Scriptlet)](#스크립트릿-scriptlet)
    - [표현식 (Expression)](#표현식-expression)
    - [주석 (Comment)](#주석-comment)
  - [JSP 지시자 (Directive)](#jsp-지시자-directive)
  - [JSP 기본객체](#jsp-기본객체)
  - [](#)
  - [](#)

<br>

---

## Web Architecture
<br>
<img src="img/../imgs/WebArchitecture.png">

<br>

[목차로 이동](#목차)

---

## Dynamic Web Project 구조

- root
  - Java Resources
    - src : web app java file 위치
    - Libraries
      - Apache Tomcat : tomcat lib (servlet-api.jar 위치)
      - JRE Library
  - WebContent : view 폴더
    - MATA-INF
    - WEB-INF
      - lib : web app 확장 library 폴더
      - web.xml : web app 설정 파일 (module 3.0 부터 annotation 으로대체)

<br>

[목차로 이동](#목차)

---

## Java Servlet 개요

- 자바를 사용하여 웹 페이지를 동적으로 생성하는 서버측 프로그램

- 웹 서버의 성능을 향상하기 위해 사용되는 자바 클래스의 일종
  > 서블릿은 자바 코드 안에 HTML을 포함   
  > JSP는 HTML 문서 안에 Java 코드를 포함

<br>

[목차로 이동](#목차)

---

## Servlet Class 구조

```java
public class HelloServlet extend HttpServlet {
  private static final long serialVersionUID = 1L;

  protect void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String name="홍길동";
    // response.setContentType("text/html");
    // response.setCharacterEncoding("UTF-8");
    response.setContentType("text/html;charset=UTF-8");
    PrintWriter out=response.getWriter();
    out.println("<html>");
    out.println("<head>");
    out.println("<title>"+name+"의 페이지</title>");
    out.println("</head>");
    out.println("<body>");
    out.println("Hello");
    out.println("안녕 "+name+"!");
    out.println("</body>");
    out.println("</html>");
  }
}
```

<br>

[목차로 이동](#목차)

---

## Servlet Life-Cycle

- Servlet class는 javaSE의 class와 다르게 main method가 없음

  > 객체의 생성과 사용(method call)의 주체가 사용자가 아닌 Servlet Container에게 있음
  - client가 요청시 Servlet Container는 Servlet 객체를 생성(한번만)하고, 초기화(한번만)하며 요청에 대한 처리(요청시마다 반복)를 하고 Servlet 객체가 필요 없게 되면 제거함

- 주요 메서드
  method|desciption
  :---|:---
  init()|서블릿이 메모리에 로드 될 때 한번 호출, 코드 수정으로 다시 로드 되면 재 호출
  deGet()|GET방식으로 data 전송 시 호출
  dePost()|POST방식으로 data 전송 시 호출
  service()|모든 요청은 service()를 통해서 doXXX() 메서드로 이동
  destroy()|서블릿이 메모리에서 해제되면 호출, 코드 수정시 호출

<br>

[목차로 이동](#목차)

---

## Servlet Parameter 전송 방식

&nbsp;|GET|POST
:--|:--|:--
특징|데이터를 URL 뒤 Query String으로 전달<br>입력 값이 적은 경우나 데이터가 노출되도 문제가 없을 경우 사용|HTTP header 뒤 body에 입력 스트림 데이터로 전달
장점|간단한 데이터를 빠르게 전송<br>form tag뿐 아니라 직접 URL에 입력하여 전송 가능|데이터의 제한이 없음<br>최소한의 보안 유지 효과를 볼 수 있음
단점|데이터 양에 제한이 있음<br>location bar(URL+parameters)를 통해 전송할 수 있는 데이터의 사이즈는 2048byte로 제한|전송 패킷을 body에 구성해야 하므로 GET방식보다 느림

- ```http://www.naver.com/do.jsp?parameter1=value1&parameter2=value2```

<br>

[목차로 이동](#목차)

---

## JSP (Java Server Page) 개요

- Java Server Page는 HTML내에 자바 코드를 삽입하여 웹 서버에서 동적으로 웹 페이지를 생성하여 웹 브라우저에 돌려주는 언어임

- Java EE 스펙 중 일부로써 웹 애플리케이션 서버에서 동작함

- 실행 시 자바 서블릿으로 변환된 후 실행되므로 서블릿과 거의 유사하다고 볼 수 있으나 서블릿과 달리 HTML 표준에 따라 작성되므로 웹 디자인하기에 편리함

- 아파치 스트럿츠나 자카르타 프로젝트의 JSTL 등 JSP 태그 라이브러리를 사용하는 경우 자바 코드없이 태그만으로 간략히 기술이 가능하여 생산성을 높일 수 있음

- PHP, ASP, ASP.NET과 비슷한 구조

  > servlet 변환 파일 위치   
  > %workspace%\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\work\Catalina\localhost\[project]\org\apache\jsp

<br>

[목차로 이동](#목차)

---

## JSP Scripting Element

### 선언 (Declaration)

- 멤버변수 선언이나 메서드를 선언하는 영역

  ```java
  <%! 멤버변수와 method작성 %>

  <%!
  String name;

  public void init() {
    name="홍길동";
  } 
  %>
    ```

### 스크립트릿 (Scriptlet)

- Client 요청 시 호출되는 영역으로, Servlet으로 변환 시 service() method에 해당되는 영역

- request, response에 관련된 코드 구현

  ```java
  <% java code %>

  <%
  for(int i=2; i<10; i++) {
    out.println("<tr>");
    String name=i%2==0?"1":"2";
    for(int j=1; j<10; j++) {
      out.println("<td class=\""+name+"\">"+i+"*"+i+" = "+i*j+"</td");
    }
    out.println("</tr>");
  }
  %>
  ```

### 표현식 (Expression)

- 데이터를 브라우저에 출력할 때 사용
  ```java
  <%= 문자열 %> == <% out.print(문자열); %>

  안녕 <%= name %> !!!
  ```

### 주석 (Comment)

- 코드 상에서 부가 설명을 작성

- html 주석은 출력 결과에 포함, jsp 주석은 출력 결과에 포함되지 않음
  ```java
  <%-- 주석 code --%>
  ```

<br>

[목차로 이동](#목차)

---

## JSP 지시자 (Directive)

- page Directive

  - 컨테이너에게 현재 JSP 페이지를 어떻게 처리할 것인가에 대한 정보 제공

    ```java
    <%@ page attr1="val" attr2="val2" ... %>
    ```

  - page 속성
    속성|기본값|설명
    :--|:--|:--
    language|java|스크립트에서 사용 할 언어 지정
    info|&nbsp;|현재 JSP 페이지에 대한 설명
    contentType|text/html;charset=ISO-8859-1|브라우저로 내보내는 내용의 MIME 형식 지정 및 문자 집합 지정
    pageEncoding|ISO-8859-1|현재 JSP 페이지 문자집합 지정
    import|&nbsp;|현재 JSP 페이지에서 사용할 Java 패키지나 클래스를 지정
    session|true|세션의 사용 유무 설정
    errorPage|&nbsp;|에러가 발생할 때 대신 처리될 JSP 페이지 지정
    isErrorPage|false|현재 JSP 페이지가 에러 핸들링 페이지인지 지정하는 요소
    buffer|8KB|버퍼의 크기
    autoflush|true|버퍼의 내용을 자동으로 브라우저로 보낼 지에 대한 설정
    isThreadsafe|true|현재 JSP 페이지가 멀티 쓰레드로 동작해도 안전한지 여부를 설정
    extends|javax.servlet.jsp.HttpJspPage|현재 JSP 페이지를 기본적인 클래스가 아닌 다른 클래스로부터 상속하도록 변경

- include Drective

  - 특정 jsp file을 페이지에 포함

  - 여러 jsp페이지에서 반복적으로 사용되는 부분을 jsp file로 만든 후 반복 영역에 include 시켜 반복되는 코드를 줄일 수 있음

    ```java
    <%@ include file="/template/header.jsp" %>
    ```

- taglib Directive

  - JSTL 또는 사용자에 의해서 만든 커스텀 태그(custom tag)를 이용할 때 사용

  - JSP 페이지 내에 불필요한 자바 코드를 줄일 수 있음

    ```java
    <%@ taglib prefix="c" url="http://java.sun.com/jsp/jstl/core" %>
    ```

<br>

[목차로 이동](#목차)

---

## JSP 기본객체

- JSP 기본객체

  기본 객체명|Type|설명
  :--|:--|:--
  request|javax.servlet.http.HttpServletRequest|HTML 폼 요소의 선택 값 같은 사용자 입력 정보를 읽어올 때 사용
  response|javax.servlet.http.HttpServletResponse|사용자 요청에 대한 응답을 처리하기 위해 사용
  pageContext|javax.servlet.jsp.PageContext|각종 기본 객체를 얻거나 forward 및 include 기능을 활용할 때 사용
  session|javax.servlet.http.HttpSession|클라이언트에 대한 세션 정보를 처리하기 위해 사용<br>page directive의 session 속성을 false로 하면 내장 객체는 생성이 안됨
  application|javax.servlet.ServletContext|웹 서버의 application 처리와 관련된 정보를 레퍼런스하기 위해 사용
  out|javax.servlet.jsp.JspWriter|사용자에게 전달하기 위한 output 스트림을 처리할 때 사용
  config|javax.servlet.ServletConfig|현재 JSP에 대한 초기화 환경을 처리하기 위해 사용
  page|java.lang.Object|현재 JSP 페이지에 대한 참조 변수
  exception|java.lang.Exception|전달된 오류 정보를 담고 있는 내장 객체<br>Error를 처리하는 JSP에서 isErrorPage 속성을 true로 설정하면 exception 내장 객체를 사용할 수 있고, 기본은 false로 설정되어 있음

<br>

- JSP 기본객체의 영역 (scope)

  기본객체|설명
  :--|:--
  pageContext|하나의 JSP 페이지를 처리할 때 사용되는 영역<br>한 번의 요청에 대하여 하나의 JSP 페이지 호출, 한 개의 page 객체만 대응<br>페이지 영역에 저장한 값은 페이지를 벗어나면 사라짐<br>커스텀 태그에서 새로운 변수를 추가할 때 사용
  request|하나의 HTTP 요청을 처리할 때 사용되는 영역<br>웹 브라우저가 요청할 때 마다 새로운 request 객체 생성됨<br>request 영역에 저장한 속성은 그 요청에 대한 응답이 완료시 사라짐
  session|하나의 웹 브라우저와 관련된 영역<br>같은 웹 브라우저 내 요청되는 페이지들은 같은 session들을 공유<br>로그인 정보 등을 저장
  application|하나의 웹 어플리케이션과 관련된 영역<br>웹 어플리케이션당 1개의 application 객체가 생성<br>같은 웹 어플리케이션에서 요청되는 페이지들은 같은 application 객체를 공유

<br>

- JSP 기본객체 영역의 공통 method

  - Servlet과 jsp 페이지 사이에 특정 정보를 주고 받거나 공유하기 위한 메서드

    method|설명
    :--|:--
    void setAttribute(String name, Object value)|문자열 name 이름으로 Object형 데이터를 저장<br>Object형이므로 어떠한 Java객체도 저장이 가능
    Object getAttribute(String name)|문자열 name에 해당하는 속성 값이 있다면 Object 형태로 가져오고 없으면 null을 리턴<br>리턴 값에 대한 적절한 형 변환이 필요
    Enumeration getAttributeNames()|현재 객체에 저장된 속성들의 이름들을 Enumeration 형태로 가져옴
    void removeAttribute(String name)|문자열 name에 해당하는 속성을 삭제

<br>

- WEB Page 이동

  &nbsp;|forward(request, response)|sendRedirect(location)
  :--|:--|:--
  사용 방법|RequestDispatcher dispatcher=request.getRequestDispatcher(path);<br>dispatcher forward(request, response);|response.sendRedirect(location);
  이동 범위|동일 서버(project)내 경로|동일 서버 포함 타 URL 가능
  location bar|기존 URL 유지(실제 이동되는 주소 확인 불가)|이동하는 page로 변경
  객체|기존의 request와 response가 그대로 전달|기존의 request와 response는 소멸되고, 새로운 request와 response가 생성
  속도|비교적 빠름|forward()에 비해 느림
  데이터 유지|request의 setAttribute(name, value)를 통해 전달|request로는 data 저장 불가능<br>session이나 cookie를 이용

  <br>

  [목차로 이동](#목차)

  ---

## 