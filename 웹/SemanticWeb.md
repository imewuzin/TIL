# Semantic Web

: 웹 데이터를 의미론적으로 구조화된 형태로 표현하는 방식

: 이 요소가 시각적으로 어떻게 보여질까? -> 이 요소가 가진 목적과 역할은 무엇일까?

## HTML Semantic Web

: 검색엔진 및 개발자가 웹 페이지 콘텐츠를 이해하기 쉽도록 기본적인 모양과 기능 이외에 의미를 가지는 HTML 요소

- 대표적인 Semantic Element
  
  - `header`, `nav`, `main`, `article`, `section`, `aside`, `footer` == `div`

## Semantic in CSS

- CSS 방법론
  
  : CSS를 효율적이고 유지 보수가 용이하게 작성하기 위한 일련의 가이드라인

- OOCSS (Object Oriented CSS)
  
  : 객체 지향적 접근법을 적용하여 CSS를 구성하는 방법론
  
  - 기본 원칙
    
    - 구조와 스킨을 분리 -> 재사용 가능성 높임
      
      - 모든 버튼의 공통 구조를 정의 + 각각의 스킨(배경색과 폰트 색상) 정의
      
      > ```css
      > .button {
      >     border: none;
      >     font-size: 1em;
      >     padding: 10px 20px;
      > }
      > 
      > .button-blue {
      >     background-color: blue;
      >     color: white;
      > }
      > 
      > .button-red {
      >     background-color: red;
      >     color: while;
      > }
      > ```
    
    - 컨테이너와 콘텐츠를 분리
      
      - 객체에 직접 적용하는 대신 객체를 둘러싸는 컨테이너에 스타일을 적용
      
      - 스타일을 정의할 때 위치에 의존적인 스타일을 사용하지 않도록 함
      
      - 콘텐츠를 다른 컨테이너로 이동시키거나 재배치할 때 스타일이 깨지는 것 방지
      
      > - bad
      > 
      > ```css
      > .header h2 {
      >     font-size: 24px;
      >     color: white;
      > }
      > 
      > .footer h2 {
      >     font-size: 24px;
      >     color: black;
      > }
      > ```
      > 
      > - good
      > 
      > ```css
      > .container .title {
      >     font-size: 24px;
      > }
      > 
      > .header {
      >     color: white;
      > }
      > 
      > .footer {
      >     color: black;
      > }
      > ```
