# Bootstrap

: CSS 프론트엔드 프레임워크(Toolkit)

: 미리 만들어진 다양한 디자인 요소들을 제공하여 웹 사이트를 빠르고 쉽게 개발할 수 있도록 함

- 사용 방법
  
  1. [Bootstrap](https://getbootstrap.com/) 공식 문서 접속 (영어로 들어갈 것)
  
  2. `Docs` -> `Introduction` -> `Quick start`
  
  3. [Include Bootstrap's CSS and JS](https://getbootstrap.com/docs/5.3/getting-started/introduction/#quick-start) 코드 확인 및 가져오기
     
     - head와 body에 bootstrap CDN이 포함된 코드 블록

- CDN Content Delivery Network
  
  : 지리적 제약 없이 빠르고 안전하게 콘텐츠를 전송할 수 있는 전송 기술
  
  - 서버와 사용자 사이의 물리적인 거리를 줄여 콘텐츠 로딩에 소요되는 시간을 최소화(웹 페이지 로드 속도 높임)
  
  - 지리적으로 사용자와 가까운 CDN 서버에 콘텐츠를 저장해 사용자에게 전달
  
  - Bootstrap CDN
    
    1. Bootstrap 홈페이지 - `Download` - `Compiled CSS and js` 다운로드
    
    2. CDN을 통해 가져오는 bootstrap css 와 js 파일 확인
    
    3. `bootstrap.css`, `bootstrap.js` 파일 참고
       
       -> 온라인 CDN 서버에 업로드 된 css 및 js 파일을 불러와서 사용하는 것

- #### 기본 사용법
  
  : 모든 스타일을 클래스 선택자로 만들어 놓았기 때문에 클래스 선택자로 사용
  
  - <mark>`{property}{sides}-{size}`</mark>
    
    - property
      
      | 기호  | 의미      |
      | --- | ------- |
      | `m` | margin  |
      | `p` | padding |
    
    - sides
      
      | 기호      | 의미          |
      | ------- | ----------- |
      | `t`     | top         |
      | `b`     | bottom      |
      | `s`     | left(start) |
      | `e`     | right (end) |
      | `y`     | top, bottom |
      | `x`     | left, right |
      | `blank` | 4 sides     |
    
    - size
      
      - `rem` 중요! 반응형 웹의 포인트. 기준 사이즈에 따라 달라짐
      
      | 기호     | 의미-rem   | 의미-px |
      | ------ | -------- | ----- |
      | `0`    | 0 rem    | 0 px  |
      | `1`    | 0.25 rem | 4 px  |
      | `2`    | 0.5 rem  | 8 px  |
      | `3`    | 1 rem    | 16 px |
      | `4`    | 1.5 rem  | 24 px |
      | `5`    | 3 rem    | 48 px |
      | `auto` | auto     | auto  |
  
  - [Bootstrap에서 클래스 이름으로 Spacing을 표현하는 방법](https://getbootstrap.com/docs/5.3/utilites/spacing/#margin-and-padding)
  
  - - ex) `<p class="mt-5">Hello,world!</p>`
      
      - `mt-5` : margin top 5

---

## Reset CSS

: 모든 HTML 요소 스타일을 일관된 기준으로 재설정하는 간결하고 압축된 규칙 세트

- HTML Element, Table, List 등의 요소들에 일관성 있게 스타일을 적용시키는 기본 단계

- 사용 배경
  
  - 모든 브라우저는 웹사이트를 보다 읽기 편하게 하기 위해 각자의 `user agent stylesheet`를 가지고 있음
  
  - 문제는 이 설정이 브라우저마다 상이해 모든 브라우저에서 웹사이트를 동일하게 보이게 만들어야 하는 개발자에게는 매우 골치아픔
  
  - 모두 똑같은 스타일 상태로 만들고 스타일 개발을 시작하기 위해 사용

- **Normalize CSS**
  
  - Reset CSS 방법 중 대표적인 방법으로 웹 표준 기준으로 브라우저 중 하나가 불일치한다면 차이가 있는 브라우저를 수정하는 방법
  
  - 경우에 따라 IE 또는 EDGE 브라우저는 표준에 따라 수정할 수 없는 경우도 있는데, 이 경우 IE 또는 EDGE의 스타일을 나머지 브라우저에 적용

- Bootstrap에서의 Reset CSS
  
  - `bootstrap-reboot.css`라는 파일명으로 `normalize.css`를 자체적으로 커스텀해서 사용중

---

## Bootstrap 활용

- #### Typography
  
   : 제목, 본문 텍스트, 목록 등
  
  - Display Headinigs : 기존 Heading보다 더 눈에 띄는 제목이 필요할 경우(더 크고 약간 다른 스타일)
  
  - Inline text elements : HTML inline 요소에 대한 스타일
  
  - Lists : HTML list 요소에 대한 스타일
  
  > ```html
  > <!doctype html>
  > <html lang="en">
  >   <head>
  >     <meta charset="utf-8">
  >     <meta name="viewport" content="width=device-width, initial-scale=1">
  >     <title>Bootstrap demo</title>
  >     <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
  >   </head>
  >   <body>
  >     <h1>Hello, world!</h1>
  > 
  >     <!-- display heading -->
  >     <h1 class="display-1">Display 1</h1>
  >     <h1 class="display-2">Display 2</h1>
  >     <h1 class="display-3">Display 3</h1>
  >     <h1 class="display-4">Display 4</h1>
  >     <h1 class="display-5">Display 5</h1>
  >     <h1 class="display-6">Display 6</h1>
  > 
  >     <p>You can use the mark tag to <mark>highlight</mark> text.</p>
  >     <p><del>This line of text is meant to be treated as deleted text.</del></p>
  >     <p><s>This line of text is meant to be treated as no longer accurate.</s></p>
  >     <p><ins>This line of text is meant to be treated as an addition to the document.</ins></p>
  >     <p><u>This line of text will render as underlined.</u></p>
  >     <p><small>This line of text is meant to be treated as fine print.</small></p>
  >     <p><strong>This line rendered as bold text.</strong></p>
  >     <p><em>This line rendered as italicized text.</em></p>
  > 
  >     <ul>
  >       <li>This is a list.</li>
  >       <li>It appears completely unstyled.</li>
  >       <li>Structurally, it's still a list.</li>
  >       <li>However, this style only applies to immediate child elements.</li>
  >       <li>Nested lists:
  >         <ul>
  >           <li>are unaffected by this style</li>
  >           <li>will still show a bullet</li>
  >           <li>and have appropriate left margin</li>
  >         </ul>
  >       </li>
  >       <li>This may still come in handy in some situations.</li>
  >     </ul>
  > 
  > 
  >     <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
  >   </body>
  > </html>
  > ```

- #### Bootstrap Color System
  
  : Bootstrap이 지정하고 제공하는 색상 시스템
  
  - Colors : Text, Border, Background 및 다양한 요소에 사용하는 Bootstrap의 색상 키워드
  
  > ```html
  > <!DOCTYPE html>
  > <html lang="en">
  > 
  > <head>
  >   <meta charset="UTF-8">
  >   <meta name="viewport" content="width=device-width, initial-scale=1.0">
  >   <title>Document</title>
  >   <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
  >     integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
  > </head>
  > ```
  
  - Text colors
  
  > ```html
  >   <!-- text colors -->
  >   <p class="text-primary">.text-primary</p>
  >   <p class="text-primary-emphasis">.text-primary-emphasis</p>
  >   <p class="text-secondary">.text-secondary</p>
  >   <p class="text-secondary-emphasis">.text-secondary-emphasis</p>
  >   <p class="text-success">.text-success</p>
  >   <p class="text-success-emphasis">.text-success-emphasis</p>
  >   <p class="text-danger">.text-danger</p>
  >   <p class="text-danger-emphasis">.text-danger-emphasis</p>
  >   <p class="text-warning bg-dark">.text-warning</p>
  >   <p class="text-warning-emphasis">.text-warning-emphasis</p>
  >   <p class="text-info bg-dark">.text-info</p>
  >   <p class="text-info-emphasis">.text-info-emphasis</p>
  >   <p class="text-light bg-dark">.text-light</p>
  >   <p class="text-light-emphasis">.text-light-emphasis</p>
  >   <p class="text-dark bg-white">.text-dark</p>
  >   <p class="text-dark-emphasis">.text-dark-emphasis</p>
  > ```
  
  - Background colors
  
  > ```html
  >   <!-- background colors -->
  >   <div class="p-3 mb-2 bg-primary text-white">.bg-primary</div>
  >   <div class="p-3 mb-2 bg-primary-subtle text-primary-emphasis">.bg-primary-subtle</div>
  >   <div class="p-3 mb-2 bg-secondary text-white">.bg-secondary</div>
  >   <div class="p-3 mb-2 bg-secondary-subtle text-secondary-emphasis">.bg-secondary-subtle</div>
  >   <div class="p-3 mb-2 bg-success text-white">.bg-success</div>
  >   <div class="p-3 mb-2 bg-success-subtle text-success-emphasis">.bg-success-subtle</div>
  >   <div class="p-3 mb-2 bg-danger text-white">.bg-danger</div>
  >   <div class="p-3 mb-2 bg-danger-subtle text-danger-emphasis">.bg-danger-subtle</div>
  >   <div class="p-3 mb-2 bg-warning text-dark">.bg-warning</div>
  >   <div class="p-3 mb-2 bg-warning-subtle text-warning-emphasis">.bg-warning-subtle</div>
  >   <div class="p-3 mb-2 bg-info text-dark">.bg-info</div>
  >   <div class="p-3 mb-2 bg-info-subtle text-info-emphasis">.bg-info-subtle</div>
  >   <div class="p-3 mb-2 bg-light text-dark">.bg-light</div>
  >   <div class="p-3 mb-2 bg-light-subtle text-light-emphasis">.bg-light-subtle</div>
  >   <div class="p-3 mb-2 bg-dark text-white">.bg-dark</div>
  >   <div class="p-3 mb-2 bg-dark-subtle text-dark-emphasis">.bg-dark-subtle</div>
  >   <div class="p-3 mb-2 bg-body-secondary">.bg-body-secondary</div>
  >   <div class="p-3 mb-2 bg-body-tertiary">.bg-body-tertiary</div>
  >   <div class="p-3 mb-2 bg-body text-body">.bg-body</div>
  >   <div class="p-3 mb-2 bg-black text-white">.bg-black</div>
  >   <div class="p-3 mb-2 bg-white text-dark">.bg-white</div>
  >   <div class="p-3 mb-2 bg-transparent text-body">.bg-transparent</div>
  > 
  > 
  >   <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
  >   integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
  >   crossorigin="anonymous"></script>
  > ```

- #### Bootstrap Component
  
  : Bootstrap에서 제공하는 UI 관련 요소. 버튼, 네비게이션 바, 카드, 폼, 드롭다운 등
  
  - 일관된 디자인을 제공해 웹 사이트의 구성 요소를 구축하는데 유용하게 활용
  
  > ```html
  > <head>
  >   <meta charset="UTF-8">
  >   <meta name="viewport" content="width=device-width, initial-scale=1.0">
  >   <title>Document</title>
  >   <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
  >     integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
  > </head>
  > ```
  
  - Alerts
  
  > ```html
  > <!-- alerts -->
  >   <div class="alert alert-primary" role="alert">
  >     A simple primary alert—check it out!
  >   </div>
  >   <div class="alert alert-secondary" role="alert">
  >     A simple secondary alert—check it out!
  >   </div>
  >   <div class="alert alert-success" role="alert">
  >     A simple success alert—check it out!
  >   </div>
  >   <div class="alert alert-danger" role="alert">
  >     A simple danger alert—check it out!
  >   </div>
  >   <div class="alert alert-warning" role="alert">
  >     A simple warning alert—check it out!
  >   </div>
  >   <div class="alert alert-info" role="alert">
  >     A simple info alert—check it out!
  >   </div>
  >   <div class="alert alert-light" role="alert">
  >     A simple light alert—check it out!
  >   </div>
  >   <div class="alert alert-dark" role="alert">
  >     A simple dark alert—check it out!
  >   </div>
  > ```
  
  - Badges
  
  > ```html
  > <!-- badges -->
  >   <span class="badge text-bg-primary">Primary</span>
  >   <span class="badge text-bg-secondary">Secondary</span>
  >   <span class="badge text-bg-success">Success</span>
  >   <span class="badge text-bg-danger">Danger</span>
  >   <span class="badge text-bg-warning">Warning</span>
  >   <span class="badge text-bg-info">Info</span>
  >   <span class="badge text-bg-light">Light</span>
  >   <span class="badge text-bg-dark">Dark</span>
  > ```
  
  - Buttons
  
  > ```html
  > <!-- Buttons -->
  >   <button type="button" class="btn btn-primary">Primary</button>
  >   <button type="button" class="btn btn-secondary">Secondary</button>
  >   <button type="button" class="btn btn-success">Success</button>
  >   <button type="button" class="btn btn-danger">Danger</button>
  >   <button type="button" class="btn btn-warning">Warning</button>
  >   <button type="button" class="btn btn-info">Info</button>
  >   <button type="button" class="btn btn-light">Light</button>
  >   <button type="button" class="btn btn-dark">Dark</button>
  > 
  >   <button type="button" class="btn btn-link">Link</button>
  > ```
  
  - Cards
  
  > ```html
  >  <!-- Cards -->
  >    <div class="card" style="width: 18rem;">
  >     <img src="..." class="card-img-top" alt="...">
  >     <div class="card-body">
  >       <h5 class="card-title">Card title</h5>
  >       <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
  >       <a href="#" class="btn btn-primary">Go somewhere</a>
  >     </div>
  >   </div>
  > ```
  
  - Navbar
  
  > ```html
  > <!-- navbar -->
  >   <nav class="navbar navbar-expand-lg bg-body-tertiary">
  >     <div class="container-fluid">
  >       <a class="navbar-brand" href="#">Navbar</a>
  >       <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
  >         <span class="navbar-toggler-icon"></span>
  >       </button>
  >       <div class="collapse navbar-collapse" id="navbarSupportedContent">
  >         <ul class="navbar-nav me-auto mb-2 mb-lg-0">
  >           <li class="nav-item">
  >             <a class="nav-link active" aria-current="page" href="#">Home</a>
  >           </li>
  >           <li class="nav-item">
  >             <a class="nav-link" href="#">Link</a>
  >           </li>
  >           <li class="nav-item dropdown">
  >             <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
  >               Dropdown
  >             </a>
  >             <ul class="dropdown-menu">
  >               <li><a class="dropdown-item" href="#">Action</a></li>
  >               <li><a class="dropdown-item" href="#">Another action</a></li>
  >               <li><hr class="dropdown-divider"></li>
  >               <li><a class="dropdown-item" href="#">Something else here</a></li>
  >             </ul>
  >           </li>
  >           <li class="nav-item">
  >             <a class="nav-link disabled" aria-disabled="true">Disabled</a>
  >           </li>
  >         </ul>
  >         <form class="d-flex" role="search">
  >           <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
  >           <button class="btn btn-outline-success" type="submit">Search</button>
  >         </form>
  >       </div>
  >     </div>
  >   </nav>
  > ```
  
  - Carousel
    
    - id로 구분. 여러개 쓸 때 주의할 것
    
    - `id` 와 `data-bs-target`이 올바르게 매치되어 있는지 확인 필수
    
    > ```html
    > <!DOCTYPE html>
    > <html lang="en">
    > 
    > <head>
    >   <meta charset="UTF-8">
    >   <meta name="viewport" content="width=device-width, initial-scale=1.0">
    >   <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet"
    >     integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
    > </head>
    > 
    > <body>
    >   <div class="container">
    >     <!-- carousel 1번 -->
    >     <!-- carousel id 속성 값과 각 버튼의 data-bs-target 속성 값이 각각 올바르게 일치하는지 확인 -->
    >     <div id="carouselExample" class="carousel slide">
    >       <div class="carousel-inner">
    >         <div class="carousel-item active">
    >           <img src="images/01.jpg" class="d-block w-100" alt="...">
    >         </div>
    >         <div class="carousel-item">
    >           <img src="images/02.jpg" class="d-block w-100" alt="...">
    >         </div>
    >         <div class="carousel-item">
    >           <img src="images/03.jpg" class="d-block w-100" alt="...">
    >         </div>
    >       </div>
    >       <button class="carousel-control-prev" type="button" data-bs-target="#carouselExample" data-bs-slide="prev">
    >         <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    >         <span class="visually-hidden">Previous</span>
    >       </button>
    >       <button class="carousel-control-next" type="button" data-bs-target="#carouselExample" data-bs-slide="next">
    >         <span class="carousel-control-next-icon" aria-hidden="true"></span>
    >         <span class="visually-hidden">Next</span>
    >       </button>
    >     </div>
    > 
    >     <!-- carousel 2번 -->
    >     <div id="carouselExample2" class="carousel slide">
    >       <div class="carousel-inner">
    >         <div class="carousel-item active">
    >           <img src="images/04.jpg" class="d-block w-100" alt="...">
    >         </div>
    >         <div class="carousel-item">
    >           <img src="images/05.jpg" class="d-block w-100" alt="...">
    >         </div>
    >         <div class="carousel-item">
    >           <img src="images/06.jpg" class="d-block w-100" alt="...">
    >         </div>
    >       </div>
    >       <button class="carousel-control-prev" type="button" data-bs-target="#carouselExample2" data-bs-slide="prev">
    >         <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    >         <span class="visually-hidden">Previous</span>
    >       </button>
    >       <button class="carousel-control-next" type="button" data-bs-target="#carouselExample2" data-bs-slide="next">
    >         <span class="carousel-control-next-icon" aria-hidden="true"></span>
    >         <span class="visually-hidden">Next</span>
    >       </button>
    >     </div>
    > 
    >   </div>
    > 
    >   <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
    >     integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
    >     crossorigin="anonymous"></script>
    > </body>
    > 
    > </html>
    > ```
  
  - Modal
    
    - Button과 Modal은 별개로 적혀 있음
    
    - `id`와 `data-bs-target`이 올바르게 매치되어 있는지 확인 필수
    
    - Button은 상단에 있어도 되지만, Modal은 가장 하단에 모아두기. 가려져 보이지 않는 경우 예방 가능
    
    > ```html
    > 
    > ```

---

## Bootstrap Grid System

: 웹 페이지의 레이아웃을 조정하는데 사용되는 12개의 컬럼으로 구성된 시스템

- 목적
  
  : 반응형 디자인을 지원해 웹 페이지를 모바일, 태블릿, 데스크탑 등 다양한 기기에서 적절하게 표시할 수 있도록 도움

- Grid System 기본 요소
  
  - `Container` : Column들을 담고 있는 공간
  
  - `Column` : 실제 컨텐츠를 포함하는 부분
  
  - `Gutter` 
    
    : Column과 Column 사이의 여백 영역
    
    - x축은 `padding`, y축은 `margin`으로 여백 생성
  
  - 1개의 row 안에 12개의 column영역이 구성
  
  - 각 요소는 12개 중 몇 개를 차지할 것인지 지정됨

---

## 반응형 웹 디자인 Responsive Web Design

: 디바이스 종류나 화면 크기에 상관 없이 어디서든 일관된 레이아웃 및 사용자 경험을 제공하는 디자인 기술

- Bootstrap grid system에서는 12개의 `column`과 6개의 `breakpoints`를 사용해 반응형 웹 디자인을 구현

- #### Grid system breakpoints
  
  : 웹 페이지를 다양한 화면 크기에서 적절하게 배치하기 위한 분기점
  
  - 화면 너비에 따라 6개의 분기점 제공 (`xs`, `xm`, `md`, `lg`, `xl`, `xxl`)
  
  - 각 `breakpoints`마다 설정된 최대 너비 값 **이상으로** 화면이 커지면 grid system 동작이 변경됨
    
    ![](C:\Users\user\AppData\Roaming\marktext\images\2024-08-23-01-12-17-image.png)

---

### cf. Bootstrap을 사용하는 이유

- 가장 많이 사용되는 CSS 프레임워크

- 사전에 디자인된 다양한 컴포넌트 및 기능을 사용해 빠른 개발과 유지보수 가능

- 손쉬운 반응형 웹 디자인 구현

- 커스터마이징 용이

- 크로스 브라우징 지원 - 모든 브라우저에서 작동하도록 설계되어 있음

#### cf. CDN 없이 사용하기

- Bootstrap 코드 파일을 다운받아 활용
  
  1. [Bootstrap 코드 파일 다운로드](https://getbootstrap.com/docs/5.3/getting-started/download/)
  
  2. `bootstrap.css`와 `bootstrap.bundle.js`만 선택
  
  3. CSS 파일은 HTML `head` 태그에 가져와 사용
  
  4. JS 파일은 HTML `body` 태그에 가져와서 사용
  
  5. 파일 별 포함된 기능이 다르므로 [공식문서](https://getbootstrap.com/docs/5.3/getting-started/contents/) 를 통해 확인
  
  6. 

#### cf. 의미론적인 마크업이 필요한 이유

- 검색엔진 최적화 SEO
  
  - 검색 엔진이 해당 웹 사이트를 분석하기 쉽게 만들어 검색 순위에 영향을 줌

- 웹 접근성 Web Accessibility
  
  - 웹 사이트, 도구, 기술이 고령자나 장애를 가진 사용자들이 사용할 수 있도록 설계 및 개발하는 것
  
  - ex) 스크린 리더를 통해 전맹 시각장애 사용자에게 웹의 글씨를 읽어줌
  
  - [nuli](https://nuli.navercorp.com/) 에서 체험 가능

#### cf. The Grid System

: CSS가 아닌 편집 디자인에서 나온 개념으로, 구성 요소를 잘 배치해서 시각적으로 좋은 결과물을 만들기 위함

- 기본적으로 안쪽에 있는 요소들의 오와 열을 맞추는 것에서 기인

- 정보 구조와 배열을 체계적으로 작성해 정보의 질서를 부여하는 시스템

#### cf. Grid cards

- `row-cols` 클래스를 사용해 행당 표시할 열(카드) 수를 손쉽게 제어할 수 있음

- [참고 bootstrap 사이트](https://getbootstrap.com/docs/5.3/components/card/#grid-cards)

#### cf. Emmet

: HTML 및 CSS 등의 다양한 파일 종류에 유용한 코드 단축키 제공

- VS Code에 내장

- [내용 참고]([Cheat Sheet](https://docs.emmet.io/cheat-sheet/)[Cheat Sheet](https://docs.emmet.io/cheat-sheet/))
