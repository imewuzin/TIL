# CSS Box Model

: 웹 페이지의 모든 HTML 요소를 감싸는 사각형 상자 모델

- 원은 네모 박스를 깎은 것
  ![원은네모를깎은것](https://github.com/user-attachments/assets/43e3d21c-9a05-4b7d-ba81-cc42c9dd941b)

- 박스 타입
  : 박스 타입에 따라 페이지에서의 배치 흐름 및 다른 박스와 관련해 박스가 동작하는 방식이 달라짐
  
  1. Block box
  
  2. Inline box

## 박스 표시(Display) 타입

- ##### Outer display type
  
  : 박스가 문서 흐름에서 어떻게 동작할지를 결정
  
  - 속성 (`display:` 로 설정 가능)
    
    - `block`
      
      - 항상 새로운 행으로 나뉨
      - width와 height 속성 사용 가능
      - padding, margin, border로 인해 다른 요소를 상자로부터 밀어냄
      - width 속성을 지정하지 않으면 박스는 inline 방향으로 사용 가능한 공간을 모두 차지함 (상위 컨테이너 너비 100%로 채우는 것)
      - <mark>대표적인 block 타입 태그 : `h1~6`, `p`, `div`</mark>
    
    - `inline`
      
      - 새로운 행으로 넘어가지 않음
      - width와 height 속성 사용 불가
      - 수직 방향 : padding, margin, border가 적용되지만, 다른 요소 밀어내기 불가능
      - 수평 방향 : padding, margin, borders가 적용되어, 다른 요소 밀어내기 가능
      - <mark>대표적인 inline 타입 태그 : `a`, `img`, `span`, `strong`, `em`</mark>
    
    - `inline-block`
      
      - `inline` 과 `block` 요소 사이의 중간 지점을 제공하는 display 값
      - width와 height 속성 사용 가능
      - padding, margin, border로 인해 다른 요소가 상자에서 밀려남
      - 새로운 행으로 넘어가지 않음
      - 요소가 줄바꿈되는 것을 원하지 않으면서 너비와 높이를 적용하고 싶은 경우에 사용
    
    - `none`
      
      - 요소를 화면에 표시하지도 않고, 공간을 부여하지도 않음

- ##### Inner display type
  
  : 박스 내부의 요소들이 어떻게 배치될지를 결정
  
  - <mark>`Flex`</mark>
    
    - 요소를 행과 열 형태로 배치하는 1차원 레이아웃 방식 -> 공간 배열 & 정렬
    
    - 구성 요소
      
      - main axis(주 축)
        
        - flex item들이 배치되는 기본 축
        - main start에서 시작해 main end 방향으로 배치(기본 값 상->하)
      
      - cross axis(교차 축)
        
        : main axis에 수직인 축
        
        - cross start에서 시작해 cross end 방향으로 배치(기본 값 좌->우)
      
      - Flex Container
        
        - `display: flex;` 혹은 `display: inline-flex` 가 설정된 부모 요소
        - 이 컨테이너의 1차 자식 요소들이 Flex Item이 됨
        - flexbox 속성 값들을 사용해 자식 요소 Flex Item들을 배치하는 주체
      
      - Flex Item
        
        - Flex Container 내부에 레이아웃 되는 항목
    
    - `Flexbox` 속성 목록
      
      - Flex Container 관련 속성
        
        - `display: flex;`
          
          - Flex Item은 기본적으로 행(주 축의 기본값인 가로 방향)으로 나열
          
          - Flex Item을 주 축의 시작 선에서 시작
          
          - Flex Item은 교차 축의 크기를 채우기 위해 늘어남
        
        - `flex-direction`
          
          - Flex Item이 나열되는 방향을 지정
          
          - `column`으로 지정할 경우 주 축이 변경됨
          
          - `-reverse`로 지정하면 Flex Item 배치의 시작 선과 끝 선이 서로 바뀜
        
        - <mark>`flex-wrap`</mark>
          
          - Flex Item 목록이 Flex Container의 한 행에 들어가지 않을 경우 다른 행에 배치할지 여부 설정
          
          - 화면 줄이면 너비 같이 줄어들지, 다음 줄로 넘어갈지
          
          - 응용 : 반응형 레이아웃
            
            : 다양한 디바이스와 화면 크기에 자동으로 정응하여 콘텐츠를 최적으로 표시하는 웹 레이아웃 방식
        
        - `justify-content`
          
          - 주 축을 따라 Flex Item과 주위에 공간을 분배
        
        - `align-items`
          
          - 교차 축을 따라 Flex Item과 주위에 공간을 분배
            
            - `flex-wrap`이 `wrap` 또는 `wrap-reverse`로 설정된 여러 행에만 적용됨
            
            - 한 줄 짜리 행에는 효과 없음(`flex-wrap: nowrap;` 으로 설정된 경우)
        
        - `align-content`
          
          - 교차 축을 따라 Flex Item 행을 정렬
      
      - Flex Item 관련 속성
        
        - `align-self`
          
          - 교차 축을 따라 개별 Flex Item을 정렬
        
        - `flex-grow`
          
          - 남는 행 여백을 **비율에 따라 각 Flex Item에 분배**
          
          - 아이템이 컨테이너 내에서 확장하는 비율을 지정
          
          - 1 1 2 에 분배를 한다고 2가 1의 2배를 가져가지 않음. 200px을 나눠준다고 했을 때, 200//4=50 이므로 각각 50 50 100을 나눠줌
          
          - `flex-grow`의 반대는 `flex-shrink`
        
        - `flex-basis`
          
          - Flex Item의 초기 크기 값을 지정
          
          - `flex-basis`와 `width`값을 동시에 적용한 경우 `flex-basis`가 우선
        
        - `order`
      
      - 목적에 따른 속성 분류
        
        - 배치 : `flex-direction`, `flex-wrap`
        
        - 공간 분배 : `justify-content`, `align-content`
        
        - 정렬 : `align-items`, `align-self`
      
      - 속성명 Tip
        
        - `justify` : 주 축. `content`
          
          - `items`와 `self`는 `margin auto`를 통해 정렬 및 배치가 가능해 필요 없기 때문
        
        - `align` : 교차 축. `content`, `items`, `self`

- Normal flow
  : 일반적인 흐름 또는 레이아웃을 변경하지 않은 경우 웹 페이지 요소가 배치되는 방식
  
  - 가로는 Inline Direction, 세로는 Block Direction

- #### Box 구성 요소
  
  - Content Box
    
    - 실제 콘텐츠가 표시되는 영역 크기
    
    - width 및 height 속성을 사용해 크기 조정
    
    - 너비 200px을 주면 content box가 200px. border은 더 큼
  
  - Padding box
    
    - 콘텐츠 주위에 공백
    
    - `padding` 관련 속성을 사용해 크기 조정
  
  - Border box
    
    - 콘텐츠와 패딩을 래핑
    
    - `border` 관련 속성을 사용해 크기 조정
  
  - Margin box
    
    - 콘텐츠, 패딩 및 테두리를 래핑
    
    - 박스와 다른 요소 사이의 공백
    
    - `margin` 관련 속성을 사용해 크기 조정
    
    - margin `auto` 로 하면 자동으로 거리 조정
  
  ![box구성의방향별속성값](https://github.com/user-attachments/assets/f24ae76b-f132-470c-ad37-b5d5b823ef35)

- Shorthand 속성
  
  - Border box
    
    - `border: 5px dotted black` 꼴로 작성 가능
  
  - Margin box & Padding box
    
    - 4방향 속성 각각 지정하지 않고 한번에 지정 가능
    - 4개 : 상/우/하/좌
    - 3개 : 상/좌우/하
    - 2개 : 상하/좌우
    - 1개 : 공통
  
  - Flex box
    
    - `flex-flow: flex-direction flex-wrap;`
      
      - 값 1개 : 단위X - `flex-grow`, 길이 혹은 퍼센트 - `flex-basis`
      
      - 값 2개 
        
        - 단위x 단위o - `flex-grow` `flex-basis`
        
        - 단위x 단위x - `flex-grow` `flex-shrink`
      
      - 값 3개 : `flex-grow` `flex-shrink` `flex-basis`

- 수평 정렬
  
  - `Block` 요소 : `margin: auto`` 사용
  
  - `Inline` 요소 : `text-align: center;`를 부모 요소에 적용
  
  - `Inlint-block` 요소 : `text-align: center;`를 부모 요소에 적용

- 대체 상자 모델로 변경
  
  - 넓이 지정하면 Content Box 넓이로 지정되어 실제 Margin box는 더 큼
  
  - <mark>크기 지정을 Border box로 하기 위해 `box-sizing: border-box;`  사용</mark>

- 마진 상쇄 (Margin Collapsing)
  
  : 두 block 타입 요소의 `margin top` 과 `bottom`이 만나 더 큰 margin으로 결합되는 현상
  
  - 이유
    
    - 복잡한 레이아웃에서 요소 간 간격을 일관되게 유지하기 위해 -> 일관성
    
    - 요소 간의 간격을 더 예측 가능하고 관리하기 쉽게 만듦 -> 단순화
  
  - ex) 20px + 20px -> 20px로 상쇄

![flex_direction](https://github.com/user-attachments/assets/01f69c63-ee66-44ca-9fc7-41a7af47e8c5)
![flex_wrap](https://github.com/user-attachments/assets/bebc1620-91f7-47c2-b318-b426c9f8aba5)
![justify_content](https://github.com/user-attachments/assets/21492ad5-6c32-437a-8224-dfe0c8b79620)
![align_content](https://github.com/user-attachments/assets/6b3ce799-6d5a-4a37-966f-09385cbc4fc9)
![align_items](https://github.com/user-attachments/assets/5665d21b-975a-4ef0-8859-13b055e81ba3)
![align_self](https://github.com/user-attachments/assets/06c2edd5-57dd-4f31-914f-969043a00198)

---

### 예시

- `flexbox`
  
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
  >       height: 500px;
  >       border: 1px solid black;
  >       display: flex;
  >       /* flex-direction: row; */
  >       /* flex-direction: column; */
  >       /* flex-direction: row-reverse; */
  >       /* flex-direction: column-reverse; */
  > 
  >       /* flex-wrap: nowrap; */
  >       /* flex-wrap: wrap; */
  > 
  >       /* justify-content: flex-start; */
  >       /* justify-content: center; */
  >       /* justify-content: flex-end; */
  > 
  >       /* align-content: flex-start;
  >       align-content: center;
  >       align-content: flex-end; */
  > 
  >       /* align-items: flex-start; */
  >       /* align-items: center; */
  >       /* align-items: flex-end; */
  >     }
  > 
  >     .post {
  >       background-color: grey;
  >       border: 1px solid black;
  >       margin: 0.5rem;
  >       padding: 0.5rem;
  >     }
  > 
  >     .item1 {
  >       align-self: center;
  >     }
  > 
  >     .item2 {
  >       align-self: flex-end;
  >     }
  >   </style>
  > </head>
  > 
  > <body>
  >   <div class="container">
  >     <div class="post item1">
  >       <h2>Post Title 1</h2>
  >       <p>Post Content 1</p>
  >     </div>
  >     <div class="post item2">
  >       <h2>Post Title 2</h2>
  >       <p>Post Content 2</p>
  >     </div>
  >     <div class="post item3">
  >       <h2>Post Title 3</h2>
  >       <p>Post Content 3</p>
  >     </div>
  >     <div class="post item4">
  >       <h2>Post Title 4</h2>
  >       <p>Post Content 4</p>
  >     </div>
  >   </div>
  > 
  > </body>
  > 
  > </html>
  > ```

- `flex-grow`
  
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
  >       display: flex;
  >       width: 100%;
  >     }
  > 
  >     .item {
  >       height: 100px;
  >       color: white;
  >       font-size: 3rem;
  >     }
  > 
  >     .item-1 {
  >       background-color: red;
  >       flex-grow: 2;
  >     }
  > 
  >     .item-2 {
  >       background-color: green;
  >       flex-grow: 2;
  >     }
  > 
  >     .item-3 {
  >       background-color: blue;
  >       flex-grow: 3;
  >     }
  >   </style>
  > </head>
  > 
  > <body>
  >   <div class="container">
  >     <div class="item item-1">1</div>
  >     <div class="item item-2">2</div>
  >     <div class="item item-3">3</div>
  >   </div>
  > </body>
  > 
  > </html>
  > ```

- `flex-basis`
  
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
  >       display: flex;
  >       width: 100%;
  >     }
  > 
  >     .item {
  >       height: 100px;
  >       color: white;
  >       font-size: 3rem;
  >     }
  > 
  >     .item-1 {
  >       background-color: red;
  >       flex-basis: 300px;
  >     }
  > 
  >     .item-2 {
  >       background-color: green;
  >       flex-basis: 600px;
  >     }
  > 
  >     .item-3 {
  >       background-color: blue;
  >       flex-basis: 300px;
  >     }
  >   </style>
  > </head>
  > 
  > <body>
  >   <div class="container">
  >     <div class="item item-1">1</div>
  >     <div class="item item-2">2</div>
  >     <div class="item item-3">3</div>
  >   </div>
  > </body>
  > 
  > </html>
  > ```

- 반응형 웹
  
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
  >       width: 80%;
  >       border: 1px solid black;
  >       /* 1 */
  >       display: flex;
  >       /* 2 */
  >       flex-wrap: wrap;
  >     }
  > 
  >     img {
  >       width: 100%;
  >     }
  > 
  >     .thumbnail {
  >       /* 3 */
  >       flex-basis: 700px;
  >       /* 4 */
  >       flex-grow: 1;
  >     }
  > 
  >     .content {
  >       /* 3 */
  >       flex-basis: 350px;
  >       /* 4 */
  >       flex-grow: 1;
  >     }
  >   </style>
  > </head>
  > 
  > <body>
  >   <div class="card">
  >     <img class="thumbnail" src="images/sample.jpg" alt="sample">
  >     <div class="content">
  >       <h2>Heading</h2>
  >       <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Perspiciatis minus sed expedita ut nihil tempora
  >         neque autem odio eos, repudiandae blanditiis, molestiae consequatur. Adipisci illo dolor repellat alias
  >         maiores.
  >         Aut?</p>
  >     </div>
  >   </div>
  > </body>
  > 
  > </html>
  > ```

- 레이아웃
  
  > ```html
  > <!DOCTYPE html>
  > <html lang="ko">
  > 
  > <head>
  >   <meta charset="UTF-8">
  >   <meta name="viewport" content="width=device-width, initial-scale=1.0">
  >   <title>Flexbox 레이아웃 예시</title>
  >   <style>
  >     body {
  >       /* display: flex; */
  >       /* 자식 요소들을 세로로 배치 */
  >       /* flex-direction: column; */
  >       /* 최소 높이를 뷰포트 높이로 설정하여 전체 화면 차지 */
  >       min-height: 100vh;
  >       margin: 0;
  >       font-family: Arial, sans-serif;
  >     }
  > 
  >     .box {
  >       background-color: #f0f0f0;
  >       border: 1px solid #000;
  >       padding: 20px;
  >       margin: 5px;
  >     }
  > 
  >     header {
  >       /* display: flex; */
  >       /* 로고와 네비게이션을 양쪽으로 정렬 */
  >       /* justify-content: space-between; */
  >       /* 세로 방향 중앙 정렬 */
  >       /* align-items: center; */
  >       padding: 10px 20px;
  >     }
  > 
  >     header h1 {
  >       margin: 0;
  >     }
  > 
  >     nav ul {
  >       list-style-type: none;
  >       padding: 0;
  >       margin: 0;
  >       /* 네비게이션 항목을 가로로 배열 */
  >       /* display: flex; */
  >     }
  > 
  >     nav ul li {
  >       margin-left: 20px;
  >     }
  > 
  >     nav ul li a {
  >       text-decoration: none;
  >       color: #333;
  >     }
  > 
  >     main {
  >       /* display: flex; */
  >       /* 남은 공간을 모두 차지하도록 설정 */
  >       /* flex-grow: 1; */
  >     }
  > 
  >     .sidebar {
  >       width: 200px;
  >       /* display: flex; */
  >       /* 내부 요소를 세로로 배치 */
  >       /* flex-direction: column; */
  >       /* 가로 방향 중앙 정렬 */
  >       /* align-items: center; */
  >     }
  > 
  >     .content {
  >       /* 남은 공간을 모두 차지하도록 설정 */
  >       /* flex-grow: 1; */
  >       /* display: flex; */
  >       /* 가로 & 세로 방향 중앙 정렬 */
  >       /* justify-content: center; */
  >       /* align-items: center; */
  >     }
  > 
  >     footer {
  >       /* display: flex; */
  >       /* 가로 & 세로 방향 중앙 정렬 */
  >       /* justify-content: center; */
  >       /* align-items: center; */
  >     }
  >   </style>
  > </head>
  > 
  > <body>
  >   <header class="box">
  >     <h1>웹사이트 제목</h1>
  >     <nav>
  >       <ul>
  >         <li><a href="#home">홈</a></li>
  >         <li><a href="#about">소개</a></li>
  >         <li><a href="#services">서비스</a></li>
  >         <li><a href="#contact">연락처</a></li>
  >       </ul>
  >     </nav>
  >   </header>
  >   <main>
  >     <aside class="sidebar box">
  >       <h2>사이드바</h2>
  >       <ul>
  >         <li>메뉴 1</li>
  >         <li>메뉴 2</li>
  >         <li>메뉴 3</li>
  >       </ul>
  >     </aside>
  >     <section class="content box">
  >       <h2>메인 콘텐츠</h2>
  >     </section>
  >   </main>
  >   <footer class="box">
  >     <p>푸터 - 저작권 정보 등</p>
  >   </footer>
  > </body>
  > 
  > </html>
  > ```
