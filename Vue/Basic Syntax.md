# Basic Syntax 01

- [Template Syntax](#template-syntax)
  
  - [Directive](#directive)

- [Dynamically data binding](#dynamically data binding)
  
  - [v-bind](#vbind)
  
  - [Attribute Bindings](#attribute-bindings)
  
  - [Class and Style Bindings](#class-and-style-bindings)

- [Event Handling](#event-handling)
  
  - [v-on](#von)
  
  - [Modifiers](#modifiers)

- [Form Input Bindings](#form-input-bindings)
  
  - [v-bind with v-on](vbind-with-von)
  
  - [v-model](#vmodel)
  
  - [v-model 활용](#vmodel-활용)

- [Computed Properties](#computed-prorperties)

- [Conditional Rendering](#conditional-rendering)
  
  - [v-if](#v-if)
  - [v-show](#v-show)

- [List Rendering](#list-rendering)
  
  - [v-for](#v-for)

- [Watchers](#watchers)

- [Lifecycle Hooks](#lifecycle-hooks)

- [참고](#참고)
  
  - [접두어 $](#접두어-)
  
  - [IME](#ime)

---

## Template Syntax

: DOM을 기본 구성 요소 인스턴스의 데이터에 **선언적으로 바인딩**할 수 있는 HTML 기반 **템플릿 구문**을 사용

- Vue Instance와 DOM을 연결

- 확장된 문법 제공

- 종류
  
  1. **Text Interpolation**
     
     ```html
     <p>Message: {{ msg }}</p>
     ```
     
     - 데이터 바인딩의 가장 기본적인 형태
     
     - 이중 중괄호 구문 (콧수염 구문)을 사용
     
     - 콧수염 구문은 해당 구성 요소 인스턴스의 `msg` 속성 값으로 대체
     
     - `msg` 속성이 변경될 때마다 업데이트 됨
  
  2. **Raw HTML**
     
     ```html
     <div v-html="rawHtml"></div>
     ```
     
     ```javascript
     const rawHtml = ref('<span style="color:red">This should be red.</span>')
     ```
     
     - 콧수염 구문은 데이터를 일반 텍스트로 해석하기 때문에 실제 HTML을 출력하려면 `v-html`을 사용해야 함
  
  3. **Attribute Bindings**
     
     ```html
     <div v-bind:id="dynamicId"></div>
     ```
     
     ```javascript
     const dynamicId = ref('my-id')
     ```
     
     - 콧수염 구문은 HTML 속성 내에서 사용할 수 없기 때문에 `v-bind`를 사용
     
     - HTML의 `id` 속성 값을 vue의 `dynamicId` 속성과 동기화 되도록 함
     
     - 바인딩 값이 `null` 이나 `undefind`인 경우 렌더링 요소에서 제거됨
  
  4. **JavaScript Expressions**
     
     ```html
     {{ number +1 }}
     {{ ok ? 'YES' : 'NO' }}
     {{ message.split('').reverse().join('') }}
     <div v-bind:id="`list-${id}`"></div>
     ```
     
     - Vue는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원
     
     - Vue 템플릿에서 JavaScript 표현식을 사용할 수 있는 위치
       
       1. 콧수염 구문 내부
       
       2. 모든 directive의 속성 값 (`v-`로 시작하는 특수 속성)
     
     > ##### Expressions 주의사항
     > 
     > - 각 바인딩에는 하나의 단일 표현식만 포함될 수 있음
     >   
     >   (표현식은 값으로 평가할 수 있는 코드 조각으로 `return` 뒤에 사용할 수 있는 코드여야 함)
     > 
     > - 작동하지 않는 예시
     >   
     >   ```javascript
     >   // 표현식이 아닌 선언식
     >   {{ const number = 1 }}
     >   // 제어문은 삼항 표현식을 사용해야 함
     >   {{ if (ok) { return message } }}
     >   ```

#### Directive

: `v-` 접두사가 있는 특수 속성

- 특징
  
  - Directive의 속성 값은 단일 JavaScript 표현식이어야 함
    
    (`v-for`, `v-on` 제외)
  
  - 표현식 값이 변경될 때 DOM에 반응적으로 업데이트를 적용
  
  - 예시
    
    ```html
    <p v-if="seen">Hi There</p>
    ```

- 전체 구문
  
  : `v-on:submit.prevent="onSubmit"`
  
  > - `v-on` : Name. `v-`로 시작. shorthands 사용 시 생략 가능
  > 
  > - `submit` : Argument. `:` 혹은 shorthand symbol 다음에 옴. 없을수도 있음
  > 
  > - `prevent` : Modifiers. 선두 점`.` 으로 표시. 없을수도 있음
  > 
  > - `"onSubmit"` : Value. JavaScript 표현으로 번역됨
  
  - **Arguments**
    
    - 일부 directive는 directive 뒤에 콜론 `:`으로 표시되는 인자를 사용할 수 있음
    
    - 아래 예시는 `href`는 HTML <a> 요소의 `href` 속성 값을 `myUrl` 값에 바인딩하도록 하는 `v-bind`의 인자
      
      ```html
      <a v-bind:href="myUrl">Link</a>
      ```
    
    - 아래 예시의 `click`은 수신할 이벤트 이름을 작성하는 `v-on`의 인자
      
      ```html
      <button v-on:click="doSomething">Button</button>
      ```
  
  - **Modifiers**
    
    - `.` (dot)로 표시되는 특수 접미사로, directive가 특별한 방식으로 바인딩되어야 함을 나타냄
    
    - 아래 예시의 `.prevent`는 발생한 이벤트에서 `event.preventDefault()`를 호출하도록 `v-on`에 지시하는 modifier
      
      ```html
      <form v-on:submit.prevent="onSubmit">
        <input type="submit">
      </form>
      ```

- Built-in Directives
  
  - `v-text`, `v-show`, `v-if`, `v-for` 등
  
  - [Built-in Directives | Vue.js](https://vuejs.org/api/built-in-directives.html)

---

## Dynamically data binding

#### v-bind

: 하나 이상의 속성 또는 컴포넌트 데이터를 표현식에 동적으로 바인딩

- 사용처
  
  1. Attribute Bindings 
  
  2. Class and Style Bindings

#### Attribute Bindings

: 속성 바인딩

- HTML의 속성 값을 Vue의 상태 속성 값과 동기화 되도록 함
  
  ```html
  <!-- v-bind.html -->
  <img v-bind:src="imageSrc">
  <a v-bind:href="myUrl">Move to url</a>
  ```

- `v-bind` shorthand (약어) : `:` (colon)
  
  ```html
  <img :src="imageSrc">
  <a :href="myUrl">Move to url</a>
  ```

- Dynamic attribute name (동적 인자 이름)
  
  - 대괄호 `[]`로 감싸서 directive argument에 JavaScript 표현식을 사용할 수 있음
  
  - 표현식에 따라 동적으로 평가된 값이 최종 argument 값으로 사용됨
    
    ```html
    <button :[key]="myValue"></button>
    ```
    
    > 대괄호 안에 작성하는 이름은 반드시 소문자로만 구성 가능
    > 
    > (브라우저가 속성 이름을 소문자로 강제 변환하기 때문)

- 예시
  
  ```html
  <!-- v-bind.html -->
  <img :src="imageSrc">
  <a :href="myUrl">Move to url</a>
  <p :[dynamcattr]="dynamicValue">...</p>
  ```
  
  ```javascript
  const { createApp, ref } = Vue
  const app = createApp({
    setup() {
      const imageSrc = ref('https://picsum.photos/200')
      const myUrl = ref('https://www.google.co.kr/')
      const dynamicattr = ref('title')
      const dynamicValue = ref('Hello Vue.js')
      return {
        imageScrc,
        myUrl,
        dynamicattr,
        dynamicValue
        }
      }
  })
  app.mount('#app')
  ```

#### Class and Style Bindings

: 클래스와 스타일 바인딩

- `class`와 `style`은 모두 HTMl 속성이므로 다른 속성과 마찬가지로 `v-bind`를 사용하여 동적으로 문자열 값을 할당할 수 있음

- Vue는 `class` 및 `style` 속성 값을 `v-bind`로 사용할 때 **객체** 또는 **배열**을 활용하여 작성할 수 있도록 함
  
  (단순히 문자열 연결을 사용하여 이러한 값을 생성하는 것은 번거롭고 오류가 발생하기가 쉽기 때문)

- 가능한 경우
  
  1. <mark>**Binding `HTML` Classes**</mark>
     
     1. Binding to `Objects`
        
        - 객체를 `:class`에 전달하여 클래스를 동적으로 전환할 수 있음
        
        - 객체에 더 많은 필드를 포함하여 여러 클래스를 전활할 수 있음
        
        - 반드시 inline 방식으로 작성하지 않아도 됨
        
        - 반응형 변수를 활용해 객체를 한 번에 작성하는 방법
        
        > ```js
        > const ifActive = ref(false)  // setup() 안에 작성
        > const hasInfo = ref(true)  // setup() 안에 작성
        > // const classObj = ref({
        > //        active: isActive,
        > //        'text-primary': hasInfo
        > //     })
        > 
        > <div :class="{ active: isActive }">Text</div>
        > // <div class="active">Text</div> 와 동일하게 작동
        > <div class="static" :class="{ active: isActive, 'text-primary': hasInfo }">Text</div>
        > ```
     
     2. **Binding to `Arrays`**
        
        - `:class`를 배열에 바인딩하여 클래스 목록을 적용할 수 있음
        
        - 배열 구문 내에서 객체 구문을 사용하는 경우
  
  2. <mark>**Binding Inline `Styles`**</mark>
     
     1. **Binding to `Objects`**
        
        - `:style`은 JavaScript 객체 값에 대한 바인딩을 지원 (HTML `style` 속성에 해당)
        
        - 실제 CSS에서 사용하는 것처럼 `:style`은 `kebab-cased` 키 문자열도 지원 (단, `camelCase` 작성을 권장)
        
        - 반드시 inline 방식으로 작성하지 않아도 됨
        
        - 반응형 변수를 활용해 객체를 한 번에 작성하는 방법
     
     2. **Binding to `Arrays`**
        
        - 여럴 스타일 객체를 배열에 작성해서 `:style`을 바인딩할 수 있음
        
        - 작성한 객체는 병합되어 동일한 요소에 적용

- v-bind 종합
  
  [Built-in Directives | Vue.js](https://vuejs.org/api/built-in-directives.html#v-bind)

---

## Event Handling

#### v-on

: DOM 요소에 이벤트 리스너를 연결 및 수신

```javascript
v-on:event="handler"
```

- handler 종류
  
  1. Inline handlers (간단한 상황)
     
     : 이벤트가 트리거 될 때 실행될 JavaScript 코드
     
     - 메서드 이름에 직접 바인딩하는 대신 Inline Handlers에서 메서드를 호출할 수도 있음
     
     - 이렇게 하면 기본 이벤트 대신 사용자 지정 인자를 전달할 수 있음
       
       ```javascript
       const greeting = function (message) {
         console.log(message)
       }
       ```
       
       ```html
       <button @click="greeting('hello')">Say hello</button>
       <button @click="greeting('bye')">Say bye</button>
       ```
     
     - 원래 DOM 이벤트에 접근하기
     
     - `$event` 변수를 사용하여 메서드에 전달
       
       ```javascript
       const warning = function (message, event) {
         console.log(message)
         console.log(event)
       }
       ```
       
       ```html
       <button @click="warning('경고입니다.', $event)">Say bye</button>
       ```
  
  2. Method handlers (이외의 상황)
     
     : 컴포넌트에 정의된 메서드 이름
     
     - 트리거하는 기본 DOM `Event` 객체를 자동으로 수신
       
       ```javascript
       const myFunc = function (event) {
         console.log(event)
         console.log(event.currentTarget)
         console.log(`Hello ${name.value}!`)
       }
       ```

- `v-on` shorthand (약어) : `@`
  
  ```javascript
  @event="handler"
  ```

#### Modifiers

- Event Modifiers를 활용해 `event.preventDefault()`와 같은 구문을 메서드에서 작성하지 않도록 함

- `stop`, `prevent`, `self` 등 다양한 modifiers를 제공

> 메서드는 DOM 이벤트에 대한 처리보다는 데이터에 관한 논리를 작성하는 것에 집중할 것
> 
> - Modifiers는 chained 되게끔 작성할 수 있으며 이때는 작성된 순서로 실행되기 때문에 작성 순서에 유의

- **Key Modifiers**
  
  - 키보드 이벤트를 수신할 때 특정 키에 관한 별도 modifiers를 사용할 수 있음
  
  - 예시로 key가 Enter일 때만 `onSubmit` 이벤트를 호출하기
    
    ```html
    <input @keyup.enter="onSubmi">
    ```

- v-on 종합
  
  [Built-in Directives | Vue.js](https://vuejs.org/api/built-in-directives.html#v-on)

---

## Form Input Bindings

: 폼 입력 바인딩

- `form`을 처리할 때 사용자가 `input`에 입력하는 값을 실시간으로 JavaScript 상태에 동기화해야 하는 경우 (**양방향 바인딩**)

- 양방향 바인딩 방법
  
  1. `v-bind`와 `v-on`을 함께 사용
  
  2. `v-model` 사용

#### v-bind with v-on

1. `v-bind`를 사용하여 `input` 요소의 `value` 속성 값을 입력 값으로 사용

2. `v-on`을 사용하여 `input` 이벤트가 발생할 때마다 `input` 요소의 `value` 값을 별도 반응형 변수에 저장하는 핸들러를 호출
   
   ```javascript
   // form-input-bindings.html
   const inputText1 = ref('')
   const onInput = function (event) {
     inputText1.value = event.currentTarget.value
   }
   ```
   
   ```html
   <p>{{ inputText1 }}</p>
   <input :value="inputText1" @input="onInput">
   ```

#### v-modle

: `form input` 요소 또는 컴포넌트에서 양방향 바인딩을 만듦

- 사용자 입력 데이터와 반응형 변수를 실시간 동기화
  
  ```javascript
  const inputText2 = ref('')
  ```
  
  ```html
  <p>{{ inputText2 }}</p>
  <input v-model="inputText2">
  ```

- 사용
  
  - 사용자 입력 데이터와 반응형 변수를 실시간 동기화
  
  - IME가 필요한 언어(한국어, 중국어, 일본어 등)의 경우 `v-model`이 제대로 업데이트되지 않음
  
  - 해당 언어에 대한 올바르게 응답하려면 `v-bind`와 `v-on` 방법을 사용해야 함

- 활용
  
  - `Checkbox`
    
    1. 단일 체크박스와 `boolean` 값 활용
    
    2. 여러 체크박스와 배열 활용 (해당 배열에는 현재 선택된 체크박스의 값이 포함됨)
    
    > ```js
    > const checked = ref(false)  // setup()
    > 
    > <input type="checkbox" id="checkbox" v-model="checked">
    > <label for="checkbox">{{ checked }}</label>
    > ```
    > 
    > ```js
    > const checkedNames = ref([])  // setup()
    > 
    > <input type="checkbox" id="alice" value="Alice" v-model="checkedNames">
    > <label for="alice">Alice</label>
    > <input type="checkbox" id="bella" value="Bella" v-model="checkedNames">
    > <label for="bella">Bella</label>
    > ```
  
  - `Select`
    
    - `select`에서 `v-model` 표현식의 초기 값이 어떤 `option` 과도 일치하지 않은 경우 `select` 요소는 "선택되지 않은(unselected)" 상태로 렌더링 됨
    
    > ```js
    > const selected = ref('') // setup()
    > 
    > <select v-model="selected">
    >   <option disabled value="">Please select one</option>
    >   <option>Alice</option>
    >   <option>Bella</option>
    >   <option>Cathy</option>
    > </select>
    > ```

- v-model 종합
  
  [Built-in Directives | Vue.js](https://vuejs.org/api/built-in-directives.html#v-model)

---

## Computed Properties

## `Computed()`

: '계산된 속성'을 정의하는 함수

- 미리 계산된 속성을 사용하여 템플릿에서 표현식을 단순하게 하고 불필요한 반복 연산을 줄임

- 특징
  
  - 반환되는 값은 `computed ref`이며 일반 `refs`와 유사하게 계산된 결과를 `.value`로 참조할 수 있음(템플릿에서는 `.value` 생략 가능)
  
  - `computed` 속성은 의존된 반응형 데이터를 **자동으로 추적**
  
  - 의존하는 데이터가 **변경될 때만 재평가**

- `computed`와 `method` 차이
  
  - `computed`는 의존된 데이터가 변경되면 자동으로 업데이트되고, `method`는 호출해야만 실행되므로 무조건 `computed`만 사용하는 것이 아니라 사용 목적과 상황에 맞게 `computed`와 `method`를 적절히 조합하여 사용
  
  - `computed` 
    
    - 의존된 반응형 데이터를 기반으로 캐시(cached) 됨
    
    - 의존하는 데이터가 변경된 경우에만 재평가됨
    
    - 즉, 의존된 반응형 데이터가 변경되지 않는 한 이미 계산된 결과에 대한 여러 참조는 다시 평가할 필요 없이 이전에 계산된 결과를 즉시 반환
    
    - 적절한 사용처
      
      - 의존하는 데이터에 따라 결과가 바뀌는 계산된 속성을 만들 때 유용
      
      - 동일한 의존성을 가진 여러 곳에서 사용할 때 계산 결과를 캐싱하여 중복 방지
  
  - `method` 
    
    - 다시 랜더링이 발생할 때마다 항상 함수를 실행
    
    - 적절한 사용처
      
      - 단순히 특정 동작을 수행하는 함수를 정의할 때 사용
      
      - 데이터에 의존하는지 여부와 관계없이 항상 동일한 결과를 반환하는 함수

- Cache (캐시)
  
  : 데이터나 결과를 일시적으로 저장해두는 임시 저장소
  
  - 이후에 같은 데이터나 결과를 다시 계산하지 않고 빠르게 접근할 수 있도록 함
  
  - 예시 - 웹 페이지의 캐시 데이터
    
    - 과거 방문한 적이 있는 페이지에 다시 접속할 경우, 페이지 일부 데이터를 브라우저 캐시에 저장 후 같은 페이지에 다시 요청 시 모든 데이터를 다시 응답받는 것이 아닌 일부 캐시도니 데이터를 사용하여 더 빠르게 웹 페이지를 렌더링

- 예시 - 할 일이 남았는지 여부에 따라 다른 메시지를 출력하기
  
  > - 사용 전
  >   
  >   ```js
  >   const todos = ref([
  >     { text: 'Vue 실습' },
  >     { text: '자격증 공부' }
  >     { text: 'TIL 작성 }
  >   ])
  >   
  >   <h2>남은 할 일</h2>
  >   <p>{{ todos.length > 0 ? '아직 남았다' : '퇴근!' }}</p>
  >   ```
  >   
  >   - 템블릿이 복잡해지며 `todos`에 따라 계산을 수행하게 됨
  >   - 만약 이 계산을 템플릿에 여러번 사용하는 경우에는 반복이 발생
  > 
  > - `computed` 적용 후
  >   
  >   ```js
  >   const { createApp, ref, computed } = Vue
  >   
  >   const restOfTodos = computed(() => {
  >     return todos.value.length > 0 ? '아직 남았다' : '퇴근!'
  >   })
  >   
  >   <h2>남은 할 일</h2>
  >   <p>{{ restOfTodos }}</p>
  >   ```
  >   
  >   - 반응형 데이터를 포함하는 복잡한 로직의 경우 `computed`를 활용하여 미리 값을 계산하여 계산된 값을 사용
  >   - `restOfTodos`의 계산은 `todos`에 의존하고 있음
  >   - 따라서 `todos`가 변경될 때만 `restOfTodos`가 업데이트 됨
  > 
  > - `method` 사용
  >   
  >   ```js
  >   const getRestOfTodos = function () {
  >       return todos.value.length > 0 ? '아직 남았다' : '퇴근!'
  >   }
  >   
  >   <p>{{ getRestOfTodos() }}</p>
  >   ```

---

## Conditional Rendering

#### v-if

: 표현식 값의 `true/false`를 기반으로 요소를 조건부로 렌더링

- `v-if`
  
  : `v-if` directive를 사용하여 조건부로 렌더링
  
  > ```js
  > const ifSeen = ref(true)
  > 
  > <p v-if="ifSeen"> true일 때 보여요</p>
  > ```

- `v-else`
  
  : `v-else` directive를 사용하여 `v-if`에 대한 `else` 블록을 나타낼 수 있음
  
  > ```js
  > const isSeen = ref(true)
  > 
  > <p v-if="ifSeen"> true일 때 보여요</p>
  > <[ v-else>false일 때 보여요</p>
  > <button @click="isSeen = !isSeen">토글</button>
  > ```

- `v-else-if`
  
  : `v-else-if` directive를 사용하여 `v-if`에 대한 `else if` 블록을 나타낼 수 있음
  
  > ```js
  > const isSeen = ref('Cathy')
  > 
  > <div v-if="name === 'Alice'">Alice입니다</div>
  > <div v-else-if="name === 'Bella'">Bella입니다</div>
  > <div v-else-if="name === 'Cathy'">Cathy입니다</div>
  > <div v-else>아무도 아닙니다</div>
  > ```

- 여러 요소에 대한 `v-if` 적용
  
  - HTML `template` 요소에 `v-if`를 사용하여 하나 이상의 요소에 대해 적용할 수 있음 (`v-else`, `v-else-if` 모두 적용 가능)
  
  > ```js
  > <template v-if="name === 'Cathy'">
  >   <div>Cathy입니다</div>
  >   <div>나이는 30살입니다</div>
  > </template>
  > ```
  
  - HTML `template` element
    : 페이지가 로드될 때 렌더링되지 않지만 JavaScript를 사용하여 나중에 문서에서 사용할 수 있도록 하는 HTML을 보유하기 위한 메커니즘
    - 보이지 않는 wrapper 역할

#### v-show

: 표현식 값의 `true/false`를 기반으로 요소의 가시성(visibility)을 전환

- `v-show`요소는 항상 DOM에 렌더링 되어있음

- CSS `display` 속성만 전환하기 때문

> ```js
> const isShow = ref(false)
> 
> <div v-show="isShow">v-show</div>
> ```

#### v-if와 v-show

- `v-if` (Cheap initial load, expensive toggle)
  
  - 초기 조건이 false인 경우 아무 작업도 수행하지 않음
  
  - 토글 비용이 높음

- `v-show` (Expensive initial load, cheap toggle)
  
  - 초기 조건에 관계 없이 항상 렌더링
  
  - 초기 렌더링 비용이 더 높음

- 콘텐츠를 매우 자주 전환해야 하는 경우에는 `v-show`를, 실행 중에 조건이 변경되지 않는 경우에는 `v-if`를 권장

---

## List Rendering

#### v-for

: 소스 데이터(`Array`, `Object`, `Number`, `String`, `Iterable`)를 기반으로 요소 또는 템플릿 블록을 여러 번 렌더링

- `v-for`는 `alias in expression` 형식의 특수 구문을 사용
  
  ```js
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

- 인덱스(객체이서는 key)에 대한 별칭 지정 가능
  
  - 키와 값이 아닌 값과 키인 것에 주의!
  
  ```js
  <div v-for="(item, index) in arr"></div>
  
  <div v-for="value in object"></div>
  <div v-for="(value, key) in object"></div>
  <div v-for="(value, key, index) in object"></div>
  ```

- 예시
  
  > - 배열 반복
  >   
  >   ```js
  >   const myArr = ref([
  >     { name: 'Alice', age: 20},
  >     { name: 'Bella', age: 21}
  >   ])
  >   
  >   <div v-for="(item, index) in myArr">
  >     {{ index }} / {{ item }}
  >   </div>
  >   ```
  > 
  > - 객체 반복
  >   
  >   ```js
  >   const myObj = ref({
  >     name: 'Cathy', 
  >     age: 20
  >   })
  >   
  >   <div v-for="(value, key, index) in myObj">
  >     {{ index }} / {{ key }} / {{ value }}
  >   </div>
  >   ```

- 여러 요소에 대한 `v-for` 적용
  
  - HTML `template` 요소에 `v-for`를 사용하여 하나 이상의 요소에 대해 반복 렌더링할 수 있음
    
    ```js
    <ul>
    <template v-for="item in myArr">
      <li>{{ item.name }}</li>
      <li>{{ item.age }}</li>
    </template>
    </ul>
    ```

- 중첩된 `v-for`
  
  - 각 `v-for`의 하위 영역(scope)은 상위 영역에 접근할 수 있음
    
    > ```js
    > const myInfo = ref([
    >   { name: 'Alice', age: 20, friends: ['Bella', 'Cathy', Dan']},
    >   { name: 'Bella', age: 21, friends: ['Alice', 'Cathy']}
    > ])
    > 
    > <ul v-for="item in myInfo">
    >   <li v-for="friend in item.friends">
    >     {{ item.name }} - {{ friend }}
    >   </li>
    > </ul>
    > ```

- 반드시 `v-for`와 `key`를 함께 사용한다
  
  - 내부 컴포넌트의 상태를 일관된게 하여 데이터의 예측 가능한 행동을 유지하기 위함
  
  - `v-for`와 `key`
    
    - `key`는 반드시 각 요소에 대한 고유한 값을 나타낼 수 있는 식별자여야 함
      
      > ```js
      > let id = 0
      > 
      > const items = ref([
      >   { id: id++, name: 'Alice' },
      >   { id: id++, name: 'Bells' }
      > ])
      > 
      > <div v-for="item in items" :key="item.id">
      >   <!-- content -->
      > </div>
      > ```
    
    - 내장 특수 속성 `key`
      
      - `number` 혹은 `string`으로만 사용해야 함
      
      - Vue의 내부 가상 DOM 알고맂므이 이전 목록과 새 노드 목록을 비교할 때 각 node를 식별하는 용도로 사용
      
      - Vue 내부 동작 관련된 부분이기에 최대한 작성하려고 노력할 것
      
      - [Key 참고 문서](https://vuejs.org/api/built-in-special-attributes.html#key)

- 동일 요소에 `v-for`와 `v-if`를 함께 사용하지 않는다
  
  - 동일한 요소에서 `v-if`가 `v-for`보다 우선순위가 더 높기 때문
  
  - `v-if`에서의 조건은 `v-for` 범위의 변수에 접근할 수 없음
  
  - 예시
    
    > - `todo` 데이터 중 이미 처리한(`isComplete === true) todo`만 출력하기
    >   
    >   - `v-if`가 더 높은 우선순위를 가지므로 `v-for` 범위의 `todo` 데이터를 ` v-if`에서 사용할 수 없음
    >   
    >   ```js
    >   let id = 0
    >   
    >   const todos = ref([
    >     { id: id++, name: '복습', isComplete: true },
    >     { id: id++, name: '예습', isComplete: false },
    >     { id: id++, name: '저녁식사', isComplete: true },
    >     { id: id++, name: '노래방', isComplete: false }
    >   ])
    >   
    >   <ul>
    >     <li v-for="todo in todos" v-if="!todo.isComplete" :key="todo.id">
    >       {{ todo.name }}
    >     </li>
    >   </ul>
    >   
    >   // Uncaught TypeError 출력
    >   ```
  
  - 해결법
    
    - **`computed`를 활용해 이미 필터링 된 목록을 반환하여 반복**하도록 설정
      
      > ```js
      > const completeTodos = computed(() => {
      >     return todos.value.filter((todo) => !todo.isComplete)
      > })
      > 
      > <ul>
      >   <li v-for="todo in completeTodos" :key="todo.id">
      >     {{ todo.name }}
      >   </li>
      > </ul>
      > ```
    
    - `v-for`와 `template` 요소를 사용하여 **`v-if` 위치를 이동**
      
      > ```js
      > <ul>
      >   <template v-for="todo in todos" :key="todo.id">
      >     <li v-if="!todo.isComplete">
      >     {{ todo.name }}
      >     </li>
      >   </template>
      > </ul>
      > ```

---

## Watchers

#### `watch()`

: 하나 이상의 반응형 데이터를 감시하고, 감시하는 데이터가 변경되면 콜백 함수를 호출

- 구조
  
  ```js
  watch(source, (newValue, oldValue) => {
      // do something
  }
  ```
  
  - 첫번째 인자 (`source`) : watch가 간시하는 대상(반응형 변수, 값을 반환하는 함수 등)
  
  - 두번째 인자(`callback function`) : `source`가 변경될 때 호출되는 콜백 함수
    
    - `newValue` : 감시하는 대상이 변화된 값
    
    - `oldValue` (optional) : 감시하는 대상의 기존 값

- 기본 동작
  
  ```js
  <button @click="count++">Add 1</button>
  <p>Count {{ count }}</p>
  
  const count = ref(0)
  
  watch(count, (newValue, oldValue) => {
      console.log(`newValue: ${newValue}, oldValue: ${oldValue}`)
  })
  ```

- 예시 - 감시하는 변수에 변화가 생겼을 때 연관 데이터 업데이트하기
  
  > ```js
  > <input v-model="message">
  > <p>Message length: {{ messageLength }}</p>
  > 
  > const message = ref('')
  > const messageLength = ref(0)
  > 
  > watch(message, (newValue) => {
  >     messageLength.value = newValue.length
  > }
  > ```

- 여러 source를 감시하는 watch
  
  - 배열을 활용해 여러 대상 감시 가능
  
  ```js
  watch([foo, bar], ([newFoo, newBar], [prevFoo, prevBar]) => }
    /* ... */
  })
  ```

- **Deep Watchers**
  
  - 일반 Watchers는 1차원밖에 감시하지 못함. `deep` 옵션을 콜백 함수 뒤에 넣으면 내부 깊이가 얼마가 됐던간에 감시 가능
  
  ```js
  watch(
      () => state.someObject,
      (newValue, oldValue) => {
      // ... //
      },
      { deep: true }
  )
  ```

- `computed`와 `watch`
  
  - `computed`와 `watch` 모두 의존(감시)하는 원본 데이터를 직접 변경하지 않음
  
  |       | `computed`               | `watch`                                     |
  | ----- | ------------------------ | ------------------------------------------- |
  | 공통점   | 데이터의 변화를 감지하고 처리         | 데이터의 변화를 감지하고 처리                            |
  | 동작    | 의존하는 데이터 속성의 계산된 값을 반환   | 특정 데이터 속성의 변화를 감시하고 작업을 수행 (`side-effects`) |
  | 사용 목적 | 계산한 값을 캐싱하여 재사용 중복 계산 방지 | 데이터 변환에 따른 특정 작업을 수행                        |
  | 사용 예시 | 연산된 길이, 필터링 됨 목록 계산 등    | DOM 변경, 다른 비동기 작업 수행, 외부 API와 연동 등          |

---

## Lifecycle Hooks

: Vue 컴포넌트의 생성부터 소멸까지 각 단계에서 실행되는 함수

- Lifecycle Hoods Diagram
  
  - 컴포넌트의 생애 주기 중간 중간에 함수를 제공
  
  - 개발자는 컴포넌트의 특정 시점에 원하는 로직을 실행할 수 있음
  
  - [참고 문서](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)

- 주요 Lifecycle Hoods
  
  - 생성 단계 / 마운트 단계 / 업데이트 단계 / 소멸 단계 등 다양한 단계 존재
  
  - 가장 일반적으로 사용되는 것은 `onMounted`, `onUpdated` , `onUnmounted`
  
  - [참고 문서](https://vuejs.org/api/composition-api-lifecycle.html)

- 예시
  
  > - Mounting
  >   
  >   - Vue 컴포넌트 인스턴스가 초기 렘더링 및 DOM 요소 생성이 완료된 후 특정 로직 수행하기
  >     
  >     ```js
  >     const { createApp, ref, onMounted } = Vue
  >     
  >     setup() {
  >        onMounted(() => {
  >          console.log('mounted')
  >        })
  >     }
  >     ```
  > 
  > - Updating
  >   
  >   - 반응형 데이터의 변경으로 인해 컴포넌트의 DOM이 업데이트된 후 특정 로직을 수행하기
  >     
  >     ```js
  >     <button @click="count++">Add 1</button>
  >     <p>Count: {{ count }}</p>
  >     <p>{{ message }}</p>
  >     
  >     const { createApp, ref, onMounted, onUpdated } = Vue
  >     
  >     const count = ref(0)
  >     const message = ref(null)
  >     
  >     onUpdated(() => {
  >       message.value = 'updated!'
  >     })
  >     ```

---

## Vue Style Guide

- Vue의 스타일 가이드 규칙은 우선순위에 따라 4가지 번주로 나뉨

- 규칙 범주
  
  - 우선순위 A : 필수 (Essential)
    
    - 오류를 방지하는데 도움이 되므로 어떤 경우에도 규칙을 학습하고 준수
    
    - ex) `v-for`에 `key` 작성하기, 동일 요소에 `v-if`와 `v-for` 함께 사용하지 않기
  
  - 우선순위 B : 적극 권장 (Strongly Recommended)
    
    - 가독성 및/또는 개발자 경험을 향상시킴
    
    - 규칙을 어겨도 코드는 여전히 실행되겠지만, 정당한 사유가 있어야 규칙을 위반할 수 있음
  
  - 우선순위 C : 권장 (Recommended)
    
    - 일관성을 보장하도록 임의의 선택을 할 수 있음
  
  - 우선순위 D : 주의 필요 (Use with Caution)
    
    - 잠재적 위험 특성을 고려함

- [참고 문서](https://vuejs.org/style-guide/)

---

## 참고

#### 접두어 $

- Vue 인스턴스 내에서 제공되는 내부 변수

> - 사용자가 지정한 반응형 변수나 메서드와 구분하기 위함
> 
> - 주로 Vue 인스턴스 내부 상태를 다룰 때 사용

#### IME

: Input Method Editor

- 사용자가 입력 장치에서 기본적으로 사용할 수 없는 문자(비영어권 언어)를 입력할 수 있도록 하는 운영 체제 구성 프로그램

- 일반적으로 키보드 키보다 자모가 더 많은 언어에서 사용해야 함

> IME가 동작하는 방식과 Vue의 양방향 바인딩(`v-model`) 동작 방식이 상충하기 때문에 한국어 입력 시 예상대로 동작하지 않았던 것

#### `computed` 주의 사항

- `computed`의 반환 값은 변경하지 말 것
  
  - `computed`의 반환 값은 의존하는 데이터의 파생된 값(이미 의존하는 데이터에 의해 계산이 완료된 값)
  
  - 일종의 snapshot이며 의존하는 데이터가 변경될 때만 새 snapshot이 생성됨
  
  - 계산된 값은 읽기 전용으로 취급되어야하며 변경되어서는 안됨
  
  - 대신 새 값을 얻기 위해서는 의존하는 데이터를 업데이트 해야 함

- `computed` 사용 시 원본 배열 변경하지 말 것
  
  - `computed`에서 `reverse()` 및 `sort()` 사용 시 원본 배열을 변경하기 때문에 원본 배열의 복사본을 만들어서 진행해야 함
  
  - X : `return numbers.reverse()`
  
  - O : `return [...numbers].reverse()`

#### Lifecycle Hooks 주의사항

- Lifecycle Hooks는 동기적으로 작성할 것
  
  - 컴포넌트 상태의 일관성 유지
    
    - 컴포넌트의 생명주기 동안 상태가 예측 가능하고 일관되게 유지되도록 보장
    
    - 비동기적으로 실행될 경우, 컴포넌트의 상태가 예상치 못한 시점에 변경될 수 있어 버그 발생 가능성이 높아짐
  
  - Vue 내부 매커니즘과의 동기화
    
    - Vue의 내부 로직은 컴포넌트의 라이프사이클에 맞춰 최적화되어 있음
    
    - 동기적 실행을 통해 Vue의 내부 프로세스와 개발자가 작성한 코드가 정확히 동기화될 수 있음
  
  - 비동기적으로 작성한 lifecycle hook 예시
    
    > ```js
    > setTimeout(() => {
    >     onMounted(() => {
    >       console.log('이 코드는 실행되지 않습니다!')
    >     })
    > }, 100)
    > ```

#### 배열과 `v-for` 관련

- 배열 변경 관련 메서드
  
  - `v-for`와 배열을 함께 사용 시 배열의 메서드를 주의해서 사용해야 함
  
  - 변화 메서드 : 호출하는 원본 배열을 변경
    
    - `push()`, `pop()`, `shift()`, `unshift()`, `splice()`, `sort()`, `reverse()`
  
  - 배열 교체 : 원본 배열을 수정하지 않고 항상 새 배열을 반환
    
    - `filter()`, `concat()`, `slice()`

- `v-for`와 배열을 활용해 '필터링/정렬' 활용하기
  
  - 원본 데이터를 수정하거나 교체하지 않고 필터링하거나 정렬된 새로운 데이터를 표시하는 방법
    
    - `computed` 활용 : 원본 기반으로 필터링 된 새로운 결과를 생성
      
      > ```js
      > const numbers = ref([1, 2, 3, 4, 5])
      > 
      > const evenNumbers = computed(() => {
      >     return numbers.value.filter((number) => number % 2 === 0)
      > })
      > 
      > <li v-for="num in evenNumbers">{{ num}}</li>
      > ```
    
    - `method` 활용 (`computed`가 불가능한 중첩된 `v-for`의 경우 사용)
      
      > ```js
      > const numberSets = ref([
      >     [1, 2, 3, 4, 5],
      >     [6, 7, 8, 9, 10]
      > ])
      > 
      > const evenNumbers = function (numbers) {
      >     return numbers.filter((number) => number % 2 === 0)
      > })
      > 
      > <ul v-for="numbers in numberSets">
      >   <li v-for="num in evenNumbers(numbers)">{{ num}}</li>
      > </ul>
      > ```

- 주의!! 배열의 인덱스를 `v-for`의 `key`로 사용하지 말 것
  
  - 인덱스는 식별자가 아닌 배열의 항목 위치만 나타내기 때문
  
  - 만약 새 요소가 배열의 끝이 아닌 위치 삽입되면 이미 반복도니 구성 요소 데이터가 함께 엄데이트되지 않기 때문
  
  - 직접 고유한 값을 만들어내는 메서드를 만들거나 외부 라이브러리 등을 활용하는 등 식별자 역할을 할 수 있는 값을 만들어 사용
  
  ```js
  <div v-for="(item, index) in items" :key="index">  // <- :key="index" 금지!!
    <!-- content -->
  </div>
  ```

---
