# **CSS3** 🌈

## **C**ascading **S**tyle **S**heets

<br/>

- HTML 문서를 **화면에 표시하는 방식**을 정의한 언어

    - **W3C**에서 공인한 표준
    
    - 기존 웹 문서를 **다양하게 설계**하고, **변경 요구 대응**에 따르는 어려움을 보완
        > 어떤 **Style**을 적용 하느냐에 따라, **하나의 구조를 전혀 다른 페이지처럼 표현**   
          **다양한 기기**에 맞게 탄력적으로 바뀌는 문서를 만들 수 있음

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
        <link rel="stylesheet" type="text/css" href="../css/4-1.css">
        <style type="text/css">
            @import url("path/filename.css");
            /* 또는 */
            @import "../css/4-1.css";
        </style>
        ```
        - ***.css 파일**을 **\<link>** 또는 **@import**로 **HTML문서에 연결**하여 사용
            > CSS 파일 내부에서도 @import를 사용하여 외부 CSS를 로드할 수 있음   
              속도 측면에서 link tag사용에 이점이 있음
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
            > **여러 페이지에 동일한 스타일을 적용**해야 할 경우엔 유지보수를 위해 **외부** 스타일 시트 사용

    - **인라인** 스타일 시트 **(Inline)**
        ```html
        <body>
            <h1>Inline Style</h1>
            <p style="background-color: cyan; color: magenta;">인라인 스타일시트 적용</p>
        </body>
        ```
        - **개별 엘리먼트**에 스타일을 적용
        - 세 가지 스타일 시트 중에 가장 우선순위로 반영된다

<br/>

- **선택자(selector)**
    - 일반 선택자 : 전체 선택자, 타입 선택자, 클래스 선택자, ID 선택자로 분류
    - 복합 선택자 : 자식 선택자, 하위 선택자, 인접 형제 선택자, 일반 형제 선택자로 분류
    - 그 외 가상 클래스 선택자, 가상 엘리먼트 선택자, 속성 선택자가 존재
    - 선택자|의미|사용법|CSS
      :---|:---|:---:|:---:
      전체 선택자|HTML 문서 내 모든 element 선택|* { }|2
      타입 선택자|매칭되는 element 선택|h1, h2, h3 { }|1
      클래스 선택자|class 속성 값과 매칭되는 element 선택|.className { }|1
      ID 선택자|id 속성 값과 매칭되는 element 선택|#idName { }|1
      하위 선택자|하위 element 선택|E1 E2 { }|1
      자식 선택자|직속 하위 element 선택|E1>E2 { }|2
      인접 형제 선택자|인접 형제(sibling) 관계인 element 선택|E1+E2 { }|2
      일반 형제 선택자|형제(sibling) 관계인 element 선택|E1~E2 { }|3

<br/>

- **일반 선택자**
    > 우선 순위 : 전체 선택자 < 타입 선택자 < 클래스 선택자 < ID 선택자
    - 전체 선택자 : * { }
        > HTML 문서 내 모든 element를 선택   
          잘 사용되지 않으며 우선 순위가 가장 낮음
        ```html
            <style type="text/css">
        		* {
        			background: blue;
        		}
        	</style>
        ```
    - 타입 선택자 : elementName { } 
        > tag명을 이용하여 스타일을 적용할 tag를 선택   
          1개 이상의 HTML 엘리먼트를 사용할 수 있고, 여러 엘리먼트를 선택할 때는 comma(,)로 구분
        ```html
            <style type="text/css">
                div,p {
                    padding: 10px;
                }

                div {
                    background: skyblue;
                }
            </style>
        ```
    - 클래스 선택자 : .className { }
        > 클래스 명은 공백 없이 대소문자 또는 Hypen(-), UnderScore(_)로 시작 (기호나 숫자 X)   
          HTML 문서에서 동일한 클래스 명을 중복해서 사용 가능 (공통 속성 적용)   
          class 속성 값에 하나 이상의 클래스를 적용 가능 (공백으로 구분)
        ```html
            <style type="text/css">
                .target1 {
                    background: skyblue;
                }

                p.target1 {
                    font-weight: bold;
                }
            </style>
        ```
    - ID 선택자 : #IDName { }
        > HTML 문서에서 동일한 ID를 중복 사용할 수 없다   
          id 속성 값엔 1개의 id만 사용가능   
          일반 선택자 중 가장 우선순위가 높다
        ```html
            <style type="text/css">
                #target1 {
                    background: skyblue;
                }
            </style>
        ```

<br/>

- **복합 선택자**
    - 하위 선택자(Descendant) : element element { }
        > 1단계 하위 요소(child)와 2단계 이상 하위 요소(descendant) 모두 적용
    - 자식 선택자(Child) : element > element { }
        > 1단계 하위 요소(child)에만 적용
        ```html
            <style type="text/css">
                div p{
                    background: lightgray;
                }

                div>p {
                    background: purple;
                }
            </style>
        ```
    - 인접 형제 선택자(Adjacent Sibling) : element + element { }
        > 형제(sibling) 관계인 엘리먼트가 여러 개 존재할 경우 첫 번째 엘리먼트만 선택
    - 일반 형제 선택자(General Sibling) : element ~ element { }
        > 형제(sibling) 관계인 엘리먼트가 여러 개 존재할 경우 모든 엘리먼트 선택
        ```html
            <style type="text/css">
                div+p {
                    background: lightgray;
                }

                div.target~p {
                    background: green;
                }
            </style>
        ```

<br/>

- **가상 클래스 선택자** : :가상 클래스 { }
    - User Agent가 제공하는 가상의 클래스를 지정   
    - 선택자|의미|CSS   
      :---|:---|:---:
      :link|방문하지 않은 링크를 선택|1
      :visited|방문한 링크를 선택|1
      :hover|지정된 요소에 마우스가 올라간 경우 선택|1
      :active|지정된 요소가 활성화 된 경우 선택|1
      :focus|지정된 요소가 포커스를 가질 경우 선택|2
      :first-child|지정된 요소 중 부모의 첫 번째 자식 선택|2
      :last-child|지정된 요소 중 부모의 마지막 자식 선택|3
      :nth-child(n)|지정된 요소 중 n번째 자식 선택(n=0,1,...)|3
      :enabled|지정된 요소가 enabled인 경우 선택|3
      :disabled|지정된 요소가 disabled인 경우 선택|3
      :checked|지정된 요소가 checked인 경우 선택|3

        > :link, :visited, :hover: :active 는 IE 6.0에서 지원하지 않음   
          :focus 는 IE 6.0 7.0 에서 지원하지 않음
        ```html
            <style type="text/css">
                a:link { color: gray; }
                a:visited { color: red;}
                a:hover { color: green; }
                a:active { color: blue; }
            </style>
        ```
        > :nth-child(n) 에서 n은 0부터 시작, 자식 순번은 1부터 시작
        ```html
            <style type="text/css">
                li:nth-child(5) { color: magenta; }
                li:nth-child(3n+1) { color: skyblue; }
                li:last-child { color: orange; }
            </style>
        ```

- **가상 엘리먼트 선택자** : ::가상 엘리먼트 { }
    > CSS1과 CSS2 에선 single colon(:)을 사용했으나   
      CSS3에선 가상 클래스와 가상 엘리먼트를 구별하기 위해 double colon(::)으로 대체

    선택자|의미|CSS
    :--|:--|:--:
    ::after|지정된 요소 뒤에 content 추가|2
    ::before|지정된 요소 앞에 content 추가|2
    ::first-letter|지정된 요소의 첫 번째 문자 선택|1
    ::first-line|지정된 요소의 첫 번쨰 라인 선택|1
    ::selection|사용자에 의해 선택된 요소의 위치 선택|3
    > ::after, ::before 는 IE9.0부터 지원 (8.0은 single colon CSS2 문법만 지원)   
      ::first-letter, ::first-line 은 IE9.0부터 지원 (5.5~8.0은 single colon CSS2 문법만 지원)   
      ::selection은 IE9.0부터 지원   
    ```html
        <style type="text/css">
            p.intro::first-letter { font-size: 200%; }
            p.intro::first-line { font-weight: bold; }
            body:last-child::after {
                content: "(source: www.w3schools.com)";
                color: orange;
            }
            ::selection { color: steelblue; }
        </style>
    ```

<br/>

- **속성 선택자**
    - 특정한 속성을 가지거나 속성 값을 갖는 엘리먼트를 선택
        > Existence[], Equality=, Space~=, Prefix^=, Substring*= 등이 있음   
          속성 선택자를 사용하기 위해서는 HTML 문서를 작성할 때 name, title 등의 속성값을 규칙적으로 정의해야 함   
          화면에 같은 분류의 많은 항목들을 일괄적으로 선택할 때 유용(ex. 특정 이름을 갖는 체크박스)
        
        선택자|의미|CSS
        :---|:---|:---:
        [A]|A속성이 포함된 엘리먼트 선택|2
        [A=V]|A속성 값이 V와 정확히 일치하는 엘리먼트 선택|2
        [A~=V]|A속성 값이 V단어(space로 구분)를 포함하는 엘리먼트 선택|2
        [A^=V]|A속성 값이 V로 시작하는 엘리먼트 선택|3
        [A*=V]|A속성 값이 V를 포함하는 엘리먼트 선택|3
        [A$=V]|A속성 값이 V로 끝나는 엘리먼트 선택|3
        [A\|=V]|A속성 값이 정확히 V이거나, V-로 시작하는 엘리먼트 선택|2
        ```html
            <style type="text/css">
                [title] { color: steelblue; }
                [title="two"] { border: 2px solid pink; }
                p[title="two"] { color: orange; }
                p[title~="second"] { background: silver; }
                p[class^="second"] { background: cyan; }
                p[class$="wrap"] { color: deeppink; }
                [class*="three"] { background: green; }
                [class|="second"] { border: 2px dotted darkgray; }
            </style>
        ```

<br/>

- **CSS 규칙 적용 우선순위**
    - 같은 엘리먼트에 두 개 이상의 CSS 규칙이 적용된 경우
        > 마지막 규칙, 구체적인 규칙, !important가 우선적용
        >> CSS 규칙들 중 하단에 작성한 규칙이 마지막 규칙   
           p { } 보다 p b { } 더 구체적   
           속성 값 뒤에 !important 작성시 같은 엘리먼트에 대해 우선적으로 적용
        ```html
            <style type="text/css">
                p b { color: green !important; }
                p b { color: gray; }
            </style>
        ```

<br/>

- **Font**
    속성|의미|CSS
    :---|:---|:---:
    font-family|글꼴 지정|1
    font-size|글자 크기 지정|1
    font-style|글자 스타일 지정|1
    font-variant|소문자를 작은 대문자로 변형|1
    font-weight|글자 굵기 지정|1
    font|font에 관한 속성을 한번에 지정하는 단축형 속성|1

    - font-family : E { font-family : 글꼴이름, 글꼴이름, ...}
        > 앞의 글꼴부터 읽으며 글꼴이 사용자 PC에 없을 경우 다음 글꼴 적용   
          font name에 공백이 포함된 경우 quotation("")으로 감싸야한다
        ```html
            <style type="text/css">
                #monospace { font-family: monospace, "Times New Roman"; }
            </style>
        ```
    - font-style : E { font-style : normal \| italic \| oblique }
        > 글자 스타일을 지정하며 기본값은 normal   
    - font-variant : E { font-variant : normal \| small-caps }
        > 소문자를 작은 대문자로 변환하며 초기값은 normal
    - font-weight : E { font-weight : normal \| bold \| bolder \| lighter }
        > 초기값은 normal이며, 100 ~ 900 까지 숫자 값으로 지정   
          400 : normal, 700 : bold
    - font-size : E { font-size : 속성 값 }
        > 절대 사이즈 속성 값 : xx-small, x-small, small, medium, large, x-large, xx-large   
          상대 사이즈 속성 값 : larger, smaller   
          그 외 px, cm, %(부모 엘리먼트와의 비율) 단위도 사용
    - font : E { font: font-style font-variant font-weight font-size/line-height font-family }
        > font-size 와 font-family 는 필수 값, 나머지는 생략 시 기본 값 적용   
          순서에 맞지 않게 작성 시 일부만 적용되거나 전체가 무시될 수 있음

<br/>

- **text**
    > text 관련 속성은 글자, 공간, 단어, 문단들이 보여지는 속성을 정의   
      들여쓰기를 위해 &nbsp;를 사용하지 않고 text-indent 속성을 사용하여 들여쓰기를 적용할 수 있음
    
    속성|의미|CSS
    :---|:---|:---:
    text-align|text 정렬 방식 지정|1
    text-decoration|text 장식 지정|1
    text-indent|text-block 내 첫 라인의 들여쓰기 지정|1
    text-transform|text 대문자화|1
    white-space|엘리먼트 안의 공백 지정|1
    vertical-align|수직 정렬 지정|1
    letter-spacing|문자 간의 space 간격을 줄이거나 늘림|1
    word-spacing|단어 간의 간격 지정|1
    line-height|줄(행) 간격 지정|1
    color|text 색상 지정|1

    - text-align : E { text-align: left \| right \| center \| justity }
        > justify : 각 라인의 너비가 동일 하도록 간격을 늘린다
    - text-decoration : E { text-decoration: none \| underline \| overline \| line-through \| blink }
    - text-indent : E { text-indent: 절대값(px,pt,em,cm ...) \| 배율(%) }
    - text-transform : E { text-transform: capitalize \| uppercase \| lowercase \| none }
        > capitalize : 첫 글자를 대문자   
          uppercase : 글자 전체를 대문자   
          lowercase : 글자 전체를 소문자
    - white-space : E { white-space: normal \| pre \| nowrap \| pre-line \| pre-wrap }
        > normal : 정해진 영역에 따라 줄이 변경, 하나의 whitespace만 허용   
          pre : 사용자가 입력한 모습 그대로 공백을 화면에 출력   
          nowrap : 하나의 whitespace만 허용하며, 줄 바꿈 금지
    - letter-spacing : E { letter-spacing: normal \| 길이 값(length) }
        > 글자 간 간격 조절
    - word-spacing : E { word-spacing: normal \| 길이 값(length) }
        > 단어 간 간격 조절
    - line-height : E { line-height: 상대값 \| 절대값 \| 비율 }

<br/>

- 사용자 인터페이스 속성
    > 화면에 출력될 엘리먼트들에 디자인 요소를 추가하는 속성   
      커서의 모양이나 리스트 형태를 변경   
      문서의 배경색과 배경 이미지를 변경
      엘리먼트가 화면에 출력되는 방식을 조정

    속성|의미|CSS
    :---|:---|:---:
    cursor|사용자 환경의 마우스 모양을 변경|2
    classification|리스트의 글머리 기호를 변경|1
    display|엘리먼트가 화면에 출력되는 방식을 조정|1
    background-color|배경색을 지정|1
    background-image|배경을 이미지로 지정|1
    background-attachment|배경 이미지를 고정하거나 scroll 여부를 지정|1
    background-repeat|배경 그림의 반복 여부를 지정|1
    background-position|배경 그림의 위치를 지정|1
    background|배경 관련 속성을 한번에 지정 (속성 값 순서 상관 없음)|1

    - display : E { display: none \| block \| inline \| ... }
    - background-color : E { background-color: color \| transparent }
        ```html
            <head>
                <style type="text/css">
                    .transparent { background-color: transparent;  }
                    .notTransparent { background-color: black; }
                </style>
            </head>
            <body>
                <div class="transparent">배경이 투명.</div>
                <div class="notTransparent">배경이 불투명.</div>
            </body>
        ```
    - background-image : E { background-image: none \| url("그림파일경로") }
        > 배경 이미지가 격자모양으로 반복하여 나타남
    - background-repeat : E { background-repeat: repeat-x \| repeat-y \| no-repeat }
        > repeat-x 는 배경을 수평으로 반복   
          repeat-y 는 배경을 수직으로 반복
    - background-position : E { background-position: 백분율(%) \| 길이값 \| 수평값 \| 수직값 }
    
<br/>

- 테이블&amp;테두리
    > \<table> 엘리먼트의 관련 속성은 테이블의 너비나 높이를 지정하고, Cell 내부 내용을 정렬
    >> 관련 속성으로 table-layout, width, height, text-align, vertical-align 등이 있음
    
    > 테두리 관련 속성은 테두리 모델을 설정하여, 테두리 스타일과 너비 등을 지정
    >> 관련 속성으로 border-collapse, border-style, border-width, border-color 등이 있음

    속성|의미|CSS
    :---|:---|:---:
    table-layout|table layout을 설정|2
    width|table의 너비를 지정|1
    height|table의 높이를 지정|1
    text-align|Cell 내부 내용을 수평 정렬|1
    vertical-align|Cell 내부 내용을 수직 정렬|1
    border-collapse|분리된 테두리모델, 통합된 테두리모델을 설정|2
    border-style|테두리 스타일을 설정|1
    border-width|엘리먼트의 4개 테두리 너비를 설정|1
    border-color|엘리먼트의 테두리 색을 설정|1
    border|테두리 관련 속성을 한번에 지정하는 단축형 속성|1

    - table-layout : E { table-layout: auto(default) \| fixed }
        > table cell의 width, height 고정 여부를 지정   
    - table의 width와 height는 첫 번째 \<td> 에 지정하거나, \<colgroup>과 \<col>요소를 통해 지정
    - border-collapse : E { border-collapse: separate(defalut) \| collapse }
    - border-spacing : E { border-spacing: 수평길이 수직길이 }
    - border-style : E { border-style: none \| solid \| hidden \| ... }
        > 값이 1개일 때 모든면 적용   
          값이 2개일 때 첫 번째는 top, bottom, 두 번째는 right, left 적용   
          값이 3개일 때 첫 번째는 top, 두 번째는 right, left, 세 번째는 bottom 적용   
          값이 4개일 때는 top, right, bottom, left 순으로 적용
    - border-width : E { border-width: thin \| medium \| thick \| 길이 값 }    
        > border-style 이 border-width 보다 앞에 정의되어야 함
    - border-color : E { border-color: color \| transparent }
    - border
        > border-width, border-style, border-color 순서로 작성해야 함   
          margin, padding의 약식 속성과 달리 4개의 테두리에 다른 값을 설정하지 못함

<br/>

- 박스모델
    > CSS의 모든 엘리먼트는 content, padding, border, margin으로 둘러 쌓여 있음   
      컨텐츠를 정렬 또는 위치를 지정하기 위해 padding, margin 속성 활용   
      박스모델을 통해 다양한 형태의 테이블이나 버튼을 직접 작성하여 활용함
    
    - margin
        > 값이 1개일 때 모든면 적용   
          값이 2개일 때 첫 번째는 top, bottom, 두 번째는 right, left 적용   
          값이 3개일 때 첫 번째는 top, 두 번째는 right, left, 세 번째는 bottom 적용   
          값이 4개일 때는 top, right, bottom, left 순으로 적용

        > 두 개 이상의 블록레벨 인접 수직 마진은 병합됨, 수평 마진은 병합되지 않음   
          floated 박스는 다른 박스와 수직 마진 병합이 일어나지 않음   
          position이 absolute와 relative로 위치된 박스 간 마진들은 병합되지 않음
    - padding
        > 값이 1개일 때 모든면 적용   
          값이 2개일 때 첫 번째는 top, bottom, 두 번째는 right, left 적용   
          값이 3개일 때 첫 번째는 top, 두 번째는 right, left, 세 번째는 bottom 적용   
          값이 4개일 때는 top, right, bottom, left 순으로 적용

    - 가운데 정렬 E : { margin: 0 auto }
        > margin 속성을 사용하여 가운데에 정렬되도록 설정 가능   
          첫 번째 값은 상,하 여백이고 두 번째 값은 좌, 우여백임   
          속성값 auto는 현재 엘리먼트를 중심으로 상,하 또는 좌,우 여백을 균등히 분배함
    
    - min-height
        > 화면의 크기 변화에 관계 없이 엘리먼트의 최소 높이를 유지함 

<br/>

- 포지셔닝
    > HTML 내 부분 문서의 위치를 지정하거나 객체의 시각화여부를 지정함   
      엘리먼트의 위치를 고정시키거나 브라우저의 크기에 따라 이동하는 등의 설정을 함   
      정적인 HTML을 JavaScript를 이용하여 동적으로 만들기 위한 가장 기본적인 속성
    
    - width, height
        > length : px, cm 등의 길이 단위 사용   
          백분율(%) : 상위 block에 대한 백분율 단위로 상위 block의 크기가 바뀌면 자신의 크기도 자동으로 변경   
          auto(width) : 100%, 자신의 상위 block이 허용하는 width 크기만큼 채움   
          auto(height) : 0%, height를 결정하는 요인은 block box 속의 내용물의 크기

    - position
        > static : 기본값(default)으로 일반적인 내용물의 흐름, 상단과 좌측에서의 거리를 지정할 수 없음   
          relative : HTML 문서에서의 일반적인 흐름이지만 상단과 좌측 거리를 지정할 수 있음   
          absolute : 자신의 상위 box 속에서의 절대적인 위치를 지정   
          fixed : 스크롤(scroll)이 일어나도 화면상의 위치를 고정   

    - top, left, bottom, right
        > 엘리먼트의 위치를 지정하기 위해 사용   
          자신이 포함되어 있는 박스 속에서의 위치를 지정

    - overflow
        > 상위 엘리먼트에 속한 내용이 엘리먼트의 크기보다 클 경우의 처리 설정   
          visible로 설정 시 모두 표시, 내용의 크기에 따라 가로 세로 폭이 늘어남   
          hidden으로 설정 후 box의 width, height를 지정 시, 범위 초과 내용은 노출되지 않음   
          auto로 설정 시 범위 초과 내용은 스크롤바를 이용하여 표시

    - float
        > 박스를 화면의 어디에 배치할 것인지 설정   
          left : 그림이나 박스가 왼쪽에 배치되고, 글씨는 오른쪽으로 흐른다   
          right : 그림이나 박스가 오른쪽에 배치되고, 글씨는 왼쪽으로 흐른다   
          none : 그림이나 박스가 왼쪽에 배치되고, 글씨는 흐르지 않는다

    - clear
        > float 속성이 가지고 있는 값을 초기화   
          left, right : left 또는 right 속성값 취소   
          both : 양쪽 모두의 float 속성값 취소   
          none : clear를 설정하지 않은 것과 같음

    - clip
        > 이미지 사이즈가 클 경우 이미지를 일부 가려서 표시   
          auto로 설정하면 가리지 않고 모두 보여줌
        ```html
            <style type="text/css">
                .auto { clip: rect(auto); }
                .top { clip: rect(15px auto auto auto); }
                .right { clip: rect(auto 15px auto auto); }
                .bottom { clip: rect(auto auto 15px auto); }
                .left { clip: rect(auto auto auto 15px); }
            </style>
        ```

    - visibility
        > 엘리먼트를 화면에 보이거나 숨김   
          hidden : 화면에서 숨기지만 면적은 차지하고 있음   
          화면에서 숨기고 면적도 차지하지 않도록 하려면 display 속성 사용
        ```html
            <style type="text/css">
                .visible { visibility: visible; }
                .hidden { visibility: hidden; }
                .block { display: block; }
                .none { display: none;  }
            </style>
        ```
    
    - z-index : E { z-index: 정수값(양수, 음수 가능) }
        > 여러 엘리먼트를 화면에 쌓아서 표시할 때 표시 순위를 결정   
          z-index 값이 큰 엘리먼트가 표시됨   
          auto로 설정 시 부모 엘리먼트과 같은 값으로 설정(기본값)
