# **JavaScript** 🎬

## JavaScript 소개

<br>

- 개요

    - 프로토타입 기반의 스크립트 프로그래밍 언어로 객체지향 개념을 지원

    - 웹 브라우저가 JavaScript를 HTML과 함께 다운로드하여 실행
    - 웹 브라우저가 HTML 문서를 읽어들이는 시점에 JavaScript 엔진이 실행됨
    - 대부분의 JavaScript Engine은 ECMA Script 표준을 지원함

<br>

- ECMA Script (European Computer Manufacturers Association)

    - 1996년 Netscape는 Navigator 2.0을 출시하면서 JavaScript 지원

    - 1996년 Microsoft는 웹에 호환되는 JScript를 개발, IE 3.0에 포함하여 출시
    - Netscape가 JavaScript 표준화를 위해 기술 규격을 ECMA에 제출하고 1997년 채택됨
    - 1998년 ECMA Script(Edition 2)f를 ISO 표준으로 개정
    - 1999년 정규 표현식, 문자열 처리, 새로운 제어문, 예외처리, 오류정의, 포매팅을 추가 (Edition 3)
    - Edition 4는 정치적 문제로 폐기
    - Edition 5.1 (2011) 을 거쳐 Edition 6 (2015.06, Harmony) 까지 표준화 됨

<br>

- JavaScript 의 특징

    - HTML, CSS와 함께 웹을 구성하는 요소로써 웹 브라우저에서 동작하는 유일한 프로그래밍 언어

    - 개발자가 별도의 컴파일 작업을 수행하지 않는 인터프리터 언어
    - 각 브라우저별  JavaScript 엔진(Chrome의 V8 등)은 인터프리터와 컴파일러의 장점을 결합하여 비교적 처리속도가 느린 인터프리터의 단점을 해결
    - 명령형 (imperative), 함수형 (functional), 프로토 타입 기반 (prototype-based) 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어

<br>

---

<br>

## JavaScript 문법

<br>

- 선언
    - HTML에서 js를 사용하려면 \<script> tag를 사용

    - \<script> tag는 src와 type 속성을 사용하여 선언
        > HTML5 부터 type 속성 생략가능
        ```html
            <script type="text/javascript"></script>
        ```
    - src 속성은 외부의 js 파일을 HTML 문서에 포함할 때 사용하며, 생략 가능
    - type 속성은 미디어 타입을 지정할 때 사용. js 코드는 text/javascript로 지정
        ```html
            <script src="first.js" type="text/javascript"></script>
        ```
    - \<script> 태그는 HTML 파일 내부의 \<head>나 \<body>안 어느곳에서나 선언 가능
        > \<head> 안에 위치한 js는 브라우저의 각종 입/출력 발생 이전에 초기화 되므로 브라우저가 먼저 점검   
          \<body> 안에 위치하면 브라우저가 HTML부터 해석하여 화면에 그리기 때문에 사용자가 빠르다고 느낄 수 있음

<br>

- 주석
    ```javascript
        <!-- 한줄주석 -->
        // 한줄주석
        <!-- 블록주석 -->
        /* 
        블록
        주석
        */
    ```

<br>

- 변수
    - js는 변수 선언시 타입을 명시하지않고 var, let, const 키워드로 선언하며 동적으로 타입이 결정됨
        > 같은 변수에 여러 타입의 값을 할당할 수 있음

    - 일반적인 변수와 함수 이름 명명
        - 변수 이름 : 변수[형용사, 명사]
        - 함수 이름 : 함수[동사]
    - js는 ECMAScript 표준에 따라 낙타 표기법(Camel case)을 사용
        > 소문자로 작성하되 2개 이상의 단어일 경우 단어의 첫 글자는 대문자로 표기   
          금지 : 키워드, 공백 문자 포함, 숫자로 시작   
          허용 : 특수문자의 _와 $

<br>

- 자료형
    - 프로그램은 정적인 데이터 값을 동적으로 변환해 가면서 원하는 정보를 얻는데 이때 다루는 데이터 값의 종류들을 자료형이라 부름

    - js에서는 자료 형을 원시 타입(primitive type)과 객체 타입(object type)으로 분류
    - 원시 타입을 제외한 모든 값은 객체 타입임

        자료형|type|설명
        :---|:---|:---
        숫자형|number|정수 또는 실수형
        문자열형|string|문자, single or double quotation으로 표기
        boolean형|boolean|true, false
        undefined|undefined|변수가 선언 되었지만 초기화가 안되었을 경우
        numm|object|값이 존재하지 않을 경우

    - 숫자
        - js는 숫자를 정수와 실수로 나누어 구분하지 않음
            > 모든 숫자를 8바이트의 실수 형태로 처리   
              편의성을 위해 정수 리터럴과 실수 리터럴을 제공   
              연산 처리를 실수 형태로 하기 때문에 특정 소수점을 정확하게 표현하지 못함
        
        - 언더플로우, 오버플로우, 0으로 나누는 연산에 대해 예외를 발생시키지 않음
            > 숫자와 관련된 특별한 상수가 존재   
              Infinity : 무한대를 나타내는 상수, 수를 0으로 나누거나 Infinity로 사칙연산한 결과   
              NaN (Not a Number) : 계산식의 결과가 숫자가 아님을 나타내는 상수
    
    - 문자열
        - js에서 문자열은 16비트의 유니코드 문자 사용

        - 문자형은 존재하지 않고 한글자도 문자열로 표현
        - single or double quotes 둘다 사용 가능하나 혼용은 불가능
        - 이스케이프 시퀀스 ( \ ) 사용가능
        - ES6 템플릿 리터럴로 백틱 ( ` )을 이용한 문자열 표현이 추가됨

    - boolean - 비교 연산의 결과값으로 ture 또는 false
        > 비어있는 문자열, numm, undefined, 숫자 0은 false로 간주
        - null은 값이 없거나 비어 있음을 뜻함

        - undefined는 값이 초기화 되지 않았음(정의되지 않음)을 의미
            > 값을 할당하지 않은 변수는 undefined을 할당 (시스템레벨)   
            코드에서 명시적으로 값이 없음을 나타낼 때 null을 사용 (프로그램 레벨)

    - 자동 형 변환 (동적 타이핑)
        - 어떤 자료형이든 전달할 수 있고, 그 값을 필요에 따라 변환 가능

        - 서로 다른 자료형끼리 연산 가능
        - But, 느슨한 규칙 때문에 혼란을 야기

    - 변수 호이스팅 (Variable Hoisting)
        - 호이스팅이란, var 선언문이나 function 선언문 등 모든 선언문이 해당 Scope의 처음으로 옮겨진 것처럼 동작하는 특성
            > js는 선언문이 선언되기 이전에 참조가 가능

        - 변수의 생성단계
            > 선언 단계 : 변수 객체에 변수를 등록
              초기화 단계 : 변수 객체에 등록된 변수를 메모리에 할당, undefined로 초기화됨
            >> 선언과 초기화 단계는 한번에 이루어진다   

            > 할당 단계 : undefined로 초기화된 변수에 실제 값을 할당
    
    - let, const
        - ES6 부터 let과 const 키워드 추가   
            키워드|구분|선언위치|재선언
            :---|:---|:---|:---
            var|변수|전역스코프|가능
            let|변수|해당스코프|불가능
            const|상수|해당스코프|불가능

    - 연산자
        - 다른 언어에서 지원하는 연산자와 유사하므로 다른것만 작성함
            연산자|설명
            :---|:---
            delete|프로퍼티를 제거
            in|프로퍼티가 존재하는지 확인
            void|undefined 값을 알려줌
            ===|값이 일치하는지 확인 (타입포함)
            !==|값이 일치하지 않는지 확인 (타입포함)

    - 객체 (Object)
        - 객체는 이름과 값으로 구성된 프로퍼티의 집합

        - 문자열, 숫자, boolean, null, undefined 를 제외한 모든 값은 객체
        - js의 객체는 key와 value로 구성된 프로퍼티 (property)들의 집합
        - 전역 객체를 제외한 js객체는 프로퍼티를 동적으로 추가하거나 삭제 가능
        - js의 함수는 일급 객체이므로 값으로 사용할 수 있으므로 프로퍼티의 값에 함수 사용 가능
        - js 객체는 프로토타입 (prototype) 이라는 특별한 프로퍼티를 포함


    - 객체 생성 방법
        - 객체 리터럴
            > { } 를 사용하여 객체를 생성, { } 내에 1개 이상의 프로퍼티를 추가하여 객체 생성   
            ```js
                var obj = {}; // 객체 생성
                var student = { // 객체 생성
                    name: '홍길동',
                    info: function () {
                        console.log('내 이름은 ' + this.name);
                    },
                };
            ```

        - Object 생성자 함수
            > new 연산자와 Object 생성자 함수를 호출하여 빈 객체를 생성   
              빈 객체 생성 후 프로퍼티 또는 메소드를 추가하여 객체를 완성
            ```js
                var student = new Object(); // 빈 객체 생성
                student.name = '홍길동';
                student.info = function () {
                    console.log('내 이름은 ' + this.name);
                };
            ```

        - 생성자 함수
            > 동일한 프로퍼티를 갖는 객체 생성 시 위 두 방법은 동일한 코드를 반복적으로 작성하게 되므로 효율적이지 않음   
              생성자 함수를 템플릿(클래스)처럼 사용하여 프로퍼티가 동일한 객체 여러 개를 간단히 생성 가능
            ```js
                function Student(name) { // 생성자 함수
                    this.name = name;
                    this.info = function () {
                        console.log('내 이름은 ' + this.name);
                    };
                }
                // 객체 생성.
                var student1 = new Student('홍길동');
                var student2 = new Student('홍두깨');
            ```
        
    - 속성 값 조회
        - 객체는 dot (.)을 사용하거나 대괄호 (["반드시문자열"])를 사용해서 속성 값에 접근함

        - 객체에 없는 속성에 접근하면 undefined를 반환
        - 객체 속성 값을 조회 할 때 \|| 연산자를 사용하는 방법도 가능
            ```js
                console.log(member.age);
                console.log(member["age"]);
            ```

    - 속성 값 변경, 추가, 제거
        - 속성 값 변경시 dot (.) 또는 대괄호 ([])사용

        - 객체에 값을 할당하는 속성이 없을 경우 추가됨
        - delete 연산자를 이용하여 속성 제거
            ```js
                // 속성의 값 변경
                member.age = 30;
                member['city'] = '경기도';

                // 속성 추가
                member.tel = '010-1234-5678';

                // 속성 제거
                delete member.tel;
            ```
    
<br>

- 함수
    - 선언 및 호출
        - js에서 함수는 일급(first-class) 객체임
            > 일급객체란 3가지 조건을 충족하는것  
              1. 변수나 데이타에 할당할 수 있어야 함   
              2. 객체의 인자로 넘길 수 있어야 함 (콜백함수)   
              3. 객체의 리턴값으로 리턴할 수 있어야 함   

            > 함수는 프로그램 실행 중에 동적으로 생성 가능   
              함수 정의 방법은 함수 선언문, 함수 표현식, Function 생성자(constructor) 함수 세 가지 방식 제공

            ```js
                // 함수 선언문
                funtion name(a, b, c){ }
                
                // 함수 표현식
                var name = function (a, b, c) { }

                // 함수 생성자
                var name = new Function(a, b, c, "함수내용");
            ```


    - 함수 호이스팅(Hoisting)
        - 함수 선언문은 함수 선언의 위치와 상관없이 코드 내 어느 곳에서든지 호출 가능

        - js는 모든 선언(var, function)을 호이스팅함
        - 함수 선언문으로 정의된 함수는 js 엔진이 스크립트가 로딩되는 시점에 이를 변수객체 저장함
            > 함수 선언, 초기화 , 할당이 한번에 이루어진다
        - 함수 표현식의 경우 함수 호이스팅이 아니라 변수 호이스팅이 발생

    - 매개변수
        - js에서 함수 정의 시 매개변수에 대한 타입은 명시하지 않음

        - 함수 호출 시 정의된 매개변수와 전달인자의 개수가 일치하지 않더라도 호출 가능

    - 콜백 함수
        - 함수를 명시적으로 호출하는 방식이 아닌 특정 이벤트가 발생했을 때 시스템에 의해 호출되는 함수

        - 일반적으로 매개변수를 통해 전달되고 전달받은 함수의 내부에서 특정시점에 실행됨
        - 주로 비동기식 처리모델에서 사용되며, 처리가 종료되면 호출될 함수(콜백함수)를 미리 매개변수에 전달하고 처리가 종료되면 콜백함수를 호출

<br>

---

<br>

## Web Browser 와 Window 객체

<br>

- 개요
    - Window 객체는 웹 브라우저에서 작동하는 js의 최상위 전역객체임

    - Window 객체에는 브라우저와 관련된 여러 객체와 속성, 함수가 있음
    - js에서 기본으로 제공하는 프로퍼티와 함수도 포함 (Number 객체, setInterval() 함수 등)
    - BOM (Browser Object Model)으로 불리기도 함

- Window 객체 사용
    - alert() : 브라우저의 알림창
        ```js
            alert("알림");
        ```
    - confirm() : 브라우저의 확인/취소 선택창
        ```js
            if(confirm("확인?")) {
                console.log("확인");
            } else {
                console.log("취소");
	        }
        ```
    - prompt() : 브라우저의 입력 창
        ```js
            prompt("설명글", "기본입력값");
        ```
    - navigator
        - navigator 객체는 브라우저의 정보가 내장된 객체

        - navigator의 정보로 서로 다른 브라우저를 구분할 수 있으며, 브라우저 별로 다르게 처리 가능
        - HTML5에서는 위치 정보를 알려주는 역할 가능
        ```js
            console.log("Browser CodeName 	: " + navigator.appCodeName);
            console.log("Browser Name 		: " + navigator.appName);
            console.log("Browser Version 	: " + navigator.appVersion);
            console.log("Browser Enabled 	: " + navigator.cookieEnabled);
            console.log("Platform 	    	: " + navigator.platform);
            console.log("User-Agent header 	: " + navigator.userAgent);
        ```

    - location
        - 현재 페이지 주소 (URL)와 관련된 정보들을 알 수 있음

        - location.href : 프로퍼티에 값을 할당하지 않으면 현재 URL을 조회하고 값을 할당하면 할당 된 URL로 페이지 이동
        - location.reload() : 현재 페이지를 새로고침

    - history
        - 브라우저의 페이지 이력을 담는 객체

        - history.back() : 브라우저의 뒤로 가기 버튼과 같은 동작
        - history.forward() : 브라우저의 앞으로 가기 버튼과 같은 동작

    - 새 창 열기
        - window.open('페이지 URL', '창이름', '특성', '히스토리 대체여부');
            > 창이름 (string) : open할 대상(_blank, _self 등) 지정 혹은 창의 이름   
              특성 (string) : 새로 열릴 창의 너비, 높이 등의 특성 지정   
              히스토리 대체여부(Boolean) : 현재 페이지 히스토리에 덮어 쓸지 여부   
            
            특성|값|브라우저|설명
            :---|:---|:---|:---
            width|픽셀|ALL|창의 너비
            height|픽셀|ALL|창의 높이
            top|픽셀|ALL|창의 세로(y) 좌표 위치
            left|yes\|\|no|ALL|창의 가로(x) 좌표 위치
            menubar|yes\|\|no|IE, Firefox, Opera|메뉴 표시줄
            status|yes\|\|no|IE, Firefox, Opera|상태 표시줄
            scrollbars|yes\|\|no|IE, Firefox, Opera|스크롤바
            toolbar|yes\|\|no|IE, Firefox|도구모음
            resizable|yes\|\|no|IE 전용|창 크기 조절 가능여부
            location|yes\|\|no|Opera 전용|주소 입력란

        - 창 열고 닫기
            - 이벤트를 이용하여 특정 시점에 창을 열 수 있음
                > 페이지 로딩 완료 후 새 창 열기, 클릭할 때 새 창 열기 등
            - window.close() 호출로 현재 창을 닫을 수 있음
                > 브라우저에 내장 된 창이 아닌 js로 자체 구현한 팝업에서 필요

        - 부모 창 컨트롤
            - window 객체의 opener 속성을 이용하면 부모 창을 컨트롤 가능
                > 부모 창에 값 전달   
                  부모 창을 새로고침 하거나 페이지 이동
                ```js
                    // 부모 창 새로고침 후 창 닫기
                    function reloadOpener() {
                        opener.location.reload();
                        self.close();
                    }
                ```
            - opener 객체는 부모 창의 window 객체
                ```js
                    // 부모 창에 값 전달 후 창 닫기
                    function setOpenerData(data) {
                        window.opener.setData(data);
                        window.close();
                    }
                ```
    - window 객체 프로퍼티
        - window 객체는 웹 브라우저에서 구동되는 js의 전역 객체임
        - screen 객체는 화면의 가로, 세로 크기 정보를 알 수 있음
        - pageYOffset 등과 scroll() 함수를 이용하면 현재 화면의 크기를 계산하여 페이지 단위로 스크롤 제어가 가능   
            프로퍼티명|설명|프로퍼티명|설명
            :---|:---|:---|:---
            self|현재 창 자신, window와 같음|statusbar|창의 상태 바
            document|document 객체|toolbar|창의 툴바
            history|history 객체|personalbar|창의 퍼스널 바
            location|location 객체|scrollbars|창의 스크롤 바
            opener|open()으로 열린 창에서 볼 때 자기를 연 창|innerHeight|창 표시 영역의 높이 (픽셀) (IE 지원 X)
            parent|프레임에서 현재프레임의 상위프레임|innerWidth|창 표시 영역의 너비 (픽셀) (IE 지원 X)
            top|현재프레임의 최상위프레임|outerHeight|창 바깥쪽 둘레의 높이 (IE 지원 X)
            frames|창 안의 모든 프레임에 대한 배열 정보|outerWidth|창 바깥쪽 둘레의 너비 (IE 지원 X)
            locationbar|location 바|pageXOffset|현재 나타나는 페이지의 X위치 (IE 지원 X)
            menubar|창 메뉴바|pageYOffset|현재 나타나는 페이지의 Y위치 (IE 지원 X)
            
    - window 객체 함수
        - 브라우저에서 버튼으로 제공하는 기능인 find, stop, print와 같은 함수 존재
        - move 함수로 열려 있는 창의 위치 이동 가능

            함수명|설명
            :---|:---
            alert()|경고용 대화 상자 표시
            confirm()|확인, 취소를 선택 상자 표시
            prompt()|입력창이 있는 대화 상자 표시
            open()|새로운 창을 오픈
            scroll()|창을 스크롤 함
            find()|창안에 지정된 문자열이 있는지 확인, 있으면 true, 없으면 false (IE 지원 X)
            stop()|불러오기를 중지 (IE 지원 X)
            print()|화면에 있는 내용을 프린터로 출력
            moveBy()|창을 상대적 좌표로 이동, 수평방향과 수직방향의 이동량을 픽셀로 지정
            moveTo()|창을 절대적 좌표로 이동, 창의 왼쪽 상단 모서리를 기준으로 픽셀을 지정

        - resize 함수로 열려 있는 창의 크기 조절
        - window 객체에는 순수 js에서 필요한 객체나 함수도 존재
            > setTimeout() 과 setInterval() 로 함수를 특정 시간 후 혹은 특정 시간마다 호출 가능
              eval() 은 문자열로 된 js코드를 해석한 후 실행

            함수명|설명
            :---|:---
            resizeBy()|창의 크기를 상대적인 좌표로 재설정
            resizeTo()|창의 크기를 절대적인 좌표로 재설정, 창 크기를 픽셀로 지정
            scrollBy()|창을 상대적인 좌표로 스크롤, 창의 표시영역의 수평방향과 수직방향에 대해 픽셀로 지정
            scrollTo()|창을 절대적인 좌표로 스크롤, 창의 왼쪽 상단 모서리 기준으로하여 픽셀로 지정
            setTimeout()|지정한 밀리초 시간이 흐른 후에 함수 호출
            clearTimeout()|setTimeout 함수를 정지
            setInterval()|지정한 밀리초 주기마다 함수를 반복적으로 호출
            clearInterval()|setInterval 함수를 정지
            eval()|문자열을 js 코드로 변환하여 실행

<br>

---

<br>

## HTML과 DOM

<br>

- DOM 개요
    - Document Object Model은 HTML과 XML 문서의 구조를 정의하는 API를 제공
    - 문서 요소 집합을 트리 형태의 계층 구조로 HTML을 표현
        > document가 root 노드   
          HTML 태그나 요소(element)들을 표현하는 노드와 문자열을 표현하는 노드로 이루어짐
    
    - 문서계층구조
        - Document는 HTML 또는 XML 문서를 표현
        - HTMLDocument는 HTML 문서와 요소만을 표현함
        - HTMLElement의 하위 타입은 HTML 단일 요소나 요소 집합의 속성에 해당하는 js 프로퍼티를 정의
        - Comment 노드는 HTML이나 XML의 주석을 표현

- 문서 객체
    - 생성
        - 문서 객체는 text node를 갖는 객체와 갖지 않는 객체로 나뉨
            함수명|설명
            :---|:---
            createElement(tagName)|element node를 생성
            createTextNode(text)|text node를 생성
            appendChild(node)|객체에 node를 child로 추가

            ```js
                window.onload = function () {
                    // 변수를 선언하고 element node와 text node 생성.
                    var title = document.createElement('h2');
                    var msg = document.createTextNode('create');

                    // text node를 element node에 추가.
                    title.appendChild(msg);
                    document.body.appendChild(title);
                };
            ```

        - 객체의 속성 설정
            함수명|설명
            :---|:---
            setAttribute(name, value)|객체의 속성을 지정
            getAttribute(name)|객체의 속성값을 가져옴

            ```js
                var profile = document.createElement('img');
                profile.src = 'profile.png';
                // 간단하지만, 웹 표준이나 웹 브라우저가 지원하는 태그의 속성만 사용 가능

                profile.setAttribute('src', 'profile.png');
                profile.setAttribute('data-content', '내사진');
                // 웹 브라우저가 지원하지 않는 태그의 속성도 가능

                document.body.appendChild(profile);
            ```
        
        - innerHTML &amp; innerText
            함수명|설명
            :---|:---
            innerHTML|문자열을 HTML 태그로 삽입
            innerText|문자열을 text node로 삽입

            ```js
                html.innerHTML = '<h2>Hello!!!</h2>'; // html을 작성하는것과 같음
                text.innerText = '<h2>Hello!!!</h2>'; // 태그도 text로 들어감
            ```
    
    - 문서 객체 가져오기
        함수명|설명
        :---|:---
        getElementById(id)|태그의 id 속성이 id와 일치하는 element 객체 얻기
        getElementsByClassName(classname)|태그의 class 속성이 classname과 일치하는 element 배열 얻기
        getElementsByTagName(tagname)|태그이름이 tagname과 일치하는 element 배열 얻기
        getElementsByName(name)|태그의 name 속성이 name과 일치하는 element 배열 얻기
        querySelector(selector)|selector에 일치하는 첫번째 element 객체 얻기
        querySelectorAll(selector)|selector에 일치하는 모든 element 배열 얻기

        ```js
            var msg = document.getElementById('header');
            var msg = document.querySelector('#header');
            msg.innerHTML = '<h2>태그추가</h2>';

            var color = ['orange', 'cyan', 'purple'];
            var colors = document.getElementsByClassName('colors');
            var colors = document.getElementsByTagName('h2');
            var colors = document.querySelectorAll('.colors');

            for (var i = 0; i < colors.length; i++) {
            colors[i].style.background = color[i];
            }
        ```
    
    - 객체 제거
        속성|설명
        :---|:---
        removeChild(childnode)|객체의 자식 노드를 제거

        ```js
            var text = document.querySelector('#text');
            document.body.removeChild(text);
        ```

<br>

---

<br>

## 이벤트(Event)

<br>

- 개요
    - 웹 페이지에서 여러 종류의 상호작용이 있을 때 마다 이벤트 발생
    - js를 사용하여 DOM에서 발생하는 이벤트를 감지하여 이벤트에 대응하는 여러 작업 수행
    - 이벤트는 일반적으로 함수와 연결이 되고, 이 함수는 이벤트가 발성되기 전에는 실행되지 않다가 이벤트가 발생 할 경우 실행
        > 이벤트 핸들러 (Handler) 또는 이벤트 리스너 (Listener)라 하며 이 함수에 이벤트 발생 시 실행해야 하는 코드 작성   

        이벤트 발생 종류|
        :---|
        웹 페이지가 로딩되었을 때
        페이지를 스크롤 했을 때
        브라우저 창의 크기를 조절 했을 때
        마우스를 클릭했을 때
        키보드로 키를 입력했을 때
        Form이 Submit 되었을 때
        Input의 내용이 변경되었을 때
        마우스를 움직여서 Element를 이동할 때

        ```html
            <div onclick="javacript:openHello();">클릭</div>
            <script type="text/javascript">
                function openHello() { alert("안녕하세요."); }
            </script>
        ```

- 이벤트 종류
    - 마우스 이벤트
        - 마우스 이벤트 핸들러에 전달되는 이벤트 객체에는 마우스 위치와 버튼 상태 등의 정보를 담고있음
        
            이벤트|설명
            :---|:---
            onclick|마우스로 Element를 클릭했을 때 발생
            ondblclick|마우스로 Element를 더블 클릭했을 때 발생
            onmouseup|마우스로 Element에서 마우스 버튼을 올렸을 때 발생
            onmousedown|마우스로 Element에서 마우스 버튼을 눌렀을 때 발생
            onmouseover|마우스를 움직여서 Element 밖에서 안으로 들어올 때 발생
            onmouseout|마우스를 움직여서 Element 안에서 밖으로 나갈 때 발생
            onmousemove|마우스를 움직일 때 발생

    - 키보드 이벤트
        - 키보드의 커서가 웹 브라우저에 나타나는 지점에서 키보드를 조작할 때 이벤트 발생
        - 키보드 조작은 운영체제에 영향을 받으므로 특정 키가 이벤트 핸들러에게 전달되지 않을 수 있음
        - 키보드 커서가 나타내는 요소가 없다면 document에서 이벤트 발생

            이벤트|설명
            :---|:---
            onkeypress|키보드가 눌려졌을 때 발생 (ASCII)
            onkeydown|키보드를 누르는 순간 발생(KeyCode)
            onkeyup|키보드를 누르고 있던 키를 뗄 때 발생

    - Frame(UI) 이벤트
        - Frame 관련 이벤트는 특정 DOM 문서에 관련된 이벤트가 아니라 Frame 자체에 대한 이벤트
            이벤트|설명
            :---|:---
            onload|document, image, frame 등이 모두 로딩 되었을 때 발생
            onabort|이미지 등의 내용을 로딩하는 도중 취소 등으로 중단되었을 때 발생
            onerror|이미지 등의 내용을 로딩 중 오류가 발생했을 때 발생
            onresize|document, element 의 크기가 변경되었을 경우 발생
            onscroll|document, element가 스크롤 되었을 때 발생
            onselect|텍스트를 선택했을 때 발생

    - 폼(Form) 이벤트
        - Form 관련 이벤트는 웹 초기부터 지원되어 여러 웹 브라우저에서 가장 안정적으로 동작하는 이벤트
            이벤트|설명
            :---|:---
            onsubmit|form이 전송될 때 발생
            onreset|입력 form이 reset될 때 발생
            oninput|input 또는 textarea의 값이 변경되었을 때 발생
            onchange|select box, checkbox, radio button의 상태가 변경되었을 때 발생
            onfocus(focusin)|input과 같은 요소에 입력 포커스가 들어올 때 발생
            onblur(focusout)|input과 같은 요소 등에서 입력 포커스가 다른 곳으로 이동할 때 발생
            onselect|input, textarea에 입력 값 중 일부가 마우스 등으로 선택될 때 발생

- 이벤트 핸들러 등록
    - 이벤트를 감지하고 대응하는 작업을 등록하는 방법은 여러가지가 있고 어떤 이벤트를 처리할 작업을 등록하는 것을 '이벤트 핸들러(혹은 리스너)를 등록'이라 표현

    - 인라인 이벤트 핸들러
        > js의 초기에는 HTML 요소의 내부에서 직접 이벤트 핸들러를 등록했음   
        >> HTML 코드를 js코드가 침범한다는 문제가 있어 사용하지 않으나, 기존의 코드에서 사용이 되었기에 알아 두어야 함
        - 인라인 이벤트 핸들러 방식은 여러 개의 함수를 한번에 호출 가능
        - CBD(Component Based Development) 방식의 Angular/React/Vue.js 와 같은 framework/library에서는 인라인 방식으로 이벤트를 처리함
            > CBD에서는 html, css, js를 view의 구성 요소로만 보기 때문임
        ```html
            <div id="div1" onclick="msg1(); msg2();">클릭</div>

            <script type="text/javascript">
                var msg1 = function () {
                    alert('안녕');
                };
                var msg2 = function () {
                    var msg = document.querySelector('#div1');
                };
            </script>
        ```

    - 이벤트 핸들러 프로퍼티 방식
        - js에서 이벤트 핸들러를 등록함으로써 HTML와 js를 분리할 수 있음

        - 이벤트 대상이 되는 특정 DOM을 선택하고 이벤트 핸들러를 등록
            ```js
                document.getElementById("div1").onclick = function () {
                    alert('안녕');
                };
            ```
        - 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만을 바인딩 할 수 있다는 단점

    - addEventListener 메서드 방식
        - 2000년에 발표된 DOM 레벨2 이벤트 명세의 addEventListener(arg1, arg2[, arg3]) 를 이용하여 좀 더 세밀한 이벤트 제어가 가능
            > IE 9 이전 버전에서는 attachEvent() 를 사용함
            - 전달 인자의 첫 번째에는 이벤트 이름, 두 번째에는 이벤트 핸들러, 세 번째에는 캡쳐링 여부를 사용
            - 첫 번째 전달인자의 이벤트 이름에는 on을 제거한 이벤트 이름을 사용
            ```js
                // 이벤트명에 on을 생략(Internet Exploler 9 이전버전 사용불가)
                document.getElementById("div1").addEventListener("click", openAlert, false);
                
                // Internet Exploler 9 이전 버전에서 사용하기위한 방법
                //document.getElementById("div1").attachEvent("onclick", openAlert);
                
                function openAlert() {
                    alert("안녕");
                }
            ```
        - 장점
            - 하나의 이벤트에 대해 하나 이상의 이벤트 핸들러를 추가할 수 있음

            - 캡처링과 버블링을 지원
            - HTML 요소 뿐 아니라 모든 DOM(HTML, XML, SVG)에 대해 동작
            ```js
                const btn = document.querySelector('#btn');
                // 첫번째 바인딩된 이벤트 핸들러
                btn.addEventListener('click', function () {
                    alert('안녕');
                });
                // 두번째 바인딩된 이벤트 핸들러
                btn.addEventListener('click', function () {
                    btn.style.color = 'purple';
                });
            ```

        - 이벤트 핸들러 내부에서 함수를 호출하는 방식
            ```js
                const MIN_ID_LENGTH = 8;
                const input = document.querySelector('input[type=text]');
                const msg = document.querySelector('.message');

                function checkVal(len) {
                    if (input.value.length < len) {
                    msg.innerHTML = '값은 ' + len + '자 이상 입력';
                    } else {
                    msg.innerHTML = '';
                    }
                }

                input.addEventListener('blur', function () {
                    // 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달.
                    checkVal(MIN_ID_LENGTH);
                });
            ```
    
    - 버블링과 캡쳐링
        - 캡쳐링 : 이벤트가 발생한 요소를 포함하는 부모 HTML로부터 이벤트 근원지인 자식요소까지 검사하는 것
            > 이벤트 캡쳐링에서 캡쳐속성의 이벤트 핸들러가 등록되어 있으면 수행
        - 버블링 : 이벤트 발생 요소부터 요소를 포함하는 부모요소까지 올라가면서 이벤트를 검사하는 것
            > 이벤트 버블링에서 캡버블속성의 이벤트 핸들러가 등록되어 있으면 수행
            ```js
                // 세 번쨰 인자가 true이면 캡쳐링, false이면 버블링 시점에 수행
                div1.addEventListener("click", function(e) {
                    alert("div1");
                }, false); 
            ```

<br>

---

<br>

## Web Storage

<br>

- Web Storage - localStorage

    - WebStorage API : LocalStorage
        - 데이터를 사용자 로컬에 보존하는 방식
        - 데이터를 저장, 덮어쓰기, 삭제 등 조작 가능
        - js로만 조작
        - 모바일에서도 사용 가능

    - Cookie와의 차이점
        - 유효 기간이 없고 영구적으로 이용 가능
        - 5MB까지 사용 가능 (쿠키는 4kb까지, 브라우저에 따라 다를 수 있음)
        - 쿠키와는 다르게 네트워크 요청 시 서버로 전송되지 않음
        - 서버가 HTTP 헤더를 통해 스토리지 객체를 조작할 수 없음
        - 웹스토리지는 origin(프로토콜, 도메인, 포트)이 다르면 접근 불가

    - LocalStorage, SessionStorage 기본 구성
        - **키(key)와 값(value)** 을 하나의 세트로 저장
        - 도메인과 브라우저별로 저장
        - 값은 반드시 문자열로 저장

    - 공통 메서드와 프로퍼티
        - setItem(key, value) : key-value를 쌍으로 저장
        - getItem(key) : key에 해당하는 값을 리턴
        - removeItem(key) : key에 해당하는 값 삭제
        - clear() : 모든 값 삭제
        - key(index) : index에 해당하는 key
        - length : 저장된 항목의 개수

    ```js
        function init() {
            // localStorage 데이터 추가 방법 3가지
            localStorage.Test = 'Sample';
            localStorage["Test"] = "Sample";
            localStorage.setItem("Test", "Sample"); // 직관을 위해 사용추천

            // localStorage 데이터 취득 방법 3가지
            var val = localStorage.Test;
            var val = localStorage["Test"];
            var val = localStorage.getItem("Test"); // 직관을 위해 사용추천

            // 취득 데이터 출력
            document.querySelector('#result').innerHTML = val;

            // localStorage 데이터 삭제
            localStorage.removeItem("Test");

            // localStorage 데이터 전체 삭제
            localStorage.clear();
        }
    ```

<br>

- Web Storage - sessionStorage
    - 현재 떠 있는 탭에서만 유지 (같은 페이지라도 탭이 다르면 다른 곳에 저장)

    - 페이지 새로고침시에는 데이터 유지, 탭을 닫고 새로 열었을 경우 제거
        
        SessionStorage 사용|사용법
        :---|:---
        세션 저장|sessionStorage.setItem("key", value);
        특정 세션 값 불러오기|sessionStorage.getItem("key");
        특정세션 삭제|sessionStorage.removeItem("key");
        세션 전체 삭제|sessionStorage.clear();

- localStorage vs sessionStorage
    - localStorage - 세션이 끊겨도 사용 가능

    - sessionStorage - 같은 세션만 사용 가능
    - sessionStorage의 경우에는 동일한 세션에서만 사용 가능하지만 localStorage는 세션이 끊기거나 동일한 세션이 아니더라도 사용 가능