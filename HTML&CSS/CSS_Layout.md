# CSS Layout

: 각 요소의 위치와 크기를 조정해 웹 페이지의 디자인을 결정하는 것 (`Display`, `Position`, `Flexbox` 등)

## CSS Position

: 요소를 Normal Flow에서 제거해 다른 위치로 배치하는 것

- 목적 : 전체 페이지에 대한 레이하웃을 구성하는 것보다는 페이지 특정 항목의 위치를 조정하는 것

- 이동 방향 : top, right, botton, left, Z Axis(겹쳤을 때)

- 유형
  
  - `static`
    
    - 요소를 Normal Flow에 따라 배치
    
    - top, right, bottom, left 속성이 적용되지 않음
    
    - 기본 값
  
  - `relative`
    
    - 요소를 Normal Flow에 다라 배치
    
    - 자신의 원래 위치(static)를 기준으로 이동
    
    - top, right, bottom, left 속성으로 위치 조정
    
    - 다른 요소의 레이아웃 영향을 주지 않음(요소가 차지하는 공간은 static일 때와 같음)
  
  - `absolute`
    
    - 요소를 Normal Flow에서 제거
    
    - 가장 가Rk운 `relative` 부모 요소를 기준으로 이동
      
      - 만족하는 부모 요소가 없다면 `body` 태그를 기준으로 함
    
    - top, right, bottom, left 속성으로 위치 조정
    
    - 문서에서 요소가 차지하는 공간이 없어짐
  
  - `fixed`
    
    - 요소를 Normal Flow에서 제거
    
    - **현재 화면 영역(viewport)을 기준**으로 이동
    
    - 스크롤해도 항상 같은 위치에 유지됨
    
    - top, right, b ottom, left 속성으로 위치 조정
    
    - 문서에서 요소가 차지하는 공간이 없어짐
    
    - ex) 웹툰에서 다음화, 이전화 선택할 수 있는 버튼
  
  - `sticky`
    
    - `relative`와 `fixed`의 특성을 결합한 속성
    
    - 스크롤 위치가 임계점에 도달하기 전에는 `relative`처럼 동작
    
    - 스크롤이 특정 임계점에 도달하면 `fixed`처럼 동작해 화면에 고정됨
    
    - 만약 다음 `sticky` 요소가 나오면 다음 `sticky`요소가 이전 `sticky`요소의 자리를 대체
      
      - 이전 `sticky`요소가 고정되어 있던 위치와 다음 `sticky`요소가 고정되어야 할 위치가 겹치게 되기 때문

- `z-index`
  
  - 요소의 쌓임 순서(stack order)를 정의하는 속성
  
  - 정수 값을 사용해 z축 순서를 지정
  
  - 값이 클수록 요소가 위에 쌓이게 됨
  
  - `static`이 아닌 요소에만 적용됨
  
  - 특징
    
    - 기본값은 `auto`
    
    - 부모 요소의 `z-index`값에 영향을 받음
    
    - 같은 부모 내에서만 `z-index` 값을 비교
    
    - 부모의 `z-index`가 낮으면 자식의 `z-index`가 아무리 높아도 부모보다 위로 올라갈 수 없음
    
    - `z-index`값이 같으면 HTML 문서 순서대로 쌓임

---

### 예시 코드

- 유형 - `static`, `absolute`, `relative`, `fixed`
  
  > ```html
  > <!DOCTYPE html>
  > <html>
  > 
  > <head>
  >   <meta charset="UTF-8">
  >   <meta name="viewport" content="width=device-width, initial-scale=1.0">
  >   <title>Document</title>
  >   <style>
  >     * {
  >       box-sizing: border-box;
  >     }
  > 
  >     body {
  >       height: 1500px;
  >     }
  > 
  >     .container {
  >       position: relative;
  >       height: 300px;
  >       width: 300px;
  >       border: 1px solid black;
  >     }
  > 
  >     .box {
  >      height: 100px;
  >       width: 100px;
  >       border: 1px solid black;
  >     }
  > 
  >     .static {
  >       position: static;
  >       background-color: lightcoral;
  >     }
  > 
  >     .absolute {
  >       position: absolute;
  >       background-color: lightgreen;
  >       top: 100px;
  >       left: 100px;
  >     }
  > 
  >     .relative {
  >       position: relative;
  >       background-color: lightblue;
  >       top: 100px;
  >       left: 100px;
  >     }
  > 
  >     .fixed {
  >       position: fixed;
  >       background-color: gray;
  >       top: 0;
  >       right: 0;
  >     }
  >   </style>
  > </head>
  > 
  > <body>
  >   <div class="container">
  >     <div class="box static">Static</div>
  >     <div class="box absolute">Absolute</div>
  >     <div class="box relative">Relative</div>
  >     <div class="box fixed">Fixed</div>
  >   </div>
  > </body>
  > </html>
  > ```

- 유형 - `sticky`
  
  > ```html
  > <!DOCTYPE html>
  > <html lang="en">
  > 
  > <head>
  >   <meta charset="UTF-8">
  >   <meta name="viewport" content="width=device-width, initial-scale=1.0">
  >   <title>Document</title>
  >   <style>
  >     body {
  >       height: 1500px;
  >     }
  > 
  >     .sticky {
  >       position: sticky;
  >       top: 0;
  >       background-color: lightblue;
  >       padding: 20px;
  >       border: 2px solid black;
  >     }
  >   </style>
  > </head>
  > 
  > <body>
  >   <h1>Sticky positioning</h1>
  >   <div>
  >     <div class="sticky">첫 번째 Sticky</div>
  >     <div>
  >       <p>내용1</p>
  >       <p>내용2</p>
  >       <p>내용3</p>
  >     </div>
  >     <div class="sticky">두 번째 Sticky</div>
  >     <div>
  >       <p>내용4</p>
  >       <p>내용5</p>
  >       <p>내용6</p>
  >     </div>
  >     <div class="sticky">세 번째 Sticky</div>
  >     <div>
  >       <p>내용7</p>
  >       <p>내용8</p>
  >       <p>내용9</p>
  >     </div>
  >   </div>
  > </body>
  > 
  > </html>
  > ```

- 유형 - `absolute`
  
  > ```html
  > <!DOCTYPE html>
  > <html lang="en">
  > 
  > <head>
  >   <meta charset="UTF-8">
  >   <meta name="viewport" content="width=device-width, initial-scale=1.0">
  >   <title>Document</title>
  >   <style>
  >     .card {
  >       position: relative;
  >       width: 300px;
  >       height: 200px;
  >       border: 1px solid black;
  >     }
  > 
  >     .card-content {
  >       padding: 10px;
  >     }
  > 
  >     .badge {
  >       position: absolute;
  >       top: 0;
  >       right: 0;
  >       background-color: red;
  >       color: white;
  >       padding: 5px 10px;
  >     }
  >   </style>
  > </head>
  > 
  > <body>
  >   <div class="card">
  >     <div class="card-content">
  >       <h3>Card Title</h3>
  >       <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
  >       <span class="badge">New</span>
  >     </div>
  >   </div>
  > </body>
  > 
  > </html>
  > ```

- 유형 - z-index
  
  > ```html
  > <!DOCTYPE html>
  > <html lang="en">
  > 
  > <head>
  >   <meta charset="UTF-8">
  >   <meta name="viewport" content="width=device-width, initial-scale=1.0">
  >   <title>Document</title>
  >   <style>
  >     .container {
  >       position: relative;
  >     }
  > 
  >     .box {
  >       position: absolute;
  >       width: 100px;
  >       height: 100px;
  >     }
  > 
  >     .red {
  >       background-color: red;
  >       top: 50px;
  >       left: 50px;
  >       z-index: 1;
  >     }
  > 
  >     .green {
  >       background-color: green;
  >       top: 100px;
  >       left: 100px;
  >       z-index: 1;
  >     }
  > 
  >     .blue {
  >       background-color: blue;
  >       top: 150px;
  >       left: 150px;
  >       z-index: 3;
  >     }
  >   </style>
  > </head>
  > 
  > <body>
  >   <div class="container">
  >     <div class="box red">z-index: 3</div>
  >     <div class="box green">z-index: 2</div>
  >     <div class="box blue">z-index: 1</div>
  >   </div>
  > </body>
  > 
  > </html>
  > ```
