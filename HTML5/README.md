# **HTML5** 📃

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

<br/>

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

