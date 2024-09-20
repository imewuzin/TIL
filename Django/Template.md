# Django Template  system

: 데이터 표현을 제어하면서, 표현과 관련된 부분을 담당

---

## Django Template Language (DTL)

: Template에서 조건, 반복, 변수 등의 프로그래밍적 기능을 제공하는 시스템

- #### 변수 Variable
  
  - `render` 함수의 3번째 인자로 딕셔너리 데이터를 사용
  
  - 딕셔너리 `key`에 해당하는 문자열이 template에서 사용 가능한 변수명이 됨
  
  - dot `.`를  사용하여 변수 속성에 접근할 수 있음
  
  - ex) `{{ variable }}`, `{{ variable.attribute }}`

- #### 필터 Filters
  
  - 표시할 변수를 수정할 때 사용 (`변수|필터`)
  
  - chained(연결)이 가능하며 일부 필터는 인자를 받기도 함
  
  - 약 60개의 built-in template filters 제공
  
  - ex) `{{ variable|filter }}`, `{{ name|truncatewords:30 }}`

- #### 태그 Tags
  
  - 반복 또는 논리를 수행해 제어 흐름 만듦
  
  - 일부 태그는 시작과 종료 태그가 필요
  
  - 약 24개의 built-in template tags 제공
  
  - ex) `{% tag %}`, `{%if } {% endif% }`

- #### 주석 Comments
  
  - `{# name #}`, `{% comment %} .. {% endcomment %}`

---

## 템플릿 상속 Template inheritance

: 페이지의 공통요소를 포함하고, 하위 템플릿이 재정의 할 수 있는 공간을 정의하는 기본 'skeleton' 템플릿을 작성하여 상속 구조를 구축

- 상속 구조 만들기
  
  1. skeleton 역할을 하게 되는 상위 템플릿 `base.html` 작성
  
  2. 기존 하위 템플릿 변화
     
     - ex) `{% extends 'articles/base.html %} {% block content %} .. {% endblock content %}`

- 상속 관련 DTL 태그
  
  - `{% extends 'path' %}`
    
    : 자식(하위) 템플릿이 부모 템플릿을 확장한다는 것을 알림
    
    - 반드시 자식 템플릿 최상단에 작성되어야 함(2개 이상 사용 불가)
  
  - `{% block name %} {% endblock name %}`
    
    : 하위 템플릿에서 재정의 할 수 있는 블록의 정의
    
    - 상위 템플릿에 작성하며 하위 템플릿이 작성할 수 있는 공간을 지정하는 것
