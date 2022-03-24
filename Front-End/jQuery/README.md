# **jQuery** ✂️

## 목차
  - [개요](#jquery-소개)
  - [DOM 요소 선택 (Selector)](#dom-요소-선택-selector)
  - [DOM 객체](#dom-객체)
  - [jQuery Event](#jquery-event)

<br

---

## jQuery 소개

<br>

- 개요
    - jQuery는 John Resig이 2006년 발표한 **크로스플랫폼**을 지원하는 경량 **javascript library**

    - HTML 문서의 탐색이나 조작, 이벤트 핸들링, 애니메이션, Ajax 등을 멀티 브라우저를 지원하는 API를 통해 간편하게 사용

- 특징
    - **크로스 플랫폼**을 지원하므로 어떠한 브라우저에서나 **동일하게 동작**
        > 브라우저 호환성을 고려하여 **대체코드**를 작성할 필요가 없음

    - 네이티브 DOM API (DOM Query, Traversing, Manipulation 등) 보다 **직관적이고 편리한** API 제공하여 개발 속도를 향상
    - **Event 처리, Ajax, Animation 효과**를 쉽게 사용
    - 다양한 Effect 함수를 제공하여 **동적인 페이지**를 쉽게 구현

<br>

- 사용 설정 두가지 방법
    ```html
        <!-- 다운로드후 링크설정 -->
        <script type="text/javascript" src="../js/jquery-3.xx.x.min.js"></script>

        <!-- CDN (Content Delivery Network) 을 권장 -->
        <!-- Google CDN -->
        <script src="https://ajax.goolgleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>

        <!-- Microsoft CDN -->
        <script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.5.1.min.js"></script>
    ```

- 기본구문
    - **Selector를 사용**하여 DOM 객체를 탐색하고, 반환된 래퍼세트를 통해 함수를 수행함
        > 래퍼세트 객체는 **메서드 체인**을 제공하여, 메서드 호출에서의 **반복적인 코딩**을 줄여줌

    - Selector 표현식과 Action 메서드를 조합한 형태로 구문을 작성
        > **`$(selector).action();`**
    - jQuery로 DOM 을 탐색하기 전에, 웹 브라우저에 **문서가 모두 로드**되어 있어야 함
    - jQuery는 **Document Ready 이후 처리**할 수 있는 두 가지 방법을 제공
        ```js
            // 1번
            $(document).ready(function() {
                // todo
            });

            // 2번 권장함
            $(function(){
                // todo
            });

            $(document).ready(function () {
                $('button').click(function () {
                    $('#p1').css('background', 'orange');
                });
            });
        ```

<br>

[목차로 이동](#목차)

---

<br>

## DOM 요소 선택 (Selector)

<br>

- DOM (Document Object Model) 탐색
    - DOM (Document Object Model) 은 HTML과 XML 문서의 구조를 정의하고 API 제공

    - DOM 객체의 API를 사용하는 단순 js를 이용한 DOM 탐색도 가능하지만, 단순한 작업임에도 많은 코드를 작성해야 함
    - jQuery는 CSS에서 사용하는 **Selector 표현 방식**을 사용하여 DOM 탐색
        - 브라우저가 표준 CSS 선택자를 올바르게 구현하지 않았더라도, jQuery는 W3C 표준에 맞게 요소를 탐색

        - DOM 탐색 결과를 래퍼세트 (Wrapper Set) 라는 DOM을 래핑 한 객체를 반환

- 선택자 (Selector)
    - CSS 문법을 확장하여 selector를 이용한 DOM Element의 검색은 특정 브라우저에 의존적인 스크립팅을 벗어날 수 있음

    - jQuery function의 기본 형식
        > $ : jQuery   
          ("h1") : selector   
          .css("color", "blue"); : function

- DOM 요소 탐색 - **요소선택자 (Element Selector)**
    - jQuery 선택자는 DOM 객체를 탐색하여 래퍼세트 객체로 반환
    - jQuery 선택자에는 **요소, ID, 클래스, 다중, 복합 그리고 모두 선택자** 등이 있음
        selector 종류|selector 표현방법
        :---|:---
        All selector|$("*")
        ID selector|$("#id")
        Element selector|$("elementName")
        Class selector|$(".className")
        Multiple selector|$("selector1, selector2, selector3, ...")

    - All selector
        - CSS의 가장 기본 선택자로써 HTML page에 있는 모든 문서 객체를 선택
        - 주로 HTML Layout을 확인하기 위한 용도로 사용

    - ID selector
        - 특정 id 속성을 가지고 있는 요소를 선택

        - 웹 표준에서 id는 html page에서 단 하나의 태그에만 적용
            > 선택자 중 사용빈도 높음
        - 만약 ID가 중복된 경우, 첫 번째 요소를 반환
        - ID 선택자는 다양한 선택자 중 DOM 객체를 가장 빨리 탐색
        - DOM API 의 document.getElementById() 와 같음

    - Element selector
        - 특정 element를 선택

        - DOM 요소 명(태그 이름)이 일치하는 것들을 모두 찾음
        - DOM API 의 document.getElementsByTagName() 과 같음

    - Class selector
        - 특정 class 속성을 가지고 있는 요소를 선택

        - DOM API의 document.getElementByClassName() 과 같음

    - Complex selector
        - class 속성이 2개인 경우, AND 조건을 표현

        - 복합 선택자는 주로 selector + Class selector 조합으로 사용

    - Multiple selector
        - 여러 element를 선택

        - 선택할 element가 여러 개일 경우 콤마 (,) 로 구분
        - 나열된 선택자를 하나라도 만족하는 DOM 객체를 반환

- DOM 계층구조 탐색 
    - DOM Hierarchy Selector

        - jQuery는 HTML DOM 계층 구조에 접근하고 제어하는 쉬운 방법을 제공

        - DOM 요소들은 **수평적으로 형제요소**, **수직적으로 부모, 자식, 자손, 조상요소**로 관계를 구분함
            selector 종류|selector 표현방법
            :---|:---
            Child selector|$("parent > child")
            Descendant selector|$("ancestor descendant")
            Next Adjacent selector|$("previous + next")
            Next Siblings selector|$("previous ~ siblings")

- DOM 속성 탐색 - 속성선택자 (Attribute selector)
    - 속성 선택자를 사용하여 DOM 요소가 가진 속성 값으로 DOM 객체 탐색
    - 속성 선택자 표현식은 기본 선택자 바로 뒤에 나오는 대괄호 ([])안에 표현
    - 기본 선택자를 생략하면 모든 요소에 대해 주어진 속성 선택자를 비교
    - 입력 양식과 관련된 요소를 선택할 때 사용

        selector 형식|설명
        :---|:---
        $(selector[attr])|attr을 속성값으로 가지는 문서객체 선택
        $(selector[attr="value"])|attr의 속성값이 value와 같은 문서객체 선택
        $(selector[attr!="value"])|attr의 속성값이 value와 같지 않은 문서객체 선택
        $(selector[attr~="value"])|attr의 속성값이 공백과 함께 value를 포함하는 문서객체 선택
        $(selector[attr^="value"])|attr의 속성값이 value로 시작하는 문서객체 선택
        $(selector[attr\$="value"])|attr의 속성값이 value로 끝나는 문서객체 선택
        $(selector[attr*="value"])|attr의 속성값이 value를 포함하는 문서객체 선택

- DOM 요소 필터링 - 필터선택자 (Filter selector)
    - 필터 선택자는 DOM 요소를 탐색한 결과에서 원하는 요소를 걸러 내기 위하여 사용
    - 기본 선택자 뒤에 콜론 (:) 기호와 함께 표기
    - jQuery에는 이 외에도 위치 기반 필터 선택자와 함수 기반 필터 선택자가 있음
    
        필터 선택자|설명
        :---|:---
        el:button|type이 button인 input 요소 또는 button 요소 선택
        el:checkbox|type이 checkbox인 input 요소 선택
        el:file|type이 file인 input 요소 선택
        el:image|type이 image인 input 요소 선택
        el:password|type이 password인 input 요소 선택
        el:radio|type이 radio인 input 요소 선택
        el:reset|type이 reset인 input 요소 선택
        el:submit|type이 submit인 input 요소 선택
        el:text|type이 text인 input 요소 선택
        el:checked|체크된 입력 폼 선택
        el:disabled|비활성화된 입력 폼 선택
        el:enabled|활성화된 입력 폼 선택
        el:focus|초점이 맞추어진 입력 폼 선택
        el:input|모든 입력 폼 선택(input, textarea, select, button ...)
        el:selected|option 객체 중 선택된 요소 선택
        el:hidden|감춰진 요소 선택
        el:visible|보이는 요소 선택
        el:header|헤더 엘리먼트만 선택 (<\h1> ~ \<h6>)

    - 위치 기반 필터선택자
        - 선택한 요소들의 위치를 기반으로 필터를 수행

        필터선택자|설명
        :---|:---
        :first|첫 번째 요소 선택
        :last|마지막 요소 선택
        :first-child|첫 번쨰 자식 요소 선택
        :last-child|마지막 자식 요소 선택
        :only-child|형제가 없는 모든 요소 선택
        :even|짝수 번째 요소 선택
        :odd|홀수 번째 요소 선택

    - 함수 기반 필터선택자
        - jQuery는 함수 형태의 필터 선택자를 제공

            필터 선택자|설명
            :---|:---
            :not(filter)|주어진 선택자와 일치하지 않는 모든 요소를 선택
            :contains(str)|텍스트 str을 포함하는 요소만 선택
            :nth-child(n)|n번째 자식 요소를 선택
            :eq(n)|n번째로 일치하는 요소를 선택
            :gt(n)|n번째(포함x) 이후 요소를 선택
            :lt(n)|n번째(포함x) 이전 요소를 선택
            :has(f)|주어진 선택자와 일치하는 하나 이상의 요소를 포함하는 요소를 선택

            > nth-* 계열 필터의 n값은 1부터 시작하는 index 사용   
                eq와 같은 필터는 n값이 0부터 시작하는 js의 0기반 index 사용   

- jQuery method 
    - 래퍼세트와 메서드
        - jQuery는 선택자를 통해 탐색한 DOM 객체들을 래퍼세트라는 특별한 배열 객체에 담아 반환
        - 선택된 DOM 객체가 없는 경우에도, 비어 있는 래퍼세트 객체를 반환
        - 래퍼세트 객체에는 내포된 DOM 객체들을 처리하는 다양한 메서드가 있음
        - jQuery 메서드는 플러그-인 확장을 통해 추가할 수 있음
        
            메서드|반환값|설명
            :---|:---|:---
            size()|요소개수|래퍼세트의 요소 개수 반환
            get(index)|DOM 요소|래퍼세트에서 인덱스 번호에 위치하는 DOM 객체 반환
            index(element)|인덱스 번호|래퍼세트에서 해당 요소의 인덱스 번호 반환
            add(expr)|래퍼세트|expr으로 명시한 요소를 래퍼세트에 추가
            not(expr)|래퍼 세트|expr으로 명시한 요소를 래퍼세트에서 제거
            **each(function(index, element))**|이전 래퍼세트|래퍼세트의 각 요소마다 function을 수행
            filter(expr)|래퍼세트|expr에 명시한 요소를 필터링
            slice(begin, end)|래퍼세트|현재 래퍼세트의 일부분으로 새로운 래퍼세트를 생성하여 반환

    - 계층 구조 탐색

        메서드|설명
        :---|:---
        parent([selector])|래퍼세트에 포함된 각 요소의 부모요소로 구성된 래퍼세트 반환
        parents([selector])|래퍼세트에 포함된 각 요소의 조상요소로 구성된 래퍼세트 반환
        children([selector])|래퍼세트에 포함된 각 요소의 자식요소로 구성된 래퍼세트 반환
        prev([selector])|래퍼세트에 포함된 각 요소의 바로 이전 형제요소로 구성된 래퍼세트 반환
        prevAll([selector])|래퍼세트에 포함된 각 요소의 이전에 나오는 형제요소들로 구성된 래퍼세트 반환
        next([selector])|래퍼세트에 포함된 각 요소의 바로 이후 형제요소로 구성된 래퍼세트 반환
        nextAll([selector])|래퍼세트에 포함된 각 요소의 이후에 나오는 형제요소들로 구성된 래퍼세트 반환
        siblings([selector])|래퍼세트에 각 요소를 제외한 모든 형제요소들로 구성된 래퍼세트 반환

    - 요소 반복
        - jQuery.each() 함수는 배열이나 객체를 반복적으로 처리할 때 사용
            > `$.each(array,function(index, item) { });`
            >> 첫 번째 인자에는 자바스크립트 배열이나 래퍼세트 객체가 올 수 있음   
               두 번째 인자에는 각 요소를 반복하면서 처리할 콜백(call back)함수를 정의   
               콜백 함수는 두 개의 매개변수(index:배열 인덱스, item:반복하는 요소객체)를 가짐

        - jQuery 선택자의 결과인 래퍼세트는 탐색할 DOM 요소들을 반복하는 each() 메서드를 제공
            - $.each() 함수와 달리 반복대상을 의미하는 첫 번째 인자가 없으며, 래퍼세트의 DOM 요소들이 반복 대상
            - 래퍼세트의 each() 메서드는 유일한 인자인 콜백 함수가 있음
                > `$(selector).each(function(index, item) {});`
    
    - 필터 함수
        - filter()는 래퍼세트에 포함된 DOM 요소를 주어진 조건으로 걸러내는 메서드

        - filter() 메서드는 기존 래퍼세트에서 DOM 요소를 축소한 새로운 래퍼세트를 반환
        - filter() 메서드 인자에는 필터링 조건을 나타내는 선택자나 함수가 올 수 있음
            > `$(selector).filter(selector);`   
              `$(selector).filter(function(index, item) {});`
        - 필터 선택자를 사용하여 필터링 조건을 나타내기 어려운 경우 함수를 사용
        - filter() 메서드를 호출하여 필터링한 래퍼세트에서 end()메서드를 호출하면 이전 래퍼세트로 돌아감

    - 위치기반 함수
        - HTML DOM 객체의 특정 위치를 선택할 수 있는 함수

        - eq() 메서드는 래퍼세트에서 주어진 인덱스번호에 해당하는 DOM 요소를 감싼 새로운 래퍼세트를 반환
        - first() 메서드는 래퍼세트의 첫 번째 DOM 요소를 감싼 새로운 래퍼세트를 반환
        - last() 메서드는 래퍼세트의 마지막 DOM 요소를 감싼 새로운 래퍼세트를 반환
            > `$(selector).eq(index);`   
              `$(selector).first();`   
              `$(selector).last();`   

    - 래퍼세트에 요소 추가/삭제
        - 필터 함수와 위치 기반 함수는 래퍼세트에 포함된 요소를 축소하기 위해 사용

        - jQuery는 래퍼세트에 요소를 추가하거나 특정 요소를 삭제하는 메서드를 제공함
        - add() 메서드는 래퍼세트에 주어진 조건을 만족하는 요소를 추가한 새로운 래퍼세트를 반환
        - not() 메서드는 래퍼세트에서 주어진 조건에 해당하는 요소를 제거한 새로운 래퍼세트를 반환
            > `$("element");`   
              `.add("element");`   
              `.not("element");`

    - DOM 요소 판별
        - is() 메서드는 기존 래퍼세트가 주어진 선택자와 일치하는지 여부를 반환

        - 다른 메서드들이 새로운 래퍼세트 객체를 반환하는 반면, is() 메서드는 boolean 값을 반환
        - 비교할 조건을 선택자로 표현하기 어려운 경우, 함수를 사용할 수 있음
        - 비교 조건을 나타내는 함수 호출결과가 true인 경우, is() 메서드는 true를 반환
            > `$(selector).is(selector);`
              `$(selector).is(function(index, item) {});`

    - 자손 요소 탐색
        - find() 메서드는 래퍼세트의 모든 요소들에 대하여 주어진 선택자를 만족하는 모든 자손 요소를 선택

        - find() 메서드는 선택자를 통해 탐색한 DOM 요소들을 새로운 래퍼세트로 반환
            > 래퍼세트의 filter() 메서드는 래퍼세트 요소를 걸러내기 위해 사용   
              But, find() 메서드는 래퍼세트 요소들의 하위 자손들을 탐색하기 위해 사용
              `$(selector).find(selector);`

<br>

[목차로 이동](#목차)

---

<br>

## DOM 객체

<br>

- DOM 객체 제어
    - jQuery는 DOM 요소의 속성이나 class, style 을 제어하는 DOM 특성제어 메서드 제공
        > DOM 내부제어 메서드, DOM 추가/삭제 메서드, DOM 객체 삽입 메서드 존재
    
        DOM 특성 제어|DOM 내부 제어|DOM 추가 및 삭제|DOM 객체 삽입
        :---|:---|:---|:---
        attr()|html()|$()|append()
        removeAttr()|text()|remove()|appendTo()
        addClass()||empty()|prepend()
        removeClass()||clone()|prependTo()
        toggleClass()|||after()
        css()|||insertAfter()
        ||||before()
        ||||insertBefore()

- DOM 특성 제어
    - 속성 제어 메서드
        - attr(name) 메서드는 래퍼세트의 첫 번째 요소에 대한 속성(attribute)값을 반환
            > 해당하는 속성이 없으면 undefined 반환   
              `$(selector).attr(name);`
        - attr(name, value), attr(object), attr(name, function) 메서드는 확장 집합의 모든 요소에 속성값을 설정
            > `$(selector).attr(name, value);`   
              `$(selector).attr(name, object);`   
              `$(selector).attr(object);`   
        - removeAttr(name) 메서드는 DOM 요소에서 해당 속성을 제거
            > `$(selector).removeAttr(name);`

    - Class 속성제어
        - class 속성 값 추가 및 삭제 메서드
            > `$(selector).addClass(className);`   
              `$(selector).removeClass(className);`   
              `$(selector).toggleClass(className);`   

    - Style 속성제어
        - css(name) 메서드는 래퍼세트의 첫 번째 요소에 대한 style 속성값을 반환

        - css(name, valut) 메서드는 래퍼세트 내 모든 DOM 요소에 style을 설정
        - css(name,function) 메서드는 style 속성 값을 바로 지정하는 대신, 함수를 통해 style 속성 값을 설정함
        - 여러 style 속성값을 한번에 설정하려면 css(object)와 같이 객체를 인자로 사용함
            > `$(selector).css(styleName);`   
              `$(selector).css(name,value);`   
              `$(selector).css(name, function(i, style) {});`   
              `$(selector).css(object);`

- DOM 내부 제어
    - HTML과 텍스트 조회
        - html()과 text()는 DOM 객체 내부를 조회하는 메서드

        - js의 innerHTML, textContent 속성과 관련이 있음
        - html() 함수는 HTML 태그를 인식하지만, text() 함수는 HTML 태그를 인식하지 않음
        - 래퍼세트에서 text()를 호출하면 태그를 인식하지 못하므로 태그를 제외한 텍스트만 반환
            > `$(selector).html();`   
              `$(selector).text();`

    - HTML과 텍스트 설정
        - html(value)와 text(value)는 DOM 객체 내부를 설정하는 메서드

        - html(value) 메서드는 HTML 태그를 인식하므로, DOM 객체 형태로 지정한 내용을 설정
        - text(value) 메서드는 태그를 인식하지 못하므로, 텍스트 형태로 지정한 내용을 설정
        - text(value)로 태그가 포함된 내용을 설정하면, 단순 텍스트로 인식되어 태그 내용 그대로 출력

- DOM 추가 및 삭제
    - DOM 객체 추가
        - html() 메서드는 문자열로 된 HTML을 DOM 객체로 추가
            > 문자열을 이어 붙여서 HTML을 구성하는 작업은 복잡하고 지저분한 코드를 유발

        - $() 함수는 선택자를 수행하는 기능 뿐 아니라, DOM 객체를 바로 생성하는 기능 제공
        - 메서드 체인을 사용하면, $() 함수로 DOM 객체를 생성하여 객체 특성을 설정하는 작업을 간편하게 할 수 있음
            > `$();`   
              예시 : `$("<h1>안녕</h2>").css("color","green").appendTo("body");`

    - DOM 객체 삭제
        - jQuery는 DOM 객체를 삭제하거나, DOM 객체 내부를 비우는 메서드를 제공

        - remove() 메서드는 래퍼세트의 모든 요소를 HTML 문서에서 삭제하고, 삭제한 내용을 반환
        - empty() 메서드는 래퍼세트의 모든 요소에 대해 하위 자식요소들을 삭제
        - empty() 메서드는 하위 자식 요소들을 삭제한 결과를 반환
            > `$(selector).remove();`   
              `$(selector).empty();`

- DOM 객체 삽입
    - 개요
        - DOM 객체 삽입은 이미 존재하는 DOM 객체에 다른 DOM 객체를 삽입하는것
        - Target 객체를 기준으로 어느 위치에 삽입하는지에 따라 여러 메서드가 있음
        - append(), prepend() 메서드는 Target 객체의 자식요소로 DOM 객체 삽입
        - before(), after() 메서드는 Target 객체의 인접한 형제요소로 DOM 객체 삽입

            메서드|설명
            :---|:---
            `$(Target).append(B)`|Target 객체의 하위 자식요소로 맨 뒤에 B 객체를 추가
            `$(Target).prepend(B)`|Target 객체의 하위 자식요소로 맨 앞에 B 객체를 추가
            `$(Target).after(B)`|Target 객체 바로 이후 요소로 B 객체를 추가
            `$(Target).before(B)`|Target 객체 바로 이전 요소로 B 객체를 추가
        
    - jQuery 메서드
        - DOM 객체 삽입 메서드는 파라미터에 어떤 객체를 사용하는 지에 따라 두 가지 유형으로 구분

        - appendTo(), prependTo(), insertAfter(), insertBefore() 메서드는 삽입 대상 객체를 인자로 받음
        - append(), prepend(), after(), before() 메서드는 삽입하려는 내용 (또는 객체)을 인자로 받는다
        - 삽입하려는 내용이 DOM 객체인 경우, 해당 객체는 이전 위치에서 새로운 위치로 이동

- jQuery Effect
    - Effect 메서드
        - 화면에서 보여주는 시각 효과를 구현하는 방법
        - 사용자가 직접 애니메이션 효과를 만들 수 있음
        - jQuery plug-in 으로 시각 효과를 내는 메서드를 구현할 수 있음
            
            메서드|설명
            :---|:---
            show()|DOM 요소를 화면에 보이도록 함
            hide()|DOM 요소를 화면에서 숨김
            toggle()|show()와 hide()를 번갈아 실행
            slideDown()|DOM 요소를 슬라이드 효과와 함께 나타나게 함
            slideUp()|DOM 요소를 슬라이드 효과와 함께 사라지게 함
            slideToggle()|slideDown()과 slideUp()을 번갈아 실행
            fadeIn()|DOM 요소가 서서히 나타남
            fadeOut()|DOM 요소가 서서히 사라짐
            fadeToggle()|fadeIn()과 fadeOut()이 번갈아 실행
            animate()|css 속성값을 이용하여 위치, 크기, 속성 값을 서서히 변경하는 애니메이션 효과를 줌

    - Effect 매개변수
        - jQueryEffect 모든 메서드는 공통적으로 세 개의 매개변수를 제공
            > speed는 효과를 진행하는 속도를 지정   
              callback은 효과를 완료한 후 실행할 함수를 지정   
              easing은 애니메이션 easing 형태를 지정함
              >> plug-in을 사용하지 않으면 linear와 swing만 입력 가능

            > `$(selector).method();`   
            > `$(selector).method(speed);`   
            > `$(selector).method(speed, callback);`   
            > `$(selector).method(speed, easing, callback);`   

    - 사용자 정의 효과
        - jQuery에서 기본으로 제공하는 간단한 효과만으로도 시각적인 효과를 얻을 수 있음
        - 더 수준 높은 효과를 만들려면 효과를 개별적으로 정의해야 함
        - animate() 함수는 사용자 정의 효과를 만들 수 있는 방법을 제공
        - 첫 번쨰 인자인 객체에 속성 값을 설정하여 세밀하게 효과를 조정할 수 있음
            > `$(selector).animate(object);`   
            > `$(selector).animate(object, speed);`   
            > `$(selector).animate(object, speed, callback);`   
            > `$(selector).animate(object, speed, easing, callback);`   

            object 속성의 종류||
            :---|:---
            opacity|top
            height|bottom
            width|margin
            left|padding
            right

<br>

[목차로 이동](#목차)

---

<br>

## jQuery Event

<br>

- jQuery DOM Event 처리

    - jQuery Event 처리

        - jQuery Event로 기존 js DOM Event를 간편하게 처리, 연결 가능

        - 이벤트 핸들러를 할당, 해제할 수 있는 통합 메서드 제공
        - DOM Element의 이벤트 타입마다 여러 핸들러 할당 가능
        - click, mouseover 같은 표준 이벤트 타입명 사용
        - 핸들러의 매개변수로 이벤트 인스턴스를 사용 가능
        - 자주 사용하는 이벤트 인스턴스의 프로퍼티 등에 일관된 이름 제공
        - 하나의 API로 표준 호환 브라우저와 IE 동시 지원
    - jQuery Event 기능
        - click 함수로 클릭 이벤트 처리

        - bind, on 함수를 이용한 모든 이벤트 처리
        - unbind, off 함수를 이용하여 이벤트 제거

- jQuery Event Binding
    - bind() 함수
        - 선택된 DOM 객체의 이벤트에 지정한 핸들러를 연결하는 함수

        - 동적으로 생성한 DOM 객체에는 적용 X
            > `bind(eventType, data, listener)`
            >> eventType : 핸들러를 할당할 이벤트 타입의 이름   
            >> data : 핸들러 함수에서 사용할 데이터, 이벤트 인스턴스에 data라는 프로퍼티로 제공됨, 생략 시 두 번째 인자는 listener   
            >> listener : 이벤트 발생시 수행되는 핸들러 함수

    - unbind() 함수
        - 객체 이벤트에서 지정한 핸들러를 지운다
            > `unbind(eventType, handler)`   
            > `unbind(eventType)`   
            >> 확장 집합의 모든 엘리먼트에서 매개변수에 따라 이벤트 핸들러를 삭제   
            >> 매개변수가 없을 경우 모든 핸들러를 제거
            
            > 파라미터 종류
            >> eventType : 핸들러를 할당할 이벤트 타입의 이름   
            >> handler : 이벤트 발생 시 수행되는 핸들러 함수

    - on() 함수
        - bind() 함수처럼 DOM 객체에 이벤트 핸들러를 연결

        - delegate 방식으로 사용 시 동적으로 생성한 DOM 객체에도 적용 가능
        - 1.7.x 버전부터 적용되었고, 이벤트 연결에 가장 기본이 되는 함수로 권장됨
            > `$(selector).on(eventType, delegate selector, function(event) {});`

    - off() 함수
        - on() 과 반대로 DOM 객체의 이벤트 제거

        - 선택된 DOM 객체의 특정 이벤트 또는 모든 이벤트를 제거
            > `$(selector).off();`   
            > `$(selector).off(eventType);`   
            > `$(selector).off(eventType,function(event){});`   

    - Simple Event Bind
        - jQuery는 DOM 객체에 이벤트를 간단하게 연결할 수 있는 다양한 함수를 제공함
        - 클릭 이벤트를 연결할 때 on()을 사용하는 대신, click() 함수를 사용할 수 있음

            종류|||||
            :---|:---|:---|:---|:---
            .blur|.focus|.focusin|.mousedown|.resize
            .change|.keydown|.focusout|.mousemove|.scroll
            .click|.keypress|.mouseenter|.mouseout|.select
            .dblclick|.keyup|.mouseleave|.mouseover|.submit
            .error|.load|.ready|.mouseup|.unload

            > `$(selector).on("click", function(event){});`   
            > `$(selector).click(function(event){});`   

    - one() 함수
        - 이벤트를 연결하고 한번 실행 후 삭제

- Mouse Event
    - jQuery 에서 마우스 클릭 및 움직임과 관련된 이벤트 처리가 가능
    - mouseenter 이벤트는 경계 외부에서 내부로 접근 시 발생
    - mouseover 이벤트는 마우스가 요소 안에 들어 올 때 발생하며 이벤트 버블링이 적용
        
        이벤트|설명
        :---|:---
        click|마우스를 클릭할 때 발생
        dblclick|마우스를 더블 클릭할 때 발생
        mousedown|마우스 버튼을 누를 때 발생
        mouseup|마우스 버튼을 뗄 때 발생
        mouseenter|마우스가 경계의 외부에서 내부로 이동할 때 발생
        mouseleave|마우스가 경계의 내부에서 외부로 이동할 때 발생
        mousemove|마우스가 움직일 때 발생
        mouseout|마우스가 요소를 벗어날 때 발생
        mouseover|마우스가 요소안에 들어올 때 발생

- Keyboard Event
    - 키보드에서 입력할 때, 키보드 이벤트 처리 가능
        이벤트|설명
        :---|:---
        keydown|키보드를 누를 때 발생 (KeyCode)
        keypress|글자가 입력될 때 발생(ASCII)
        keyup|키보드를 뗄 때 발생

- Window Event
    - 윈도우에서 발생한 변화를 이벤트로 처리 가능
        이벤트|설명
        :---|:---
        ready|DOM 객체가 준비되면 발생
        load|윈도우를 불러들일 때 발생
        unload|윈도우를 닫을 때 발생
        resize|윈도우 크기가 변화될 때 발생
        scroll|윈도우 스크롤을 할 때 발생
        error|에러가 있을 때 발생

- Input Event
    - 일반적으로 HTML 에서 입력 양식으로 데이터를 전송하는 작업을 많이 함

    - 입력 양식에서도 발생하는 이벤트가 존재하고 jQuery에서도 이를 지원함
        이벤트|설명
        :---|:---
        change|입력 양식에 내용이 변경할 때 발생
        focus|입력 양식에 포커스가 가면 발생
        focusin|입력 양식에 포커스가 가기 전에 발생
        focusout|입력 양식에 포커스가 사라지기 전에 발생
        blur|입력 양식에 포커스가가 사라지면 발생
        select|입력 양식을 선택할 때 발생 (text, textarea 제외)
        submit|submit 버튼 클릭 시 발생
        reset|reset 버튼 클릭 시 발생

[목차로 이동](#목차)