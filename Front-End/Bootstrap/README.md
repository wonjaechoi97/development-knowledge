# **Bootstrap** 🛒

## 목차
  - [반응형 웹 개요](#반응형-웹)
  - [Bootstrap 개요](#bootstrap)

<br>

---

## 반응형 웹

<br/>

- 반응형 웹의 장단점

  - 모든 스마트 기기에서 접속가능

    - 반응형 웹에서 사용하는 기술들은 W3C에서 웹 표준으로 지정한 HTML과 CSS로 구성

    - 웨어러블 기기 뿐 아니라 스마트 TV나 게임 콘솔 등 웹표준을 지원하는 모든 기기 사용가능

  - 가로 모드에 맞추어 레이아웃 변경 가능

    - 스마트폰이나 태블릿에서 가로 모드로 돌렸을 때 너비 값이 커지면 그에 맞추어 레이아웃을 자동으로 변경

  - 사이트 유지, 보수, 관리 용이

    - PC용 모바일용 코드가 따로 있는 것이 아니기 때문에 유지, 보수가 쉬움

    - 서버 쪽 코드가 아닌 HTML과 CSS로만 구성되어 복잡하지 않음

- 뷰포트 (viewport)

  - PC화면에 보이는 내용을 모바일 기기에서 그대로 볼 수 없는 이유는 PC와 모바일의 픽셀 표현 방법이 다르기 때문

  - 뷰포트를 지정하여 접속한 기기 화면에 맞추어 확대 또는 축소하여 표시할 수 있음

    > 뷰포트 : 스마트폰 화면에서 실제 내용이 표시되는 영역

  - 웹킷(webkit) 기반인 모바일 브라우저들의 기본 뷰포트 너비는 980px인데, 화면의 크기를 고려하여 320px로 맞추어 웹사이트를 제작해도 스마트폰 모바일 브라우저의 기본 뷰포트 너비가 980px이므로 글씨와 그림은 작아지는데 이를 해결하기 위해 뷰포트를 지정함

  - 뷰포트 지정
    ```html
    <meta name="viewport" content="[속성1=값], [속성2=값], ..." />
    ```
    | 속성          | 설명                | 사용 가능한 값          | 기본 값          |
    | :------------ | :------------------ | :---------------------- | :--------------- |
    | width         | 뷰포트 너비         | device-width 또는 크기  | 브라우저 기본 값 |
    | height        | 뷰포트 높이         | device-height 또는 크기 | 브라우저 기본 값 |
    | user-scalable | 확대/축소 가능 여부 | yes 또는 no             | yes              |
    | initial-scale | 초기 확대/축소 값   | 1 ~ 10                  | 1                |
    | minimum-scale | 최소 확대/축소 값   | 0 ~ 10                  | 0.25             |
    | maximum-scale | 최대 확대/축소 값   | 0 ~ 10                  | 1.6              |
    ```html
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="viewport" content="width=device-width, initial-scale=0.5" />
    ```

[목차로 이동](#목차)

<br>

---

<br>

## Bootstrap

<br>

- 개요

  - 빠르고 쉬운 웹 개발을 위한 무료 프런트 엔드 프레임 워크

  - typography, forms, buttons, tables, navigation, modal, image 및 기타 여러 가지를 위한 HTML 및 CSS 기반 디자인 템플릿과 선택적 JavaScript 플러그인이 포함되어 있음

  - 반응형 디자인을 쉽게 만들 수 있는 기능 제공

- 장점

  - 사용하기 쉬움 : HTML과 CSS에 대한 기본적인 지식만 있으면 누구나 Bootstrap을 사용할 수 있음

  - 반응형 기능: Bootstrap의 반응형 CSS는 휴대폰, 태블릿 및 데스크톱에 맞게 조정됨

  - 모바일 우선 접근 방식 : Bootstrap에서 모바일 우선 스타일은 핵심 프레임 워크의 일부임

  - 브라우저 호환성 : Bootstrap은 모든 최신 브라우저와 호환됨

- 사용

  - Bootstrap CDN
    ```html
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css"
      rel="stylesheet"
    />
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    ```

- .container

  - .container 클래스는 반응형 고정 너비 컨테이너를 제공

  - .container-fluid 클래스는 뷰포트의 전체 너비에 걸쳐 있는 전체 너비 컨테이너를 제공

- Grid System

  - 부트 스트랩의 그리드 시스템은 플랙스 박스로 구축되어 페이지에 최대 12개의 열을 허용

  - 12개 열을 모두 개벽적으로 사용하지 않으면 열을 함께 그룹화하여 더 넓은 열을 만들 수 있음

  - Grid Class

    - 클래스를 결합하여 보다 동적이고 유연한 레이아웃을 만들 수 있음

      | class       | device      | 설명(screen width 대비) |
      | :---------- | :---------- | :---------------------- |
      | .col-num    | extra small | <576px                  |
      | .col-sm-num | small       | >=576px                 |
      | .col-md-num | medium      | >=768px                 |
      | .col-lg-num | large       | >=992px                 |
      | .col-xlnum  | extra large | >=1200px                |

[목차로 이동](#목차)