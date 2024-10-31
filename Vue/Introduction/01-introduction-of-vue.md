- [Fronted Development](#fronted-development)
  - [Client-side frameworks](#client-side-frameworks)
  - [SPA](#spa)
  - [CSR](#csr)
- [Vue](#vue)
  - [What is Vue](#what-is-vue)
  - [Component](#component)
- [Vue Application](#vue-application)
  - [Vue Application 생성](#vue-application-생성)
  - [반응형 상태](#반응형-상태)
  - [Vue 기본 구조](#vue-기본-구조)
- [Template Syntax](#template-syntax)
  - [Temaplate Syntax](#temaplate-syntax)
    - [Text Interpolation](#text-interpolation)
    - [Raw HTML](#raw-html)
    - [Attribute Bindings](#attribute-bindings)
    - [JavaScript Expressions](#javascript-expressions)
    - [Expressions 주의 사항](#expressions-주의-사항)
- [참고](#참고)
  - [ref 객체](#ref-객체)
  - [Ref Unwrap 주의사항](#ref-unwrap-주의사항)
  - [SEO](#seo)
  - [CSR과 SSR](#csr과-ssr)

# Fronted Development

- Fronted Development
  - 웹사이트와 웹 애플리케이션의 사용자 인터페이스(UI)와 사용자 경험(UX)을 만들고 디자인하는 것
  - HTML, CSS, JavaScript 등을 활용하여 사용자가 직접 상호작용하는 부분을 개발  
    <img src ='images\fronted-development.png' width=450 style='margin:8px'>  

## Client-side frameworks

- 클라이언트 측에서 UI와 상호작용을 개발하기 위해 사용되는 JavaScript 기반 프레임워크
- Client-side frameworks가 필요한 이유
  - 이유 1
    - "웹에서 하는 일이 많아졌다"
    - 단순히 무언가를 읽는 곳에서 -> 무언가를 하는 곳으로
    - 사용자는 이제 웹에서 문서만을 읽는 것이 아닌 음악을 스트리밍하고, 영화를 보고, 지구 반대편 사람들과 텍스트 및 영상 채팅을 통해 즉시 통신을 하고 있음
    - 이처럼 현대적이고 복잡한 대화형 웹 사이트를 "**웹 애플리케이션(web applications)**"이라 부름
    - JavaScript 기반의 Client-side frameworks가 등장하면서 매우 동적인 대화형 애플리케이션을 훨씬 더 쉽게 구축할 수 있게 됨
  - 이유 2
    - 웹에서 하는 일이 많아졌다 = 다루는 데이터가 많아졌다.
    - 만약 친구가 이름을 변경했다면? 친구 목록, 타임라인, 스토리 등 친구 이름이 출력되는 모든 곳이 함께 변경되어야 함
    - 애플리케이션의 기본 데이터를 안정적으로 추가하고 업데이트(렌더링, 추가, 삭제 등)하는 도구가 필요
    - **애플리케이션의 상태를 변경할 때마다 일치하도록 UI를 업데이트해야 함**
    - Vanilla JS만으로는 쉽지 않다  
      <img src ='images\vanilla-js.png' width=600 style='margin:8px'>  
- Client-side frameworks의 필요성
  - 동적이고 반응적인 웹 애플리케이션 개발
    - 실시간 데이터 업데이트
  - 코드 재사용성 증가
    - 컴포넌트 기반 아키텍처
    - 모듈화된 코드 구조
  - 개발 생산성 향상 
    - 강력한 개발 도구 지원

## SPA

- SPA; Single Page Application
  - 단일 페이지에서 동작하는 웹 애플리케이션
- SPA 작동 원리
  - 최초 로드 시 필요한 모든 리소스 다운로드
  - 이후 페이지 갱신에 대해 필요한 데이터만을 비동기적으로 전달 받아 화면의 필요한 부분만 동적으로 갱신
    - AJAX와 같은 기술을 사용하여 필요한 데이터만 비동기적으로 로드
    - 페이지 전체를 다시 로드할 필요 없이 필요한 데이터만 서버로부터 가져와서 화면에 표시
  - JavaScript를 사용하여 클라이언트 측에서 동적으로 콘텐츠를 생성하고 업데이트
    - CSR 방식

## CSR

- CSR; Client-side Rendering
  
  - 클라이언트에서 콘텐츠를 렌더링 하는 방식

- CSR 작동 원리
  
  1. 사용자가 웹사이트에 요청을 보냄
  2. 서버는 최소한의 HTML과 JavaScript 파일을 클라이언트로 전송
  3. 클라이언트는 HTML과 JavaScript를 다운로드 받음
  4. 브라우저가 JavaScript를 실행하여 동적으로 페이지 콘텐츠를 생성
  5. 필요한 데이터는 API를 통해 서버로부터 비동기적으로 가져옴

- CSR 작동 예시  
  <img src ='images\csr.png' width=450 style='margin:8px'>  
  
  1. 클라이언트는 서버로부터 최소한의 HTML 페이지와 해당 페이지에 필요한 JavaScript 응답 받음
  2. 그런 다음 클라이언트 측에서 JavaScript를 사용하여 DOM을 업데이트하고 페이지를 렌더링
  3. 이후 서버는 더 이상 HTML을 제공하지 않고 요청에 필요한 데이터만 응답
  - Google Maps, Facebook, Instagram 등의 서비스에서 페이지 갱신 시 새로고침이 없는 이유

- SPA와 CSR의 장점
  
  1. 빠른 페이지 전환
     - 페이지가 처음 로드된 후에는 필요한 데이터만 가져오면 되고 JavaSCript는 전체 페이지를 새로 고칠 필요 없이 페이지의 일부를 다시 렌더링 할 수 있기 때문
     - 서버로 전송되는 데이터의 양을 최소화 (서버 부하 방지) 
  2. 사용자 경험
     - 새로고침이 발생하지 않아 네이티브 앱과 유사한 사용자 경험을 제공 
  3. Frontend와 Backend의 명확한 분리 
     - Frontend는 UI 렌더링 및 사용자 상호 작용 처리를 담당 & Backend는 데이터 및 API 제공을 담당
     - 대규모 애플리케이션을 더 쉽게 개발하고 유지 관리 가능 

- SPA와 CSR의 단점
  
  1. 느린 초기 로드 속도
     - 전체 페이지를 보기 전에 약간의 지연을 느낄 수 있음
     - JavaScript가 다운로드, 구문 분석 및 실행될 때까지 페이지가 완전히 렌더링 되지 않기 때문
  2. SEO (검색 엔지 최적화) 문제
     - 페이지를 나중에 그려 나가는 것이기 때문에 검색에 잘 노출되지 않을 수 있음
     - 검색엔진 입장에서 HTML을 읽어서 분석해야 하는 데 아직 콘텐츠가 모두 존재하지 않기 때문

- SPA vs. MPA / CSR vs. SSR
  
  - MPA; Multi Page Application
    - 여러 개의 HTML 파일이 서버로부터 각각 로드
    - 사용자가 다른 페이지로 이동할 때 마다 새로운 HTML 파일이 로드 됨
  - SSR; Server-side Rendering
    - 서버에서 화면을 렌더링 하는 방식
    - 모든 데이터가 담긴 HTML을 서버에서 완성 후 클라이언트에게 전달

# Vue

## What is Vue

- Vue.js
  
  - 사용자 인터페이스를 구축하기 위한 JavaScript 프레임워크

- What is Vue?
  
  - Evan You에 의해 발표(2014)
    - Angular 개발팀 출신
    - (학사 - 미술, 미술사/ 석사 - 디자인 & 테크놀로지 전공)
  - 최신 버전은 'Vue 3' (2024)
    - https://vuejs.org/
  - Vue 2 문서에 접속하지 않도록 주의

- Vue를 학습하는 이유
  
  1. 낮은 학습 곡선
     - 간결하고 직관적인 문법을 가지고 있어 빠르게 익힐 수 있음
     - 잘 정리된 문서를 기반으로 어렵지 않게 학습 할 수 있음 
  2. 확장성과 생태계
     - 다양한 플러그인과 라이브러리를 제공하는 높은 확장성
     - 전세계적으로 활성화된 커뮤니티를 기반으로 많은 개발자들이 새로운 기능을 개발하고 공유하고 있음 
  3. 유연성 및 성능
     - 작은 규모의 프로젝트부터 대규모의 애플리케이션까지 다양한 프로젝트에 적합 
  4. 가장 주목받는 Client-side framework 

- SSAFY에서의 Vue
  
  - Vue는 React나 Angular 대비 간결하고 직관적인 문법을 가지고 있어 학습이 상대적으로 원할하기 때문에 짧은 시간내에 효율적으로 결과물을 만들어 낼 수 있음
  - 거대하고 활발한 커뮤니티를 가지고 있어 풍부한 문서, 튜토리얼, 예제 및 다양한 리소스를 공유 받을 수 있음(최신 업데이트 및 트렌드를 공유함으로써 지속적 학습 촉진 가능)

- Vue 체험하기
  
  - <img src ='images\vue.png' width=750 style='margin:8px'>   
  - 결과 : <img src ='images\vue02.png' width=300 style='margin:8px'>   

- Vue의 2가지 핵심 기능
  
  - 선언적 렌더링(Declarative Rendering)
    - 표준 HTML을 확장하는 Vue "템플릿 구문"을 사용하여 JavaScript 상태(데이터)를 기반으로 화면에 출력될 HTML을 선언적으로 작성
  - 반응성 (Reactivity) 
    - JavaScript 상태 변경을 추적하고, 변경사항이 발생하면 자동으로 DOM을 업데이트

- Vue의 주요 특징 정리
  
  - 반응형 데이터 바인딩
    - 데이터 변경 시 자동 UI 업데이트
  - 컴포넌트 기반 아키텍처
    - 재사용 가능한 UI 조각
  - 간결한 문법과 직관적인 API
    - 낮은 학습 곡선
    - 높은 가독성
  - 유연한 스케일링
    - 작은 프로젝트부터 대규모 애플리케이션까지 적합

## Component

- 재사용 가능한 코드 블록
- Component 특징
  - UI를 독립적이고 재사용 가능한 일부분으로 분할하고 각 부분을 개별적으로 다룰 수 있음
  - 자연스럽게 애플리케이션은 중첩된 Component의 트리 형태로 구성됨  
    <img src ='images\component.png' width=500 style='margin:8px'>  
- Component 예시
  - 웹 서비스는 여러 개의 Component로 이루어져 있음  
    <img src ='images\component02.png' width=600 style='margin:8px'>  

# Vue Application

- Vue 사용하는 방법
  - CDN 방식
  - NPM 설치 방식
    - CDN 방식 이후 진행

## Vue Application 생성

1. CDN 작성
   
   ```html
   <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
   ```
   
    <img src ='images\vue-application.png' width=600 style='margin:8px'>  

2. Application instance
   
   1. CDN에서 Vue를 사용하는 경우 전역 Vue 객체를 불러오게 됨
   2. 구조분해할당 문법으로 Vue 객체의 createApp 함수를 할당  
      <img src ='images\vue-application02.png' width=600 style='margin:8px'>   
   3. 모든 Vue 애플리케이션은 createApp 함수로 새 Application instance를 생성하는 것으로 시작함  
      <img src ='images\vue-application03.png' width=600 style='margin:8px'>   

3. Root Component
   
   1. createApp 함수에는 객체(컴포넌트)가 전달됨
   2. 모든 App에는 다른 컴포넌트들을 하위 컴포넌트로 포함할 수 있는 Root(최상위) 컴포넌트가 필요(현재는 단일 컴포넌트)  
      <img src ='images\vue-application04.png' width=600 style='margin:8px'>   

4. Mounting the App (앱 연결)
   
   1. HTML 요소에 Vue Application instance를 탑재(연결)
   2. 각 앱 인스턴스에 대해 `mount()`는 한 번만 호출할 수 있음   
      <img src ='images\vue-application05.png' width=600 style='margin:8px'>     

## 반응형 상태

- `ref()`
  - 반응형 상태(데이터)를 선언하는 함수 (declaring reactive state)
  - 반응형을 가지는 참조 변수를 만드는 것
    - (ref === reactive reference)
- ref 함수
  - `.value` 속성이 있는 ref 객체로 래핑(wrapping)하여 반환하는 함수
  - ref로 선언된 변수의 값이 변경되면, 해당 값을 사용하는 템플릿에서 자동으로 업데이트
  - 인자는 어떠한 타입도 가능  
    <img src ='images\ref.png' width=600 style='margin:8px'>      
  - 템플릿의 참조에 접근하려면 `setup` 함수에서 선언 및 반환 필요
  - 편의상 템플릿에서 ref를 사용할 때는 `.value`를 작성할 필요 없음 (automatically unwrapped)  
    <img src ='images\ref02.png' width=600 style='margin:8px'>     

## Vue 기본 구조

- `createApp()`에 전달되는 객체는 Vue 컴포넌트

- 컴포넌트의 상태는 `setup()` 함수 내에서 선언되어야 하며 **객체를 반환해야 함**  
  <img src ='images\vue-fabric.png' width=600 style='margin:8px'>    

- 템플릿 렌더링
  
  - 반환된 객체의 속성은 템플릿에서 사용할 수 있음
  - Mustache syntax(콧수염 구문)를 사용하여 메시지 값을 기반으로 동적 텍스트를 렌더링  
    <img src ='images\rendering.png' width=600 style='margin:8px'>    
  - 콘텐츠는 식별자나 경로에만 국한되지 않으며 유효한 JavaScript 표현식을 사용할 수 있음
    <img src ='images\rendering02.png' width=500 style='margin:8px'>  

- Event Listeners in Vue
  
  - `v-on` directive를 사용하여 DOM 이벤트를 수신할 수 있음
    - 자세한 directive 학습은 다음 시간에 진행  
      <img src ='images\event.png' width=750 style='margin:8px'>  
  - 함수 내에서 반응형 변수를 변경하여 구성 요소 상태를 업데이트 

# Template Syntax

- DOM을 기본 구성 요소 인스턴스의 데이터에 **선언적으로 바인딩(Vue Instance와 DOM을 연결)** 할 수 있는 HTML 기반 **템플릿 구문(확장된 문법 제공)** 을 사용

## Temaplate Syntax

- Template Syntax 종류
  - Text Interpolation
  - Raw HTMl
  - Attribute Bindings
  - JavaScript Expressions

### Text Interpolation

<img src ='images\template-syntax.png' width=450 style='margin:8px'>      

- 데이터 바인딩의 가장 기본적인 형태
- 이중 중괄호 구문 (콧수염 구문)을 사용
- 콧수염 구문은 해당 구성 요소 인스턴스의 msg 속성 값으로 대체
- msg 속성이 변경될 때마다 업데이트 됨

### Raw HTML

<img src ='images\template-syntax02.png' width=600 style='margin:8px'>      

- 콧수염 구문은 데이터를 일반 텍스트로 해석하기 때문에 실제 HTML을 출력하려면 `v-html`을 사용해야 함

### Attribute Bindings

<img src ='images\template-syntax03.png' width=450 style='margin:8px'>      

- 콧수염 구문은 HTML 속성 내에서 사용할 수 없기 때문에 `v-bind`를 사용
- HTMl의 **`id`** 속성 값을 vue의 **`dynamicId`** 속성과 동기화 되도록 함
- 바인딩 값이 `null`이나 `undefind`인 경우 렌더링 요소에서 제거됨

### JavaScript Expressions

<img src ='images\template-syntax04.png' width=500 style='margin:8px'>    

- Vue는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원
- Vue 템플릿에서 JavaScript 표현식을 사용할 수 있는 위치
  - 콧수염 구문 내부
  - 모든 directive의 속성 값 (**"v-"**로 시작하는 특수 속성)

### Expressions 주의 사항

- 각 바인딩에는 하나의 단일 표현식만 포함될 수 있음
  - 표현식은 값으로 평가할 수 있는 코드 조각(return 뒤에 사용할 수 있는 코드여야 함)
- 작동하지 않는 경우
   <img src ='images\template-syntax05.png' width=500 style='margin:8px'>    

# 참고

## ref 객체

- ref 객체가 필요한 이유
  - 일반적인 변수가 아닌 객체 데이터 타입으로 사용하는 이유는?
  - Vue는 템플릿에서 `ref`를 사용하고 나중에 `ref`의 값을 변경하면 자동으로 변경 사항을 감지하고 그에 따라 DOM을 업데이트함 ("의존성 추적 기반의 반응형 시스템")
  - Vue는 렌더링 중에 사용된 모든 `ref`를 추적하며, 나중에 `ref`가 변경되면 이를 추적하는 구성 요소에 대해 다시 렌더링
  - 이를 위해 참조 자료형의 객체 타입으로 구현한 것
    - JavaScript에서는 일반 변수의 접근 또는 변화를 감지할 방법이 없기 때문
    - https://vuejs.org/guide/essentials/reactivity-fundamentals.html#why-refs  
- 반응형 변수 vs. 일반 변수
  <img src ='images\reactive-value.png' width=500 style='margin:8px'>      
  <img src ='images\reactive-value02.png' width=200 style='margin:8px'>    

## Ref Unwrap 주의사항

📌 템플릿에서의 unwrap시 주의사항

- 템플릿에서의 unwrap은 ref가 최상위 속성인 경우에만 적용 가능

- 다음 표현식은 어떻게 출력될까?    
  <img src ='images\unwrap.png' width=450 style='margin:8px'>       
  
  - object는 최상위 속성이지만 **object.id**는 그렇지 않음
  - 표현식을 평가할 때 **object.id**가 unwrap되지 않고 **ref** 객체로 남아 있기 때문

- 이 문제를 해결하기 위해서는 '**id를 최상위 속성으로 분해**'해야 함  
  <img src ='images\unwrap02.png' width=450 style='margin:8px'>  

- 단, ref가 "{{ }}"의 최종 평가 값인 경우 unwrap 가능  
  <img src ='images\unwrap03.png' width=450 style='margin:8px'>  
  
  - 다음과 동일   
    
    <img src ='images\unwrap04.png' width=200 style='margin:8px'>  

## SEO

- SEO; Search Engine Optimization
  - google, bing과 같은 검색 엔진 등에 내 서비스나 제품 등이 효율적으로 검색 엔진에 노출되도록 개선하는 과정을 일컫는 작업
  - 정보의 대상은 주로 HTMl에 작성된 내용
  - 검색
    - 각 사이트가 운용하는 검색 엔진에 의해 이루어지는 작업
  - 검색 엔진
    - 웹 상에 존재하는 가능한 모든 정보들을 긁어 모으는 방식으로 동작
- 최근에는 SPA, 즉 CSR로 구성된 서비스의 비중이 증가
- SPA 서비스도 검색 대상으로 넓히기 위해 JS를 지원하는 방식으로 발전하는 중

## CSR과 SSR

- CSR와 SSR은 흑과 백이 아니다.
- 애플리케이션의 목적, 규모, 성능 및 SEO 요구 사항에 따라 달라질 수 있다.
  - 내 서비스에 적합한 렌더링 방식을 적절하게 활용할 수 있어야 한다.
- SPA 서비스에서도 SSR을 지원하는 Framework가 발전하고 있음
  - Vue의 Nuxt.js
  - React의 Next.js