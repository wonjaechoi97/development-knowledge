# **HTML5** 📃

## 목차
  - [개요](#hypertext-markup-language)
  - [구성요소](#구성요소)
  - [table](#table)
  - [img](#img)
  - [anchor](#anchor)
  - [form](#form)
  - [input](#input)
  - [div & span](#div와-span)
  - [semantic](#semantic)

<br>

---

## **H**yper**T**ext **M**arkup **L**anguage

<br/>

- **W**orld **W**ide **W**eb에서 사용하는 문서 양식

    - **마크업 언어**(**M**arkup **L**anguage)로써 **웹 문서를 작성**하며, **tag를 사용**하여 문서의 **구조** 등을 기술하는 언어임
    - 문서에 **tag**를 사용하여 하이퍼텍스트, 표, 목록, 비디오 등을 **Web Browser**에 표현함

<br/>

- **웹표준**

    - **모든 브라우저**에서 웹서비스가 정상적으로 **보여질 수 있도록** 하는 것

        >**W3C(World Wide Web Consortium)** 에서 **HTML5**를 웹 표준으로 권고하고 웹 브라우저는 이를 따름

<br/>

- **특징**

    - 지금도 개발 중에 있고, **다양한 기능**이 추가됨
    - 별도의 플러그인 없이 **멀티미디어 요소 재생** 가능
    - **서버**와 **클라이언트** 사이에 **소켓 통신** 가능
    - 검색엔진이 웹사이트를 빠르게 검색할 수 있도록 **특정 tag에 의미를 부여**하는 **Semantic tag**를 추가

<br/>

- **Web & HTML 작동원리**

    1. 서버는 클라이언트의 **요청 내용**을 분석하여 결과값을 **HTML로 전송**
    2. 서버는 결과값을 전송한 후 클라이언트와 **연결 종료**
    3. 클라이언트는 서버로부터 **전달받은 HTML**을 **WebBrowser에 표시**
    4. WebBrowser는 **브라우저 엔진이 내장**되어 있고, **엔진이 tag를 해석**하여 화면에 표현

<br/>

- **문서구조**

    - **```<html>,<head>,<body>```** 로 구성됨 
    - ```<!DOCTYPE html>```
        > 현재 문서가 **HTML 문서임을 정의**

<br/>

- **tag와 속성**

    - ```html
        <a href="https://www.w3schools.com/">go w3school</a>
      ```
        > **tag**는 **속성**과 **속성의 값**이 존재함<br/>
        > **시작tag와 종료tag 사이**에 문서 내용을 정의
        >> 각 tag는 **고유의 의미**를 가지고 있으며, **WebBrowser**는 **tag의 의미**에 따라 **화면에 표시**

        > **시작 tag만 존재**하는 tag도 있음
        >> ```<br/>, <hr/>, <img/> ...```

    - HTML tag에는 **어느 tag에나 넣어서 사용**할 수 있는 **글로벌속성(global attribute)** 이 있음
        - **class** : tag에 적용할 **스타일의 이름**을 지정
            > ```<div class="content">...</div>```
        
        - **dir** : 내용의 **텍스트 방향**을 지정
            > 왼쪽 >> 오른쪽 (기본값, ltr)<br/>
            >  오른쪽 >> 왼쪽 (rtl)
            ```html
                <p dir="rtl">오른쪽에서 왼쪽으로 표시됨</p>
            ```
        - **id** : tag에 **유일한 ID**를 지정함
            > 자바스크립트에서 주로 사용
            ```html
                <input type="text" id="userid">        
            ```
        - **style** : **인라인 스타일**을 적용하기 위해 사용
            ```html
                <p style="color:red; text-align:center;">빨간색 가운데</p>
            ```
        - **title** : tag에 **추가 정보**를 지정, tag에 **마우스 포인터**를 위치시킬 경우 **title의 값 표시**
            ```html
                <p><abbr title="Web Application Server">WAS</abbr>는 ...</p>
            ```

<br/>

- **주석**
    ```html
        <!--
            주석은 내용에 포함되지 않음
        -->
    ```

[목차로 이동](#목차)

<br/>

### 구성요소

- **Root** 요소
    - **\<html>** tag는 HTML 문서 전체를 정의
    - **Head**(\<head>...\</head>) 와 **Body**(\<body>...\</body>)로 구성됨

<br/>

- **Head** 요소
    - **\<head>** tag는 브라우저에게 **HTML 문서의 머리 부분**임을 인식
    - **\<title>,\<meta>,\<style>,\<script>,\<link>** tag를 포함할 수 있음
    - **\<title>** tag는 **문서의 제목**을 의미하며, **브라우저의 제목 표시줄**에 tag 내용이 나타남
        > \<title> tag 이외의 **다른 tag로 표현한 정보**는 **화면에 출력되지 않음**
    - **\<meta>** tag는 문서의 작성자, 날짜, 키워드등 브라우저의 **본문에 나타나지 않는 일반 정보**를 나타냄
        - **name**과 **content**속성을 이용하여 **다양한 정보**를 나타냄
            ```html
                <!--name속성: description(문서요약), keyword(검색어 입력), author(제작자)등-->
                <meta name="name" content="value"> 

                <!--페이지 설명, 검색엔진 로봇이 수집-->
                <meta name="description" content="간단한 설명"> 

                <!--페이지의 키워드를 ,로 구분해서 나열, 검색엔진 로봇이 수집-->
                <meta name="keyword" content="html5, web"> 
                <meta name="author" content="페이지 제작자 정보">
            ```
        - **http-equiv** 속성을 이용하여 **인코딩 설정 및 문서 이동**, **새로고침**이 가능
            ```html
                <!--인코딩설정-->
                <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
                <!--문서를 자동으로 업데이트-->
                <meta http-equiv="refresh" content="30">
            ```
        - **charset** 속성을 이용하여 문서의 **인코딩 정보**를 설정
            ```html
                <meta charset="utf-8">
            ```

<br/>

- **Body** 요소
    - Web Browser에 보여질 **문서의 내용**을 작성
    - **\<head> tag 다음에 위치**하고 \<head> 내부에 위치하는 tag와 \<html>을 **제외한 모든 tag로 구성**할 수 있음
    - **id 속성**을 이용하여 문서 내에서 **특정 tag를 유일하게 식별**할 수 있음 (**id속성은 중복X**)
    - **class 속성**을 이용하여 **여러 tag에 공통적인 특성(CSS)을 부여**할 수 있음(**class속성은 중복O**)
    - **문단의 제목**을 지정할 때 **\<h1> - \<h6>** tag를 이용함
    - 문서구조를 **\<section>** tag로 구분하면 **같은 tag를 서로 다르게 표현**할 수 있음
        > 각 **\<section>** tag 내 문단의 제목을 \<h1>으로 작성해도 구조에 따라 다르게 표현됨

<br/>

- **특수문자**
    - 엔티티이름|설명|화면출력
      ---|:---:|---:
      `&nbsp;`|Non-breaking space(공백)|&nbsp;
      `&lt;`|Less than|&lt;
      `&gt;`|Greater than|&gt;
      `&amp;`|Ampersand|&amp;
      `&quot;`|Quotation mark|&quot;
      `&copy;`|Copyright|&copy;
      `&reg;`|regisered trademark|&reg;

<br/>

- **포맷팅 요소**
    - 화면에는 **동일하게 출력**되지만 각 요소가 가진 **의미가 다른 것**이 있음
        > **\<b>** 와 **\<string>** 은 모두 텍스트를 굵게 표현하지만, \<string>요소는 **텍스트를 강조**
    - tag명|설명|tag명|설명
      :---:|:---|:---:|:---
      \<abbr>|생략된 약어 표시, Title속성을 함께 사용|\<mark>|특정 문자열을 강조, 화면에는 하이라이팅 됨
      \<address>|연락처 정보 표시|\<hr>|구분선
      \<blockquote>|긴 인용문구 표시, 좌우로 들여쓰기가 됨|\<b>,\<string>|굵은 글씨로 표시, 특정 문자열을 강조(\<string>)
      \<q>|짧은 인용문구 표시, 좌우로 따옴표가 붙음|\<i>,\<em>|이탤릭(기울게) 표시, 특정 문자열을 강조(\<em>)
      \<cite>|웹 문서나 포스트에서 참고 내용 표시|\<big>,\<small>|큰 글자, 작은 글자로 표시
      \<pre>|공백, 줄바꿈 등 입력된 그대로 화면에 표시|\<sup>,\<sub>|위 첨자, 아래 첨자로 표시
      \<code>|컴퓨터 인식을 위한 소스 코드|\<s>,\<u>|취소선, 밑줄

<br/>

- **목록형 요소**
    - 목록 tag는 하나 이상의 **하위 tag를 포함**하고, 각 항목을 **들여쓰기**로 표현함
    - tag명|설명
      :---:|:---
      &lt;ul&gt;|번호 **없는** 목록을 표시, 항목 앞에 **심볼**을 표시
      &lt;ol&gt;|번호 **있는** 목록을 표시, **숫자, 알파벳, 로마숫자** 등으로 표시
      &lt;li&gt;|**&lt;ul&gt; 또는 &lt;ol&gt;** tag 하위에서 사용
      &lt;dl&gt;|**용어 정의와 설명**에 대한 내용을 목록화하여 표시
      &lt;dt&gt;|용어 목록의 **정의** 부분을 나타냄
      &lt;dd&gt;|용어 목록의 **설명** 부분을 나타냄

[목차로 이동](#목차)

<br/>

### table

- **table**
    - 데이터를 **행(Row)** 과 **열(Column)** 의 **셀(Cell)** 에 표시
      > **&lt;thead&gt;, &lt;tbody&gt;, &lt;tfoot&gt;** 요소를 사용하여 행들을 그룹화   
        **&lt;colgroup&gt;, &lt;col&gt;** 요소는 열 그룹을 위한 추가적인 구조정보를 제공   
      > 각 셀에는 **머리글(&lt;th&gt;)** 이나 **데이터(&lt;td&gt;)** 를 가질 수 있음
    - **&lt;caption&gt;** tag는 **table 제목**을 정의하기 위해 사용
      > table 당 **하나만** 사용   
        기본적으로 **가운데 정렬**, CSS를 통해 정렬방식을 변경
    - table 행 그룹 요소(**&lt;thead&gt;,&lt;tbody&gt;,&lt;tfoot&gt;**) 는 행들을 각 항목으로 **그룹핑**
      > 행 그룹은 최소 **하나 이상의 &lt;tr&gt;** 을 가져야 함   
        tbody는 여러 번 사용 가능
    - table 열 그룹 요소(**&lt;colgroup&gt;, &lt;col&gt;**)는 table 내에서 **구조적인 분리**를 가능하게 함
      > &lt;colgroup&gt; 요소의 **span** 속성은 열 그룹에서 **열의 개수**를 정의   
        &lt;col&gt; 요소의 **span** 속성은 &lt;col&gt; 에 의해 묶여진 **열의 개수**를 의미
    - 셀 병합
      > &lt;colspan&gt; 속성은 두 개 이상의 **열**을 하나로 합치기 위해 사용   
        &lt;rowspan&gt; 속성은 두 개 이상의 **행**을 하나로 합치기 위해 사용

[목차로 이동](#목차)

<br/>

### img

- **img**
    - **&lt;img&gt;** tag를 사용하여 문서에 **이미지를 삽입**
      > src 속성은 이미지 경로를 지정   
        height, width 속성은 이미지 사이즈를 지정   
        alt 속성은 이미지를 표시할 수 없을때 대신하여 보여질 텍스트를 지정   
        ```html
           <img src="../img/night.png" title="화성입니다." width="300" height="200" alt="야경이 참 이쁘네요.">
        ```
      > 설명글이 필요한 대상은 &lt;figure&gt; 태그로 묶고 설명 글은 &lt;figcaption&gt; 태그로 묶는다   
        ```html
            <figure>
			    <img src="../img/night.png" title="화성입니다.">
			    <figcaption>화성의 야경이 참 예쁘네요.</figcaption>
		    </figure>
        ```

[목차로 이동](#목차)
<br/>

### anchor

- **Anchor**
    - **&lt;a&gt;** 하나의 문서에서 다른 문서로 연결하기 위해 사용 (하이퍼링크)
      > 중첩해서 사용할 수 없다   
        href 속성으로 이동할 문서의 URL이나 현재 문서의 책갈피 지정
        target 속성으로 현재 또는 새로운 윈도우에서 이동할지 지정
        ```html
            <!-- 책갈피 지정 -->
                <ul id="menu">
                    <li><a href="#content1">내용1</a></li>
                </ul>
                <h3 id="content1">내용 1</h3>
                <p><a href="#menu">메뉴로이동</a></p>
            <!-- 이동 지정 -->
            <a href="http://www.naver.com" target="_self">네이버</a>
            <a href="http://www.google.com" target="_blank">구글</a>
        ```
    - **&lt;map&gt;** 하나의 이미지에 click 위치에 따라 서로 다른 link
        > area의 속성으로 href, target, shape 등이 있음   
          shape의 값으로 default, rect, circle, poly가 있음
        ```html
            <img src="../img/logo.png" usemap="#logo">
            <map name="logo">
                <area shape="rect" coords="5,5,185,80" href="http://www.naver.com" target="_blank">
                <area shape="rect" coords="190,5,345,80" href="http://www.daum.net" target="_blank">
            </map>
        ```
    - **&lt;link&gt;** 문서와 외부 자원을 연결하기 위해 사용
        > &lt;head&gt; 위치에 정의   
          rel 속성은 현재 문서와 연결될 문서 사이의 연관관계를 지정
          href 속성은 연결된 문서의 위치를 지정하기 위해 사용
        ```html
            <head>
                <meta charset="UTF-8">
                <link rel="stylesheet" href="css/main.css">
            </head>
        ```

<br/>

- **iframe**
    - 화면의 **일부분**에 **다른 문서**를 포함
        > src 속성은 포함시킬 외부 문서의 **경로**를 지정   
          height, width 속성은 프레임 **크기**를 지정   
          name 속성은 프레임의 **이름**을 지정
        ```html
            <a href="java.html" target="javacontent">java</a>
            <iframe src="java.html" name="javacontent" width="500" height="300"></iframe>
        ```

[목차로 이동](#목차)

<br/>

### form

- **form**
    - 사용자로부터 데이터를 **입력** 받아 **서버에서 처리**하기 위한 용도로 사용
        > 사용자가 HTML form에 적절한 데이터를 입력한 후 서버로 **submit**   
          서버는 submit 내용을 **분석**한 후 데이터를 **등록**하거나, 원하는 데이터를 **조회**하여 결과를 반환   
          control 요소마다 다양한 형식으로 입력을 받는다   
        
        tag명|설명
        :---:|:---
        &lt;form&gt;|사용자에게 입력 받을 항목을 정의
        &lt;input&gt;|여러 방식으로 사용자가 데이터를 입력
        &lt;textarea&gt;|여러 줄의 문자를 입력
        &lt;button&gt;|버튼을 표시
        &lt;select&gt;|select box(dropdown, combobox)를 표시
        &lt;optgroup&gt;|select box의 각 항목들을 그룹화
        &lt;option&gt;|select box의 각 항목들을 정의
        &lt;label&gt;|for 속성을 이용하여 다른 control 요소와 연결시켜 편리하게 선택할 수 있음
        &lt;fieldset&gt;|입력 항목들을 그룹화
        &lt;legend&gt;|&lt;fieldset&gt;의 제목을 지정
    
    - **form**
        - method : 사용자가 입력한 내용을 어떻게 전달할 것인지 지정
            > GET : 주소 표시줄에 입력한 내용이 표시, 256 ~ 2048byte의 데이터만 서버로 전송
              POST : HTTP 메세지의 Body에 담아서 전송하기 때문에 전송 내용의 길이에 제한이 없고, 표시되지 않음
        - name : form의 이름을 지정, 한 문서 안에 여러 &lt;form&gt; tag가 있을 경우 구분자로 사용
        - action : &lt;form&gt; tag 안의 내용들을 처리해 줄 서버상의 프로그램 지정(URL)
        - target : &lt;action&gt; 태그에서 지정한 스크립트 파일을 현재 창이 아닌 다른 위치에 열도록 지정
        - autocomplete : 자동완성 기능, 기본값 on
    
    - **label**
        - form control에 레이블(텍스트)을 연결
        ```html
            <form method="post" action="login.jsp">
                <label for="userid">아이디 : </label>
                <input type="text" id="userid" name="userid">
                <label for="pass">비밀번호 : </label>
                <input type="text" id="pass" name="pass">
                
                <input type="submit" value="로그인">
            </form>
        ```
    
    - **fieldset, legend**
        - form 요소를 그룹으로 묶음
        ```html
            <form>
                <fieldset>
                    <legend>필수 입력</legend>
                    <label for="userid">아이디 : </label>
                    <input type="text" id="userid" name="userid">
                    <label for="pass">비밀번호 : </label>
                    <input type="text" id="pass" name="pass">
                </fieldset>
                <fieldset>
                    <legend>선택 입력</legend>
                    <label for="phone">전화번호 : </label>
                    <input type="text" id="phone" name="phone">
                    <label for="address">주소 : </label>
                    <input type="text" id="address" name="address">
                </fieldset>
            </form>
        ```

[목차로 이동](#목차)

<br/>

### input

- **input**
    - type 속성에 따라 여러 형태로 화면에 표시
        > html 문서내에 id는 하나의 값만 가능하고, name은 중복이 허용
        ```html
            <input type="text" id="id" name="userid">
            <input type="password" id="pwd" name="pass">
            <input type="text" id="tel" name="phone" size="15" maxlength="11">
            <input type="submit" value="전송">
	        <input type="reset" value="초기화">
        ```
        type|설명
        :---|:---
        text|한 줄의 텍스트 입력필드
        password|비밀번호 입력필드
        search|검색 상자
        tel|전화번호 입력필드
        url|URL 주소 입력필드
        email|메일 주소 입력필드
        datetime|국제 표준시(UTC)로 설정된 날짜와 시간(년,월,시,분,초,분할 초)
        datetime-local|사용자 지역 기준 날짜와 시간(년,월,시,분,초,분할 초)
        date|사용자 지역 기준 날짜(년,월,일)
        month|사용자 지억 기준 날짜(년,월)
        week|사용자 지역 기준 날짜(년,주)
        time|사용자 지역 기준 시간(시,분,초,분할 초)
        number|숫자를 조절할 수 있는 화살표
        range|숫자를 조절할 수 있는 슬라이드 막대
        color|색상 표
        checkbox|주어진 항목에서 2개 이상 선택 가능한 체크박스
        radio|주어진 항목에서 1개만 선택할 수 있는 라디오 버튼
        file|파일을 첨부할 수 있는 버튼
        submit|서버 전송 버튼
        image|submit + image
        reset|리셋 버튼
        button|기능이 없는 버튼
        hidden|사용자에게 보이지 않지만 서버로 넘겨지는 값을 설정
    - textfield, password
        ```html
            <input type="text" id="userid" name="userid" size="10" maxlength="16" value="아이디입력">
            <input type="text" id="pass" name="pass" size="10" maxlength="16" placeholder="비밀번호입력">
        ```
    
    - search, url, email, tel
        > search는 박스의 x표시로 텍스트를 쉽게 지울 수 있음
        ```html
            <input type="search" id="name" name="name">
            <input type="email" id="mail" name="mail">
            <input type="tel" id="phone" name="phone">
            <input type="url" id="homepage" name="homepage">
        ```
    
    - number, range   
        속성|설명
        :---|:---
        min|필드에 입력할 수 있는 최소값 지정 (range 일때 기본 0)
        max|필드에 입력할 수 있는 최대값 지정 (range 일때 기본 100)
        step|짝수나 홀수 등 특정 숫자로 제한하려 할 때 숫자 간격 지정(기본 1)
        value|페이지 로딩 시 필드에 표시할 초기값
        ```html
            <input type="number" id="pnum" name="pnum" min="1" max="5" value="2">개</label>
            <input type="range" id="pnum" name="pnum" min="1" max="5" value="2">개</label>
        ```
    
    - checkbox, radio
        ```html
            <input type="checkbox" name="fruit" value="사과"> 사과
            <input type="checkbox" name="fruit" value="수박" checked="checked"> 수박
            <input type="checkbox" name="fruit" value="딸기"> 딸기
            <input type="checkbox" name="fruit" value="포도"> 포도<br>

            <input type="radio" name="gender" value="m"> 남
	        <input type="radio" name="gender" value="f" checked="checked"> 여
        ```
    
    - color
        > 파이어폭스, 크롬, 오페라, 안드로이드 브라우저만 지원
        ```html
            <input type="color" id="pcolor" name="pcolor" value="#00ff00"></label>
        ```
    
    - date, time
        - &lt;input type="date|month|week" [속성="속성값"]&gt;
        - &lt;input type="time|datetime|datetime-local" [속성="속성값"]&gt;   

        - 속성|설명
          :---|:---
          min|날짜나 시간의 최소값을 지정
          max|날짜나 시간의 최대값을 지정
          step|스핀 박스의 화살표를 누를 때마다 날짜나 시간을 얼마나 조절할지 지정
          value|화면에 표시할 초기값 지정
          > value속성의 경우   
          type="time" 일 경우, 시간은 00:00~23:59까지 입력   
          type="datetime 또는 datetime-local" 일 경우 T19:00 처럼 키워드 T를 쓰고 입력

    - button
        ```html
            <input type="submit" value="제출">
			<input type="reset" value="다시입력">
            <input type="button" value="반응없음">
			<input type="button" value="알림창" onclick="alert('일반 버튼입니다.');">
        ```
    
    - image button
        > image + submit
        ```html
            <input type="image" src="../img/login.png" alt="로그인">
        ```

    - file
        ```html
            <form method="post" action="register.jsp" enctype="multipart/form-data">
                <fieldset>
                    <label for="picture">사진첨부 : </label>
                    <input type="file" id="picture" name="picture">
                </fieldset>
            </form>
        ```

    - 속성
        속성|설명
        :---|:---
        autofocus|페이지 로딩 후 해당 요소에 마우스 커서 표시
        placeholder|입력 란에 내용을 표시하고 클릭 시 내용이 사라짐
        readonly|읽기 전용으로 지정
        required|submit 클릭 시 필수 입력 항목을 체크
        min,max,step|해당 필드의 최대, 최소, 일정간격 지정
        size|화면에 보여지는 글자의 길이 지정
        minlength,maxlength|텍스트 최대, 최소 입력길이 지정
        height,width|type="image" 일 때 이미지의 너비와 높이 지정
        list|&lt;datalist&gt;에 미리 정의해 놓은 옵션 값을 나열해 보여줌
        multiple|type="email 또는 file" 일 때 두 개 이상의 값을 입력

    - dropdown
        ```html
            <select id="chanel" name="chanel">
                <option value="mbc" selected="selected">MBC
                <option value="ytn">YTN
            </select>

            <select id="chanel" name="chanel" size="2" multiple="multiple">
                <option value="mbc" selected="selected">MBC
                <option value="ytn">YTN
            </select>

            <select id="chanel" name="chanel">
                <optgroup label="지상파">
                    <option value="mbc" selected="selected">MBC
                    <option value="sbs">SBS
                </optgroup>
                <optgroup label="종합편성채널">
                    <option value="jtbc">JTBC
                    <option value="ytn">YTN
                </optgroup>
            </select>
        ```
    
    - datalist
        ```html
            <datalist id="teamlist">
                <option value="bears" label="두산베어스"></option>
                <option value="wyverns" label="SK와이번스"></option>
            </datalist>
        ```

    - textarea
        ```html
            <textarea rows="10" cols="30">
                이 영역은 text area에 보입니다.
            </textarea>
            <textarea rows="10" cols="30" disabled="disabled">
                이 영역은 text area에 보입니다.
                보이긴 하지만 비활성화 되어있습니다.
            </textarea>
        ```
    
    - button
        > contents를 포함할 수 있기 때문에 아이콘을 추가할 수 있고, CSS를 이용하여 원하는 형태로 꾸밀수 있음
        ```html
            <button type="submit" class="submit">
                <img src="../img/tick.png" alt=""> 전송하기
            </button>
        ```

    - progress
        > 작업의 진행 상태를 표시
        ```html
            <label>10초 남음</label>
			<progress value="50" max="60"></progress>

            <label>진행률 30%</label>
			<progress value="30" max="100"></progress>
        ```

    - meter
        > 값이 차지하는 크기 표시
        ```html
            <!-- 전체 크기 1을 기준으로 0.8만큼 차지합니다 -->
			<meter value="0.8"></meter>
            <!-- 전체 100 중에서 64만큼 차지합니다  -->
			<meter min="0" max="100" value="64"></meter>
            <!-- 전체 크기는 1024~10240까지인데 높다고 설정한 8192보다 현재 값 9216이 더 큽니다 -->
			<meter min="1024" max="10240" low="2048" high="8192" value="9216"></meter>
            <!-- 전체 1 중에서 현재 0.5를 차지하고 있으며 적정도를 0.8로 설정했습니다 -->
			<meter value="0.5" optimum="0.8"></meter>
        ```

[목차로 이동](#목차)

<br/>

### div와 span

- **div & span**
    - 태그|설명
      :---:|:---
      div|block 형식으로 공간을 분할
      &nbsp;|웹사이트의 레이아웃(전체 틀)을 만들때 사용
      &nbsp;|각각의 블록(공간)을 알맞게 배치하고 CSS를 활용하여 스타일을 적용
      span|inline 형식으로 공간을 분할
      &nbsp;|&lt;div&gt;, &lt;p&gt; tag와 함께 웹 페이지의 일부분에 스타일을 적용시키기 위해 사용
      > 두 태그간 차이점   
        >> div와 span을 여러 개 나열하였을때, div는 자동 줄 바꿈이 일어나면서 세로로, span은 줄 바꿈이 일어나지 않고 가로로 나열   
           div는 박스 형태로 영역이 설정되고 그 안에 텍스트가 정렬, span은 줄 단위로 영역이 설정되기 때문에 박스 형태가 아닌 텍스트가 노출되는 영역만 포함   
           div의 margin은 4방향 모두 적용, span의 margin은 양옆으로만 적용

<br/>

- block & lnline 형식 태그
    - block|inline
      :---|:---
      div|span
      p|a
      목록|input
      table|글자형식
      form|

[목차로 이동](#목차)

<br/>

---

## **semantic**

<br/>

- 웹문서의 레이아웃을 표준화하여 문서의 구조를 이해하기 쉽게 하고, 검색 로봇, 스크린 리더, 웨어러블 장치, 스마트 기기등에서 웹 문서를 읽는데 적합하게 만듬
    - 사이트의 제목과 로고, 검색 창 등이 있는 **헤더(header)**
    - 여러 내용이 있는 **본문(contents : section + articles)**
    - 본문 외 내용을 나타내는 **사이드바(aside)**
    - 저작권 정보와 제작자 정보를 표시하는 **푸터(footer)**

<br/>

- sementic
    - 브라우저, 검색엔진, 개발자 모두에게 콘텐츠의 **의미**를 명확히 설명하는 역할
    - sementic 웹이란 웹에 존재하는 수많은 웹페이지들에 **메타데이터**를 부여하여 잡다한 데이터집합이었던 웹페이지를 **의미**와 **관련성**을 가지는 거대한 데이터베이스로 구축하고자 하는 발상

<br/>

- non-semantic 요소 vs semantic 요소
    - non-semantic 요소 : div, span 등은 content에 대해 어떤 설명도 하지 않음
    - semantic 요소 : form, table, img 등은 content에 대해 의미를 명확히 설명
    - tag|설명
      :---|:---
      header|헤더(머리말) 지정
      nav|문서 간 네비게이션 지정
      aside|본문 이외의 내용 표시
      section|본문의 여러 내용(article)을 포함
      article|본문의 주 내용이 포함
      footer|제작정보와 저작구너 표시등 포함

<br/>

- header - 머리말지정
    - 주로 \<form> tag를 사용하여 검색 창을 넣거나 \<nav> tag를 사용하여 사이트의 메뉴를 삽입
    - 본문에 사용하여 해당 부분의 머리말로도 사용함

<br/>

- nav - 문서를 연결하는 navigation link
    - 동일 사이트 내의 문서나 다른 사이트의 문서로 연결하는 링크 모음
    - 단독으로 사용되거나 \<header>, \<footer>, \<aside> 내에서 사용

<br/>

- section - 주제별 콘텐츠 영역 표시
    - 문맥 흐름 중에서 contents를 주제별로 묶을 때 사용
    - \<section> 안에 여러 개의 \<article>을 넣어 contents의 내용을 표현

<br/>

- aside - 본문 이외의 내용 표시
    - 일반적으로 사이트의 왼쪽이나 오른쪽 또는 하단에 위치
    - 본문 내용 외 주변에 표시되는 기타 내용을 표현 (광고, 링크모음 등)

<br/>

- footer - 제작 정보와 저작권 정보등 표시
    - 일반적으로 사이트의 하단부에 표현

<br/>

[목차로 이동](#목차)