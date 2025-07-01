# CSS Cascading Style Sheet

: 웹 페이지의 디자인과 레이아웃을 구성하는 언어

- 예시
  
  > - `h1` : 선택자(Selector)
  > 
  > - `color: red;  font-size: 30px;` : 선언(Declaration)
  > 
  > - `color`, `font-size` : 속성(Property)
  > 
  > - `red`, `30px` : 값(Value)
  >   
  >   ```css
  >   h1 {
  >     color: red;
  >     font-size: 30px;
  >   }
  >   ```

- #### CSS 적용 방법
  
  - ##### 인라인(Inline) 스타일
    
    - HTML 요소 안에 style 속성 값으로 작성
    
    > ```html
    > <!DOCTYPE html>
    > <html lang="en">
    > <head>
    >   ...
    > </head>
    > <body>
    >   <h1 style="color: blue; backgroung-color: yellow;">Hello World!</h1>
    > </body>
    > </html>
    > ```
  
  - ##### 내부(Internal) 스타일 시트
    
    - head 태그 안에 style 태그 작성
    
    > ```html
    > <!DOCTYPE html>
    > <html lang="en">
    > <head>
    >   ...
    >   <title>Document</title>
    >   <style>
    >     h1 {
    >     color: blue;
    >     backgroung-color: yellow;
    > }
    >   </style>
    > </head>
    > ```
  
  - ##### 외부(External) 스타일 시트
    
    - 별도 CSS 파일 생성 후 HTML link 태그를 사용해 불러오기
    
    > ```html
    > <!DOCTYPE html>
    > <html lang="en">
    > <head>
    >   ...
    >   <link rel="stylesheet" href="style.css">
    > <title>Document</title>
    > </head>
    > <body>
    >   <h1>Hello World!</h1>
    > </body>
    > </html
    > ```
    > 
    > ```css
    > /* style.css */
    > h1 {
    >   color: blue;
    >   backgroung-color: yellow;
    > }
    > ```

---

## CSS Selectors

: HTML 요소를 선택해 스타일을 적용할 수 있도록 하는 선택자

- ##### 기본 선택자
  
  - 전체 선택자 `*`: HTML 모든 요소 선택
  
  - 요소(tag) 선택자 : 지정한 모든 태그 선택
  
  - 클래스(class) 선택자 `.(dot)` : 주어진 클래스 속성을 가진 모든 요소 선택
  
  - 아이디(id) 선택자 `#` : 주어진 아이디 속성을 가진 요소 선택. 문서에는 주어진 아이드를 가진 요소가 하나만 있어야 함
  
  - 속성(attr) 선택자 등

- ##### 결합자 (Combinators)
  
  - 자손 결합자 ` (space)` : 첫번째 요소의 자손 요소들 선택
  
  - 자식 결합자 `>` : 첫번째 요소의 직계 자식만 선택
    
    > - `p span` : <p> 안에 있는 모든 <span> 선택(하위 레벨 상관 없이)
    > - `ul > li` : <ul> 안에 있는 모든 <li> 선택(한 단계 아래 자식들만)

- 예시
  
  > ```html
  > <body>
  >   <h1 class="green">Heading</h1>
  >   <h2>선택자 연습</h2>
  >   <h3>Hello</h3>
  >   <h4>Nice to meet you</h4>
  >   <p id="purple">과목 목록</p>
  >   <ul class="green">
  >     <li>파이썬</li>
  >     <li>알고리즘</li>
  >     <li>웹
  >       <ol>
  >         <li>HTML</li>
  >         <li>CSS</li>
  >         <li>PYTHON</li>
  >       </ol>
  >     </li>
  >   </ul>
  >   <p class="green">Lorem, <span>ipsum</span> dolor.</p>
  > </body>
  > ```
  > 
  > ```html
  > <style>
  >   /* 전체 선택자 */
  >   * {
  >     color: red;
  >   }
  > 
  >   /* 요소 선택자 */
  >   h2 {
  >     color: orange;
  >   }
  > 
  > h3,
  > h4 {
  >     color: blue;
  >   }
  > 
  >   /* 클래스 선택자 */
  >   .green {
  >     color: green;
  >   }
  > 
  >   /* id 선택자 */
  >   #purple {
  >     color: purple;
  >   }
  > 
  >   /* 자식 결합자 */
  >   .green > span {
  >     font-size: 50px;
  >   }
  > 
  >   /* 자손 결합자 */
  >   .green li {
  >     color: brown;
  >   }
  > </style>
  > ```
  > 
  > ![기본선택자](https://github.com/user-attachments/assets/2c998949-0b8a-415e-839a-e75e57820946)

---

## 명시도 Specificity

: 결과적으로 요소에 적용될 CSS 선언을 결정하기 위한 알고리즘

- CSS Selector에 가중치를 계산해 어떤 스타일을 적용할지 결정

- 동일한 요소를 가리키는 2개 이상의 CSS 규칙이 있는 경우, 가장 높은 명시도를 가진 Selector가 승리해 스타일이 결정됨

- 한 요소에 동일한 가중치를 가진 선택자가 적용될 때, CSS에서 마지막에 나오는 선언이 사용됨

- 명시도가 높은 순
  
  1. Importance : `!important`
  2. Inline 스타일
  3. 선택자 : id 선택자 > class 선택자 > 요소 선택자
  4. 소스 코드 선언 순서
  
  > ```html
  > <p>1</p>
  > <p class="orange">2</p>
  > <p class="green orange">3</p>
  > <p class="orange green">4</p>
  > <p id="red" class="orange">5</p>
  > <h2 id="red" class="orange">6<h2>
  > <p id="red" class="orange" style="color: brown;">7</p>
  > <h2 id="red" class="orange" style="color:brown;">8</h2>
  > ```
  > 
  > ```html
  > h2 {
  >     color: darkviolet !important;
  > }
  > 
  > p {
  >     color: blue;
  > }
  > 
  > .orange {
  >     color: orange;
  > }
  > 
  > .green {
  >     color: green;
  > }
  > 
  > #red {
  >     color: red;
  > }
  > ```
  > 
  > ![명시도 순서](https://github.com/user-attachments/assets/e96066e1-d280-4fb0-b49e-7304c60edead)

- 명시도 관련 문서
  
  - [그림으로 보는 명시도](https://specifishity.com/)
  - [명시도 계산기](https://specificity.keegan.st)

---

## CSS 상속

: 기본적으로 CSS는 상속을 통해 부모 요소의 속성을 자식에게 상속해 재사용성을 높임

- MDN의 각 속성별 문서 하단에서 상속 여부 확인 가능

- 분류
  
  - 상속되는 속성
    - Text 관련 요소(font, color, text-align), opacity, visibility 등
  - 상속되지 않는 속성
    - Box model 관련 요소(width, height, border, box-sizing ..)
    - position 관련 요소(position, top/right/bottom/left, z-index) 등
  
  > ```html
  > <ul class="parent">
  >   <li class="child">Hello</li>
  >   <li class="child">Bye</li>
  > </ul>
  > ```
  > 
  > ```html
  > .parent {
  >     /* 상속 o */
  >     color: red;
  >     /* 상속 x */
  >     border: 1px solid black;
  > }
  > ```
  > 
  > ![상속예시](https://github.com/user-attachments/assets/b8214685-2c00-457d-a543-c9f199de85a0)

---

- CSS 스타일 가이드
  - 코드 구조와 포멧팅
    - 일관된 들여쓰기를 사용(보통 2칸 공백)
    - 선택자와 속성은 각각 새 줄에 작성
    - 중괄호 앞에 공백 넣기
    - 속성 뒤에는 콜론`:`과 공백 넣기
    - 마지막 속성 뒤에는 세미콜론`;` 넣기
  - 선택자 사용
    - class 선택자를 우선적으로 사용
    - id, 요소 선택자 등은 가능한 피할 것
    - 여러 선택자들과 함께 사용할 경우 우선순위 규칙에 따라 예기치 못한 스타일 규칙이 적용되어 전반적인 유지보수가 어려워지기 때문
  - 속성과 값
    - 속성과 값은 소문자로 작성
    - 0값에는 단위를 붙이지 않음
  - 명명 규칙
    - 클래스 이름은 의미있고 목적을 나타내는 이름을 사용
    - 케밥 케이스(kebab-case)를 사용
    - 약어보다는 전체 단어를 사용
  - CSS 적용 스타일
    - 인라인(inline) 스타일은 되도록 사용하지 말 것
    - CSS와 HTML 구조 정보가 혼합되어 작성되기 때문에 코드를 이해하기 어렵게 만듦
