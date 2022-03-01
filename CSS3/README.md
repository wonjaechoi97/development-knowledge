# **CSS3** 🌈

## **C**ascading **S**tyle **S**heets

<br/>

- HTML 문서를 **화면에 표시하는 방식**을 정의한 언어

    - **W3C**에서 공인한 표준
    
    - 기존 웹 문서를 **다양하게 설계**하고, **변경 요구 대응**에 따르는 어려움을 보완
        > 어떤 **Style**을 적용 하느냐에 따라, **하나의 구조를 전혀 다른 페이지처럼 표현**

<br/>

- **규칙**

    ```css
    .css /*선택자*/ { 
        margin/*속성*/: 30px/*값*/; /*선언*/
        color: #000; /*선언*/
    } /*선언 블록*/
    ```
    - **선택자(selector)** 와 **선언(declaration)** 으로 구성

    - **선택자(selector)** : 규칙이 적용되는 **엘리먼트**
    - **선언(declaration)** : 선택자에 적용될 **스타일**을 작성
        > **중괄호**로 감싸며, **속성(property)과 값(value)** 으로 구성
    - **속성(property)** : 선택자에서 **바꾸고 싶은 요소**
        > color, font, width, height, border, ...
    - **값(value)** : 속성에 **적용할 값**
    - **선택자 그룹화** : **여러 선택자**에 **동일한 스타일**을 적용할 때, **comma(,)로 구분**하여 나열
    - **선언 블록** 안에 **하나 이상의 속성**을 작성할 수 있으며, 각 속성은 **semi-colon(;)** 으로 구분

<br/>

- **Style 적용방법**

    - **외부** 스타일 시트 **(External)**
        ```html
        <link rel="stylesheet" type="text/css" href="path/filename.css">
        <style type="text/css">
            @import url("path/filename.css");
        </style>
        ```
        - ***.css 파일**을 **\<link>** 또는 **@import**로 **HTML문서에 연결**하여 사용
        - **하나의 CSS파일만 수정**하면 해당 스타일시트를 사용하는 **모든 페이지에 적용**
            > 세 가지 방법중 **가장 많이 사용**

    - **내부** 스타일 시트 **(Embedded)**
        ```html
        <style type="text/css">
            h1 { color: pink }
            .classname {
                background-color: cyan;
                color: magenta;
            }
        </style>
        ```
        - ```<head>```안에 ```<style>```을 이용하여 HTML페이지 **내부에서 CSS 적용**
            > **여러 페이지에 동일한 스타일을 적용**해야 할 경우 **외부** 스타일 시트 사용

    - **인라인** 스타일 시트 **(Inline)**
        ```html
        <body>
            <h1>Inline Style</h1>
            <p style="background-color: cyan; color: magenta;">인라인 스타일시트 적용</p>
        </body>
        ```
        - **개별 엘리먼트**에 스타일을 적용
        - 세 가지 스타일 시트 중에 가장 우선순위로 반영된다