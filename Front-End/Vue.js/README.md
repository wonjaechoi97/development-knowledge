# **Vue.js** 🎥

### 참고자료

>- 한 권으로 배우는 Vue.js 3
>- [Vue3 공식가이드](https://vuejs.org/guide/quick-start.html)
>- [Vue2 공식가이드](https://kr.vuejs.org/v2/guide/)
>- [Vue2 스타일가이드](https://kr.vuejs.org/v2/style-guide/)

### 공식가이드 실습코드
>- [시작하기](Vue_1_시작하기.html)

## 목차
>- [Vue.js 개요](#vuejs-개요)
>>- [Vue.js](#vuejs)
>>- [SFC](#sfc)
>>- [컴포지션 함수 setup](#컴포지션-함수-setup)
>>- [MVVM Pattern](#mvvm-pattern)
>>- [설치방법](#설치방법)
>>- [기초 프레임](#기초-프레임)
>- [Vue Instance](#vue-instance)
>>- [Vue Instance 생성](#vue-instance-생성)
>>- [Vue Instance Life-Cycle](#vue-instance-life-cycle)
>- [template - 보간법(interpolation)](#template---보간법interpolation)
>- [template - Directice](#template---directice)

<br>

---

## Vue.js 개요
>### Vue.js
>- 사용자 인터페이스를 만들기 위해 사용하는 오픈 소스 프레임워크
>- Options API와 컴포지션 API의 존재로 프로젝트 규모에 따라 적절한 방식을 선택할 수 있음
>>- Options API: 하나의 객체를 하나의 모듈로 만들어 컴포넌트라 칭하는 라이브러리
>>>- 소규모 혹은 중간급 규모의 웹 애플리케이션을 제작하는데 가장 손쉬운 선택
>>>- 하나의 객체를 변수와 메서드 등과 같은 특정한 옵션 기능으로 나눔
>>>- 하나의 변수와 연관된 수많은 기능들이 하나의 SFC(Single File Component) 의 이곳 저곳에 산포되어 가독성을 떨어뜨리고 코드 유지비용을 증가시킴
>>- 컴포지션 API
>>>- 컴포넌트를 작성할 때 함수 기반의 방법으로 작성
>>>- 특정 역할에 따른 함수의 분리 등을 통하여 훨씬 가독성 높고 잘 조직화된 코드를 만듬
>>>- 컴포지션 API를 통해 은닉화된 코드는 컴포넌트들이 재활용할 수 있음

<br>

[목차로 이동](#목차)

>### SFC
>- Vue의 컴포넌트를 하나의 파일로 나타내는 것
>- 하나의 파일이 하나의 컴포넌트를 나타내므로 관리가 쉽고 코드가 간결해짐
>- SFC는 총 3개의 부분으로 나눠짐
>> template|컴포넌트가 렌더링되어야 하는 HTML 코드 부분<br>뒤에 설명할 선언적 렌더링 혹은 템플릿 문법을 이용하여 반응형 컴포넌트를 만들 수 있음
>> :--|:--
>> script|템플릿에서 사용한 반응성을 가지는 변수등을 조작할 수 있음<br>자바스크립트나 타입스크립트 등을 이용해 스크립트 코드를 작성함<br>\<script setup>과 같이 setup 속성을 같이 이용하면 LOC(Line Of Code)의 양을 획기적으로 줄일 수 있음. 
>> style|컴포넌트 혹은 전체 프로젝트에서 사용할 CSS 코드를 삽입할 수 있음<br>scoped 라는 속성을 이용하면 CSS가 컴포넌트에만 적용되며, scoped 라는 속성을 제외하면 전체 프로젝트에 적용됨<br>일반적으로 SCOPED라는 속성을 항상 같이 사용함

<br>

[목차로 이동](#목차)

>### 컴포지션 함수 setup
>- Vue 3는 컴포지션 API를 이용하여 컴포넌트를 만들 수 있음
>- 컴포지션 API는 setup 함수 내에서 사용이 가능함
>- Vue 2까지는 Options API만 제공
>>- Options API는 컴포넌트를 생성할 때 옵션 속성이라 불리는 data, methods, computed 등을 정의해야 해서 코드가 길어지면 오히려 가독성을 낮추는 문제점이 있음
>>- 이를 극복하고자 다른 컴포지션 프레임워크들과 같이 컴포지션 API를 지원하고자 setup 함수를 제공
>- setup 함수의 구성
>>- 자바스크립트를 작성하듯이 스크립트로 작성
>>- setup 함수는 객체를 반환하는데 이 객체 내에는 화면을 담당하는 HTML에서 사용할 변수들이 들어있어야 함

<br>

[목차로 이동](#목차)

>### MVVM Pattern
>- Model + View + ViewModel
>>- Model: 순수 자바스크립트 객체
>>- View: 웹 페이지의 DOM
>>- ViewModel: Vue
>>>- 기존에는 자바스크립트로 View에 해당하는 DOM에 접근하거나 수정하기 위해 jQuery와 같은 라이브러리를 이용
>>>- Vue는 View와 Model을 연결하고 자동으로 바인딩하여 양방향 통신을 가능케 함

<br>

[목차로 이동](#목차)

>### 설치방법
>- Direct \<script> include
>>- download
>>- CDN: \<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js">\</s>
>- NPM
>- CLI

<br>

[목차로 이동](#목차)

>### 기초 프레임
>- ```html
>   <!DOCTYPE html>
>   <html lang="en">
>   <head>
>     <meta sharset="UTF-8">
>     <meta name="viewport" content="width=device-width, inital-scale=1.0">
>     <title>Vue.js</title>
>     <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
>   </head>
>   <body>
>     <div id="app">
>       <h2>{{message}}</h2>
>     </div>
>     <script>
>       new Vue({
>         el:'#app',
>         data: {
>           message: '첫 번쨰 Vue.js 앱'
>         }
>       });
>     </script>
>   </body>
>   </html> 
>   ```

<br>

[목차로 이동](#목차)

>### 선언적 렌더링
>- Vue는 선언적 렌더링을 지원함
>>- 즉, 변수를 선언하고 값을 넣으면 자동으로 DOM에 업데이트됨 (반응형)
>>- Options API를 사용하기 위해선 단순히 data 옵션에 변수를 선언
>>- 컴포지션 API와 함께 이용하기 위해선 setup 함수를 생성하고 그 안에 일반적인 자바스크립트 변수를 선언하듯이 선언
>>- 선언된 변수는 템플릿 변수와 결합될 수 있도록 반드시 객체 형식으로 반환되어야 함
>>>- 템플릿에서 변수의 값을 나타내기 위해선 해당 변수를 두 개의 중괄호로 감싸주며 수염표기법이라 부름
>>>>```html
>>>><template>
>>>>  <div id="date">
>>>>    {{ data }}
>>>>  </div>
>>>>  <div id="date2">
>>>>    {{ data2 }}
>>>>  </div>
>>>>
>>>>  <p>{{data}}</p>
>>>>  <p v-text="data"></p>
>>>></template>
>>>><script>
>>>>  export default {
>>>>    // Composition API
>>>>    setup() {
>>>>      const date = Date().toString();
>>>>      return {
>>>>        date,
>>>>      }
>>>>    },
>>>>
>>>>    // Options API
>>>>    data() {
>>>>      return {
>>>>        date2: Date().toString();
>>>>      }
>>>>    },
>>>>  }
>>>></script>
>>>>```

---

## Vue Instance
>### Vue Instance 생성
>- Vue Instance 생성 코드
>>```html
>><script>
>>  new Vue({ // 인스턴스
>>    el:'#app',
>>    data: {
>>      message: '첫 번쨰 Vue.js 앱'
>>    }
>>  });
>></script>
>>```
>>> 속성|설명
>>> :--|:--
>>> el|Vue가 적용될 요소 지정. CSS Selector or HTML Element
>>> data|Vue에서 사용되는 정보 저장. 객체 또는 함수의 형태
>>> template|화면에 표시할 HTML, CSS 등의 마크업 요소를 정의하는 속성<br>뷰의 데이터 및 기타 속성들도 함께 화면에 그릴 수 있음
>>> methods|화면 로직 제어와 관계된 method를 정의하는 속성<br>마우스 클릭 이벤트 처리와 같이 화면의 전반적인 이벤트와 화면 동작과 관련된 로직을 추가
>>> created|Vue Instance가 생성되자 마자 실행할 로직을 정의
>- Vue Instance의 유효범위
>>- Vue Instance를 생성하면 HTML의 특정 범위 안에서만 옵션 속성들이 적용
>>- el 속성과 밀접한 관련이 있음
>>- 인스턴스가 화면에 적용되는 과정
>>>1. 뷰 라이브러리 파일 로딩
>>>2. Vue()로 인스턴스 객체 생성(옵션 속성 포함)
>>>3. el 속성에 지정한 특정 화면 요소(DOM)에 인스턴스를 붙임
>>>4. data 속성이 el 속성에 지정한 화면 요소와 그 이하 레벨의 화면 요소에 적용되어 값이 치환
>>>5. 변환된 화면 요소를 사용자가 최종 확인

<br>

[목차로 이동](#목차)

---

## Vue Instance Life-Cycle
>- 컴포넌트를 생성하여 DOM 노드 트리에 마운트하고, 불필요한 엘리먼트를 제거하는 일련의 과정
>>- Vue는 각 생명주기를 후킹(Hooking)할 수 있는 방법을 제공하는데, 이를 생명주기 훅이라고 함
>- Life Cycle은 Instance의 생성, 생성된 Instance를 화면에 부착, 화면에 부착된 Instance의 내용이 갱신, Instance가 제거되는 소멸의 4단계로 나뉨
>- Life Cycle 속성|설명
>   :--|:--
>   beforeCreate|Vue Instance가 생성되고 각 정보의 설정 전에 호출<br>DOM과 같은 화면요소에 접근 불가
>   &nbsp;|컴포지션 API의 setup 함수 그 자체가 beforeCreate를 대체함
>   created|Vue Instance가 생성된 후 데이터들의 설정이 완료된 후 호출<br>Instance가 화면에 부착하기 전이기 때문에 template 속성에 정의된 DOM 요소는 접근 불가<br>서버에 데이터를 요청(http 통신)하여 받아오는 로직을 수행하기 좋음
>   &nbsp;|컴포지션 API의 setup 함수가 beforeCreate와 함께 created도 대체함<br>컴포넌트의 옵션에 접근이 가능하기 때문에 data 옵션에 선언한 데이터들을 초기화할 때 많이 사용
>   beforeMount|마운트가 시작되기 전에 호출
>   (onBeforeMount)|Vue의 가상 노드가 render 함수를 호출하기 직전에 호출됨<br>즉, 실제 DOM을 구성하기 직전에 호출<br>beforeMount 사이클이 지나고 나면 Vue는 Virtual DOM에 가상으로 Rendering할 DOM을 미리 구성함<br>이 과정은 onRenderTracked라는 생명주기 훅을 통해 관찰할 수 있음
>   mounted|지정된 element에 Vue Instance 데이터가 마운트 된 후에 호출<br>template 속성에 정의한 화면 요소에 접근할 수 있어 화면 요소를 제어하는 로직 수행
>   (onMounted)|실제 엘리먼트에 동적으로 변화를 줘야 할 경우 이 함수에서 처리<br>실제 엘리먼트를 참조한다는것은 Virtual DOM이 실제 DOM에 반영되었음을 의미<br>onRenderTriggered라는 생명주기 훅이 이후 호출
>   beforeUpdate|데이터가 변경될 때 virtual DOM이 랜더링, 패치 되기 전에 호출
>   (onBeforeUpdated)|데이터가 변경되었지만 아직 DOM에 반영되지 않았을 때 호출. 
>   updated|Vue에서 관리되는 데이터가 변경되어 DOM이 업데이트 된 상태. 데이터 변경 후 화면 요소 제어와 관련된 로직을 추가
>   (onUpdated)|데이터가 변경되어 DOM이 변경완료된 시점에 호출됨<br>이 순간부터는 DOM이 업데이터되었다고 보고 해당 DOM에 참조된 변수를 이용해 다양한 역할을 수행할 수 있음<br>주의할 점으로 해당 엘리먼트의 자식 노드들이 업데이트가 완료되었다고 보장하지 않음<br>즉, 현재 컴포넌트만 수정이 되었음을 보장하는것<br>자식 컴포넌트까지 모두 수정된 것을 기다리기 위해서는 nextTick을 이용해 모든 자식의 업데이트가 완료되었음을 기다려야함 
>   beforeDestroy|Vue Instance가 제거되기 전에 호출
>   destroyed|Vue Instance가 제거된 후에 호출
>   beforeUnmount<br>(onBeforeUnmount)|컴포넌트가 탈착되기 직전에 호출됨<br>아직 모든 기능을 사용할 수 있는 상태이므로 명시적으로 컴포넌트가 Unmount 되기 전에 해야할 것들을 작성
>   unmounted<br>(onUnmounted)|컴포넌트가 탈찰되고 나서 호출<br>이 순간부터 모든 디렉티브와 이벤트 사용이 불가능해짐
>   activated<br>(onActivated)|keep-alive 태그는 컴포넌트가 다시 랜더링되는 것을 방지하고 상태를 유지하기 위해 쓰임<br>일반적으로 v-is 디렉티브와 함께 쓰여 v-is 디렉티브가 컴포넌트를 변경할 때 기존 컴포넌트의 상태가 사라지지 않게 하기 위해 사용<br>이러한 keep-alive 태그로 컴포넌트의 상태가 보존되기 시작하면 onActivated 생명주기 훅 함수가 호출됨
>   deactivated<br>(onDeactivated)|keep-alive로 상태가 유지되던 컴포넌트가 효력을 상실하면 호출<br>소스코드를 수정한 후 저장하면 Vite의 HMR이 해당 컴포넌트를 다시 랜더링하게 되는데, 이 때 keep-alive로 activated된 컴포넌트에 deactivated가 호출됨을 확인할 수 있음
>   renderTracked<br>(onTenderTracked)|Virtual DOM이 변경될 때마다 관찰을 목적으로 해당 생명주기 훅이 호출됨<br>이 함수를 통해 DebuggerEvent 개체를 살펴보면 어떤 이유로 Virtual DOM이 변경되는지 알 수 있음<br>DebuggerEvent는 target이란 속성을 통해 Virtual DOM을 변경시키는 것을 추적할 수 있음
>   renderTriggered<br>(onRenderTriggered)|Virtual DOM이 DOM으로 반영이 되어야 할 때 호출<br>따라서 onMounted, onActivated, onUpdated와 같이 실제 DOM이 변경되기 직전에 호출됨을 알 수 있음<br>어떤 이유로 렌더링이 호출되었는지 확인하기 위해서 onRenderTracked와 마찬가지로 DebuggerEvent를 살펴보면 됨<br>예를 들어, 새로운 값이 추가되어 렌더링이 다시 되어야 한다면 DebuggerEvent의 type 속성에는 'add', newValue 속성에 변화를 일으킨 새로운 값이 들어있을 것임
>   errorCaptured<br>(onErrorCaptured)|자손 컴포넌트에 에러가 발생하면 어느 컴포넌트에서 어떤 에러가 발생했는지 알려줌<br>실제 동작 중에 이러한 에러가 발생하면 안되기에 일반적으로 개발 중 에러를 캡쳐하기 위해 사용함

<br>

[목차로 이동](#목차)

---

## template - 보간법(interpolation)
>### 보간법(interpolation)
>- 문자열
>>- 데이터 바인딩의 가장 기본 형태는 "Mustache" 구분(이중 중괄호)을 사용한 텍스트 보간
>>>- {{ 속성명 }}
>>- v-once 디렉티브를 사용하여 데이터 변경 시 업데이트 되지 않는 일회성 보간을 수행
>>>```html
>>><span>메시지: {{msg}}</span>
>>><span v-once>변경되지 않음</span>
>>>```
>- 원시 HTML
>>- 이중 중괄호는 HTML이 아닌 일반 텍스트로 데이터를 해석함
>>- 실제 HTML을 출력하려면 v-html 디렉티브 사용
>>>```html
>>><div id="app">
>>>  <div>메세지: {{message}}</div>
>>>  <div v-text="'메세지: '+message">무시</div>
>>>  <div v-html="'메세지: '+message">무시</div>
>>></div>
>>><script>
>>>  new Vue({
>>>    el: '#app',
>>>    data: {
>>>      message: '<h2>Hello</h2>'
>>>    }
>>>  });
>>></script>
>>>```
>- JavaScript 표현식 사용
>>- Vue.js는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원함
>>>```html
>>>{{number+1}}
>>>{{ok?'YES':'NO'}}
>>>{{message.split('').reverse().join('')}}
>>><div v-bind:id="'list-'+id"></div>
>>>```
>>- 한 가지 제한사항으로 각 바인딩에 하나의 단일 표현식만 포함될 수 있음
>>>```html
>>>{{var a=1}} <!-- 구문은 표현식이 아님 -->
>>>{{if(ok) {return message}}} <!-- 조건문은 작동하지 않으므로 삼항 연산자를 이용 -->
>>>```

<br>

[목차로 이동](#목차)

---

## template - Directice
>### 디렉티브(Directives)
>- 특징
>>- 디렉티브는 v-접두사가 있는 특수 속성
>>- 디렉티브 속성 값은 단일 JavaScript 표현식이 됨 (v-for 예외)
>>- 디렉티브의 역할은 표현식의 값이 변경될 때 사이드 이펙트를 반응적으로 DOM에 적용
>- 구성요소
>> v-text|v-bind|v-else|v-html
>>  :--|:--|:--|:--
>>  v-show|v-for|v-once|v-if
>>  v-cloak|v-model|v-else-if|v-on
>>- v-model
>>>- 양방향 바인딩 처리 (from의 input, textarea에 사용)
>>>- ```html
>>>   <input type="text" v-model="msg">
>>>   ```
>>- v-bind
>>>- 엘리먼트 속성과 바인딩 처리를 위해 사용
>>>- ':' 약어로 사용 가능
>>>- ```html
>>>   <div v-bind:id="idValue">v-bind</div>
>>>   <div :id="idValue">v-bind</div>
>>>   <button v-bind:[key]="btnId">버튼</button>
>>>   <button :[key]="btnId">버튼</button>
>>>   <a v-bind:href="link1">다음</a>
>>>   <a :href="link2">네이버</a>
>>>   ```
>>- v-show
>>>- 조건에 따라 엘리먼트를 화면에 랜더링
>>>- style의 display를 변경
>>>- ```html
>>>   <div v-show="isShow">{{msg}}</div>
>>>   ```
>>- v-if, v-else-if, v-else
>>>- 조건에 따라 엘리먼트를 화면에 랜더링
>>>- ```html
>>>   <input type="number" v-model="age" />
>>>   <span v-if="age < 10">무료</span>
>>>   <span v-else-if="age < 13">1000원</span>
>>>   <span v-else-if="age < 60">3000원</span>
>>>   <span v-else>5000원</span>
>>>   ```
>>- v-show 와 v-if의 차이점
>>> &nbsp|v-if|v-show
>>>  :--|:--|:--
>>>  랜더링|false인 경우 x|항상 o
>>>  false 인 경우|엘리먼트 삭제|display:none 적용
>>>  template 지원|o|x
>>>  v-else 지원|o|x
>>- v-for
>>>- 배열이나 객체의 반복에 사용
>>>- v-for="요소변수이름 in 배열" 또는 v-for="요소변수이름,인덱스) in 배열
>>>- ```html
>>>   <ul>
>>>     <li v-for="name in regions">{{name}}</li>
>>>   </ul>
>>>   ```
>>- template
>>>- 여러 태그들을 묶어서 처리해야 할 경우 사용
>>>- 주로 v-if, v-for, component 등과 함께 사용
>>>- ```html
>>>   <template v-if="count%2 == 0">
>>>     <h4>각 태그에 v-if 할 필요없이 한번에 처리</h4>
>>>     <h4>각 태그에 v-if 할 필요없이 한번에 처리</h4>
>>>     <h4>각 태그에 v-if 할 필요없이 한번에 처리</h4>
>>>   </template>
>>>   <template v-for="(region, index) in school" v-if="region.count == count">
>>>     <h1>지역:{{region.name}} ({{region.count}}학급)</h1>
>>>   </template>
>>>   ```
>>- v-cloak
>>>- Vue Instance가 준비될 때까지 mustache 바인딩을 숨기는데 사용
>>>- [v-cload] {display:none} 같은 CSS 규칙과 함께 사용
>>>- Vue Instance가 준비되면 v-cloak는 제거됨
>>>- ```html
>>>   <style>
>>>     [v-cloak]::before {
>>>       content: '로딩중';
>>>     }
>>>     [v-cloak] > * {
>>>       display: none;
>>>     }
>>>   </style>
>>>   <div id="app">
>>>     <h3>1. 일반 - {{msg}}</h3>
>>>     <div v-cloak>
>>>       <h3>2. v-cloak - {{msg}}</h3>
>>>     </div>
>>>   </div>
>>>   <script>
>>>     setTimeout(function(){
>>>       new Vue({
>>>         el: "#app",
>>>         data: {
>>>           msg: "hello"
>>>         }
>>>       });
>>>     }, 3000);
>>>   </script>
>>>   ```
>>- Vue method
>>>- Vue Instance는 생성과 관련된 data 및 method의 정의 가능
>>>- method 안에서 data를 "this.데이터이름" 으로 접근 가능
>>>- ```html
>>>   <div id="app">
>>>     <div>data: {{message}}</div>
>>>     <div>method kor: {{helloKor()}}</div>
>>>     <div>method eng: {{helloEng()}}</div>
>>>   </div>
>>>   <script>
>>>     new Vue({
>>>       el: "#app",
>>>       data: {
>>>         message: "hello", name: "홍길동"
>>>       },
>>>       methods: {
>>>         helloEng() {
>>>           return "hello "+this.name
>>>         },
>>>         helloKor() {
>>>           return "안녕 "+this.name
>>>         }
>>>       }
>>>     });
>>>   </script>
>>>   ```

<br>

[목차로 이동](#목차)

---

