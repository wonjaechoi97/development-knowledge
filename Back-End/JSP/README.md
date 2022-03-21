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
  - [Model1](#model1)
  - [Model2 MVC Pattern(Model-View-Controller)](#model2-mvc-patternmodel-view-controller)
  - [Cookie와 HttpSession](#cookie와-httpsession)
    - [http protocol](#http-protocol)
    - [Cookie](#cookie)
    - [HttpSession](#httpsession)
  - [EL (Expression Language)](#el-expression-language)
    - [EL 개요](#el-개요)
    - [EL 문법](#el-문법)
    - [EL 내장객체](#el-내장객체)
  - [JSTL (Jsp Standard Tag Library)](#jstl-jsp-standard-tag-library)
    - [JSTL Tag](#jstl-tag)
    - [JSTL - core tag](#jstl---core-tag)
      - [변수 선언 - <c:set>](#변수-선언---cset)
      - [예외 - <c:catch>](#예외---ccatch)
      - [조건문 - <c:if>,<c:choose>,<c:when>,<c:otherwise>](#조건문---cifcchoosecwhencotherwise)
      - [반복문 - <c:forEach>](#반복문---cforeach)

<br>

---

## Web Architecture
<br>
<img src="imgs/WebArchitecture.png">

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

## Model1

- model1은 view와 logic을 JSP 페이지 하나에서 처리하는 구조

- 요청이 들어오면 JSP 페이지는 java beans 나 별도의 service class를 이용하여 작업을 처리, 결과를 client에 출력

- View와 Controller를 JSP가 담당하고 Model을 Java Beans가 담당

- 장단점
  장점|단점
  :--|:--
  구조가 단순하며 직관적이라 배우기 쉬움|출력을 위한 view(html)코드와 로직 처리를 위한 java 코드가 섞여 있기 때문에 JSP 코드 자체가 복잡해짐
  개발 시간이 비교적 짧기 때문에 개발 비용 감소|JSP 코드에 Back-End(Developer)와 Front-End(Designer)가 혼재되기 때문에 분업이 힘듬
  &nbsp;|project의 규모가 커지면 코드가 복잡해지므로 유지보수에 어려움
  &nbsp;|확장성이 나쁨

<br>

[목차로 이동](#목차)

---

## Model2 MVC Pattern(Model-View-Controller)
<br>

<img src="imgs/Model2_Architecture.png">

<br>

- MVC(Model-View-Controller) pattern을 웹 개발에 도입한 구조

- client 요청에 대한 처리는 servlet이, logic처리는 java class(Service, Dao, ..), client에게 출력하는 response page를 JSP가 담당

  Model2|MVC Pattern|설명
  :--|:--|:--
  Service, Dao or Java Beanse|Model|Logic(Business & DB Logic)을 처리하는 모든 것<br>controller로 부터 넘어온 data를 이용하여 이를 수행하고 그에 대한 결과를 다시 contoller에 return
  JSP|View|모든 화면 처리를 담당<br>Client의 요청에 대한 결과 뿐 아니라 controller에 요청을 보내는 화면단도 jsp에서 처리<br>로직 처리를 위한 java code는 사라지고 결과 출력을 위한 code만 존재
  Servlet|Controller|Client의 요청을 분석하여 로직 처리를 위한 Model단을 호출<br>return 받은 결과 데이터를 필요에 따라 request, session등에 저장하고, redirect 또는 forward 방식으로 jsp(view) page를 이용하여 출력

- 장단점
  장점|단점
  :--|:--
  출력을 위한 view(html) 코드와 로직 처리를 위한 java 코드가 분리|구조가 복잡하여 초기 진입이 어려움
  화면단과 로직단이 분리되어 분업이 용이|개발 시간의 증가로 개발 비용 증가
  기능에 따라 code가 분리 되어 유지 보수가 쉬워짐|
  확장성이 뛰어남

<br>

[목차로 이동](#목차)

---

## Cookie와 HttpSession

### http protocol

- client가 server에 요청

- server는 요청에 대한 처리 후 client에 응답

- 응답 후 연결을 해제

  - 지속적인 연결로 인한 자원낭비를 줄이기 위해 연결을 해제

  - client와 server가 연결 상태를 유지해야 하는 경우 문제가 발생 (로그인 정보)

  - client 단위로 상태 정보를 유지해야 하는 경우 Cookie와 Session이 사용됨

<br>

[목차로 이동](#목차)

### Cookie

> javax.servlet.http.Cookie

- 개요

  - 서버가 사용자의 컴퓨터에 저장하는 정보파일

  - 사용자가 별도의 요청을 하지 않아도 브라우저는 request시 Cookie를 Request Header에 넣어 서버에 전송

  - key와 value로 구성되고 String 형태로 이루어져 있음

  - 브라우저마다 저장되는 쿠키는 다름 (서버는 브라우저가 다르면 다른 사용자로 인식)

- 사용목적

  - 세션관리 : 사용자 아이디, 접속시간, 장바구니 등 서버가 알아야 할 정보 저장

  - 개인화 : 사용자마다 적절한 페이지를 보여줄 수 있음

  - 트래킹 : 사용자의 행동과 패턴을 분석하고 기록

- 사용예시

  - ID 저장(자동로그인)

  - 일주일간 다시 보지 않기

  - 최근 검색한 상품들을 광고에 추천

  - 장바구니 기능

- 구성요소

  - 이름 : 각 쿠키를 구별하는 데 사용되는 이름

  - 값
  - 유효기간
  - 도메인 : 쿠키를 전송할 도메인
  - 경로(path) : 쿠키를 전송할 요청 경로

- 동작 순서

  - Client가 페이지를 요청

  - WAS가 Cookie를 생성
  - HTTP Header에 Cookie를 넣어 응답
  - 브라우저는 넘겨받은 Cookie를 PC에저장, 다시 WAS가 요청할 때 요청과 함께 Cookie를 전송
  - 브라우저가 종료되어도 Cookie의 유효 기간이 남아 있다면 Client는 계속 보관
  - 동일 사이트 재방문시 Client의 PC에 해당 Cookie가 있는 경우, 요청 페이지와 함께 Cookie를 전송

- 특징

  - 클라이언트에 총 300개의 쿠키를 저장할 수 있음

  - 하나의 도메인 당 20개의 쿠키를 가질 수 있음
  - 하나의 쿠키는 4KB까지 저장 가능

- 주요 기능

  기능|method
  :--|:--
  생성|Cookie cookie=new Cookie(String name, String value);
  값 변경/얻기|cookie.serValue(String value); / String value=cookie.getValue();
  사용 도메인지정/얻기|cookie.setDomain(String domain); / String domain=cookie.getDomain();
  값 범위지정/얻기|cookie.setPath(String path); / String  path=cookie.getPath();
  Cookie의 유효기간지정/얻기|cookie.setMaxAge(int expiry); / int expiry=cookie.getMaxAge();<br>cookie 삭제 : cookie.setMaxAge(0);
  생성된 Cookie를 Client에 전송|response.addCookie(cookie);
  Client에 저장된 Cookie 얻기|Cookie cookies[]=request.getCookies();

<br>

[목차로 이동](#목차)

### HttpSession

> javax.servlet.http.HttpSession

- 개요

  - 방문자가 웹 서버에 접속해 있는 상태를 하나의 단위로 보고 그것을 세션이라 함

  - WAS의 메모리에 Object의 형태로 저장
  - 메모리가 허용하는 용량까지 제한 업싱 저장 가능

- 사용 예

  - site 내에서 화면을 이동해도 로그인(사용자 정보)이 풀리지 않고 유지

  - 장바구니

- 동작 순서

  - 클라이언트가 페이지를 요청

  - 서버는 접근한 클라이언트의 Request-Header 필드인 Cookie를 확인하여, 클라이언트가 해당 session-id를 보냈는지 확인
  - session-id가 존재하지 않는다면, 서버는 session-id를 생상해 클라이언트에게 돌려줌
  - 서버에서 클라이언트로 돌려준 session-id를 쿠키를 사용해 서버에 저장. 쿠키 이름 : JSESSIONID
  - 클라이언트는 재 접속 시, 이 쿠키(JSESSIONID)를 이용하여 session-id 값을 서버에 전달

- 특징

  - 웹 서버에 웹 컨테이너의 상태를 유지하기 위한 정보를 저장

  - 웹 서버에 저장되는 쿠키(세선쿠키)
  - 브라우저를 닫거나, 서버에서 세션을 삭제 했을 떄만 삭제가 되므로, 쿠키보다 비교적 보안이 좋음
  - 저장 데이터에 제한이 없음
  - 각 클라이언트 고유 Session ID를 부여함
  - Session ID로 클라이언트를 구반하여 각 클라이언트 요구에 맞는 서비스 제공

- 주요 기능
  기능|method
  :--|:--
  생성|HttpSession session=request.getSession();<br>HttpSession session=request.getSession(false);
  값 저장|session.setAttribute(String name, Object value);
  값 얻기|Object obj=session.getAttribute(String name);
  값 제거|session.removeAttribute(String name); // 특정 이름의 속성제거<br>session.invalidate(); // binding되어 있는 모든 속성 제거
  생성시간|long ct=session.getCreateionTime();
  마지막 접근 시간|long last=session.getLastAccessedTime();

  <br>

[목차로 이동](#목차)

---

## EL (Expression Language)

### EL 개요

- 개요

  - EL은 표현을 위한 언어로 JSP 스크립트의 표현식(<%= %>)을 대신하여 속성 값을 쉽게 출력하도록 고안된 언어

  - EL 표현식에서 도트 연산자 왼쪽은 반드시 java.util.Map 객체 또는 Java Bean 객체여야 함
  - EL 표현식에서 도트 연산자 오른쪽은 반드시 맵의 키이거나 Bean 프로퍼티여야 함

- 기능

  - JSP의 네 가지 기본 객체가 제공하는 영역의 속성 사용

  - 자바 클래스 메서드 호출 기능
  - 표현 언어만의 기본 객체 제공
  - 수치, 관계, 논리 연산 제공

<br>

[목차로 이동](#목차)

### EL 문법

- 문법
  
  ```java
  // JSP의 스크립트릿을 EL로 표현
  <%= ((com.myapp.model.MemberDto)request.getAttribute("userinfo")).getZipDto().getAddress %>
  ${userinfo.zipDto.address}

  // EL :  Map을 사용하는 경우
  ${Map.Map의키} 

  // EL :  Java Bean을 사용하는 경우
  ${Java Bean.Bean 프로퍼티}
  ```

  - [] 연산자

    - EL에는 Dot 표기법 외에 [] 연산자를 사용하여 객체의 값에 접근할 수 있음

    - [] 연산자 안의 값이 문자열인 경우, 맵의 키가 될 수도, Bean 프로퍼티나 리스트 및 배열의 인덱스가 될 수 있음
    - 배열과 리스트인 경우, 문자로 된 인덱스 값은 숫자로 변경하여 처리

      ```java
      // [] 연산자를 이용한 객체 프로퍼티 접근
      ${userinfo["name"]}

      // Dot 표기를 이용한 객체 프로퍼티 접근
      ${userinfo.name}

      // 리스트나 배열 요소에 접근
      // Servlet
      String[] names={"홍길동", "이순신"};
      request.setAttribute("userNames", names);

      // JSP
      ${userNames[0]} // 홍길동
      ${userNames["1"]} // 문자열인 인덱스 값이 숫자로 변경되어 이순신
      ```

<br>

[목차로 이동](#목차)

### EL 내장객체

- JSP 페이지의 EL 표현식에서 사용할 수 있는 객체

  category|identifier|Type|description
  :--|:--|:--|:--
  JSP|pageContext|Java Bean|현재 페이지의 프로세싱과 상응하는 PageContext instance
  범위(scope)|pageScope|Map|page scope에 저장된 객체를 추출
  &nbsp;|requestScope|Map|request scope에 저장된 객체를 추출
  &nbsp;|sessionScope|Map|session scope에 저장된 객체를 추출
  &nbsp;|applicationScope|Map|application scope에 저장된 객체를 추출
  요청 매개변수|param|Map|ServletRequest.getParameter(String)을 통해 요청 정보를 추출
  &nbsp;|paramValues|Map|ServletRequest.getParameterValues(String)을 통해 요청 정보를 추출
  요쳥 헤더|header|Map|HttpServletRequest.getHeader(String)을 통해 헤더 정보를 추출
  &nbsp;|headerValues|Map|HttpServletRequest.getHeaders(String)을 통해 헤더 정보를 추출
  쿠키|cookie|Map|HttpServletRequest.getCookies()를 통해 쿠키 정보를 추출
  초기화 매개변수|initParam|Map|ServletContext.getInitParameter(String)를 통해 초기화 파라미터를 추출

<br>

[목차로 이동](#목차)

### EL 사용

- EL 사용

  - pageContext를 제외한 모든 EL 내장 객체는 Map

  - key와 value의 쌍으로 값을 저장
  - 기본 문법: ```${expr}```
  
- EL에서 객체 접근

  - request.setAttribute("userinfo", "홍길동");

    - ${requestScope.userinfo}

    - ${pageContext.request.userinfo}
    - ${userinfo}
      > property 이름만 작성시 자동으로 pageScope > requestScope > sessionScope > applicationScope 순으로 객체를 찾음

  - url?name=홍길동&fruit=사과&fruit=파인애플
    - ${param.name}

    - ${paramValues.fruit[0]}
    - ${paramValues.fruit[1]}

- ${cookie.id.value}

  - Cookie가 null이라면 null return

  - null이 아니라면 id를 검사 후 null이라면 null return
  - null이 아니라면 value값 검사
    > EL은 값이 null이면 공백을 출력함

  ```java
  // 스크립트릿을 통한 쿠키 값 출력
  Cookie[] cookies=request.getCookies();
  for(Cookie cookie:cookies) {
    if(cookie.getName().equals("userId")) {
      out.println(cookie.getValue());
    }
  }

  // EL 내장객체를 통한 쿠키 값 출력
  ${cookie.userId.value} 
  ```

- EL 연산자
  종류|설명
  :--|:--
  산술|+, -, *, / (div), % (mod)
  관계형|== (eq), != (ne), < (lt), > (gt), <= (le), >= (ge)
  3항 연산|조건 ? 값1 : 값2
  논리|&& (and), || (or), ! (not)
  타당성검사|empty

  - empty 연산결과 true인 경우 (${empty var})

    - null

    - 빈 문자열("")
    - 길이가 0인 배열([])
    - 빈 Map 객체
    - 빈 Collection 객체

- EL 에서 객체 메서드 호출
  ```java
  <%
  List<MemberDto> list=dao.getMembers();
  request.setAttribute("users", list);
  %>

  // 회원수
  ${requestScope.users.size()}
  ${users.size()}
  <%= request.getAttribute("users").getSize() %>
  ```

  <br>

[목차로 이동](#목차)

---

## JSTL (Jsp Standard Tag Library)

- 개요

  - custom tag : 개발자가 직접 태그를 작성할 수 있는 기능을 제공
  - custom tag 중에서 많이 사용되는 것들을 모아서 JSTL로 표준화
  - 논리 판단, 반복문 처리, 데이터베이스 등의 처리를 할 수 있음
  - JSP 2.1 ~ JSP 2.2 와 호환되는 버전은 JSTL 1.2
  - JSTL은 JSP 페이지에서 스크립트릿을 사용하지 않고 액션태그를 통해 간단하게 처리하는 방법 제공
  - JSTL에는 다양한 액션태그가 존재, EL과 함께 사용하여 코드를 간결하게 작성함
    > jstl 1.2 jap link https://mvnrepository.com/artifact/javax.servlet/jstl

<br>

[목차로 이동](#목차)

### JSTL Tag
- JSTL Tag

  - directive 선언 형식 : ```<%@ taglib prefix="prefix" url="url" %>```

    library|prefix|function|URI
    :--|:--|:--|:--
    core|c|변수 지원, 흐름제어, URL처리|http://java.sun.com/jsp/jstl/c
    XML|x|XML코어, 흐름제어, XML변환|http://java.sun.com/jsp/jstl/xml
    국제화|fmt|지역, 메시지 형식, 숫자 및 날짜 형식|http://java.sun.com/jsp/jstl/fmt
    database|dql|SQL|http://java.sun.com/jsp/jstl/sql
    함수|&nbsp;|Collection, String 처리|http://java.sun.com/jsp/jstl/functions

<br>

[목차로 이동](#목차)

### JSTL - core tag

  - 선언 : ```<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core %>```

    function|tag|description
    :--|:--|:--
    변수지원|set|jsp page에서 사용 할 변수 설정
    &nbsp;|remove|설정한 변수를 제거
    흐름제어|if|조건에 따른 코드 실행
    &nbsp;|choose,when,otherwise|다중 조건을 처리할 때 사용(if ~ else if ~ else)
    &nbsp;|forEach|array나 collection의 각 항목을 처리할 때 사용
    &nbsp;|forTokens|구분자로 분리된 각각의 토큰을 처리할 때 사용(StringTokenizer)
    URL처리|import|URL을 사용하여 다른 자원의 결과를 삽입
    &nbsp;|redirect|지정한 경로로 redirect
    &nbsp;|url|URL 작성
    기타태그|catch|Exception 처리에 사용
    &nbsp;|on|JspWriter에 내용을 처리한 후 출력

<br>

[목차로 이동](#목차)

#### 변수 선언 - <c:set>

- 변수나 특정 객체의 프로퍼티에 값을 할당할 때 사용

  ```java
  //value 속성을 사용하여 생존범위 변수 값 할당
  <c:set value="value" var="varName" [scope="{page|request|session|application}"] />
  ```

- value 속성의 값이나 액션의 Body content로 값을 설정
- var 속성은 변수를 나타내며, 변수의 생존범위는 scope 속성으로 설정 (default로 page)

  ```java
  // 액션의 Body 컨텐츠를 사용하여 생존범위 변수 값 할당
  <c:set var="varName" [scope="page|request|session|application}"]>
  body content
  </c:set>
  ```

- 특정 객체의 프로퍼티에 값을 할당할 때는 target 속성에 객체를 설정하고 property에 프로퍼티명을 설정

  ```java
  // value 속성을 이용하여 대상 객체의 프로퍼티 값 할당
  <c:set value="value" target="target" property="propertyName"/>
  ```
  ```java
  // 액션의 Body 컨텐츠를 사용하여 대상 객체의 프로퍼티 값 할당
  <c:set target="target" property="propertyName">
  body content
  </c:set>
  ```

<br>

[목차로 이동](#목차)

#### 예외 - <c:catch>

-  JSP 페이지는 예외가 발생하면 지정된 오류페이지를 통해 처리함

-  <c:catch> 액션은 JSP 페이지에서 예외가 발생할 만한 코드를 오류페이지로 넘기지 않고 직접 처리할 때 사용
-  var 속성에는 발생한 예외를 담을 page 생존범위 변수 지정
-  <c:catch>와 <c:if> 액션을 함께 사용하여 try-catch 같은 기능 구현

    ```java
    <%@ page contentType="text/html" pageEncoding="UTF-8" errorPage="error.jsp" %>
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

    <c:catch var="try">
    <%
      String str=null;
      out.println("length of string : "+str.length()); // 예외 상황
    %>
    </c:catch>

    <c:if test="${try != null}">
      예외 상황입니다. ${try.message}
    </c:if>
    ```

<br>

[목차로 이동](#목차)

#### 조건문 - <c:if>,<c:choose>,<c:when>,<c:otherwise>

- <c:if> 액션은 test 속성에 지정된 표현식을 평가하여 결과가 true인 경우 액션의 Body 컨텐츠 수행
  ```java
  <c:if test="${userType eq 'admin'}">
    <jsp:include page="admin.jsp">
  </c:if>
  ```

- <c:if> 액션의 var 속성은 표현식의 평가 결과인 Boolean 값을 담은 변수를 나타내며, 변수의 생존범위는 scope 속성으로 설정
  ```java
  <c:if test="${userType eq 'admin'}" var="accessible">
    <jsp:include page="admin.jsp">
  </c:if>
  <c:if test="${category=='user' && menu=='list'}">
    회원 목록
  </c:if>
  ```

- <c:choose><c:when><c:otherwise> 액션을 사용하면 if, else if, else 와 같이 처리 가능
  ```java
  <c:choose>
    <c:when test="${userType=='admin'}">
      관리자 화면
    </c:when>
    <c:when test="${userType=='member'}">
      회원 사용자 화면
    </c:when>
    <c:otherwise>
      일반 사용자 화면
    </c:otherwise>
  </c:choose>
  ```

<br>

[목차로 이동](#목차)

#### 반복문 - <c:forEach>

- 컬렉션에 있는 항목들에 대하여 액션의 Body 컨텐츠를 반복하여 수행

- 컬렉션에는 Array, Collection, Map 또는 콤마로 분리된 문자열이 올 수 있음
- var 속성에는 반복에 대한 현재 항목을 담는 변수를 지정하고 items 속성은 반복할 항목들을 갖는 컬렉션을 지정
- varStatus 속성에 지정한 변수를 통해 현재 반복의 상태를 알 수 있음

  ```java
  courses - [java, c, spring, mybatis, jquery, javascript]

  // 리스트 출력
  <c:forEach var="course" items="${courses}">
    ${course.name}<br>
  </c:forEach>

  // 순번과 함께 리스트 출력
  <c:forEach var="course" items="${courses}" varStatus="varStatus">
    ${varStatus.count}. ${course.name}<br>
  </c:forEach>

  // 짝수번만 출력
  <c:forEach var="course" items="${courses}" begin="0" end="5" step="2">
    ${course.name}<br>
  </c:forEach>

  // 1부터 5까지 출력
  <c:forEach var="value" begin="1" end="5" step="1">
    ${value}<br>
  </c:forEach>
  ```
  
<br>

[목차로 이동](#목차)