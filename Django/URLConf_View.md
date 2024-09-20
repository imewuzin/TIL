## 웹사이트 개발 개요

### 웹사이트 개발 개요

- **웹 브라우저 역할**: 클라이언트 역할을 함.
- **요청 및 응답 과정**:
  - 클라이언트가 요청을 네트워크를 통해 서버에 전달.
  - 서버는 요청 정보를 처리하고 결과를 클라이언트에 반환.
  - 결과는 보통 HTML 형태의 문서 또는 일반 문자열.

### 정적 vs 동적 서비스

- **정적 서비스**:
  
  - HTML 파일, CSS, 자바스크립트, 이미지 등 정적 파일로 구성.
  - 정적인 결과를 반환하는 방식.

- **동적 서비스**:
  
  - 클라이언트의 요청에 따라 다양한 응답 결과가 제공됨.
  - Django는 동적인 결과를 생성하기 위해 사용됨.

---

### Django 환경 설정

1. **작업 폴더 준비**:
   
   - 명령 프롬프트에서 작업 폴더로 이동.
   - `django-admin startproject mysite` 명령으로 프로젝트 생성.
   - 생성된 폴더 이름을 `myproject01`으로 변경.

2. **기본 설정**:
   
   - `manage.py`와 `mysite` 폴더가 있는지 확인.
   - 비주얼 스튜디오 코드로 열기.
   - 기본 설정 진행:
     - 디버그 모드를 `True`로 설정 (실제 서비스 시에는 `False`로 변경).
     - 외부 접속 설정.
     - 템플릿, 데이터베이스 설정, 관리자 사이트 언어 코드(`ko-kr`), 타임존(`Asia/Seoul`) 설정.

3. **터미널 설정**:
   
   - 명령 프롬프트에서 서버 실행 (`python manage.py runserver`).
   - VS code 내 Terminal에서 실행 가능

4. **초기 설정 완료**:
   
   - `python manage.py migrate` 명령으로 데이터베이스 마이그레이션.
   - `python manage.py createsuperuser` 명령으로 관리자 계정 생성.
   - 관리자 사이트에서 로그인 및 모델 관리 가능.

### 앱 생성 및 설정

1. **앱 생성**:
   
   - `python manage.py startapp ex_view` 명령으로 앱 생성.
   - 생성된 `ex_view` 폴더에서 작업.

2. **URL 설정**:
   
   - `ex_view/urls.py` 파일 생성.
   
   - URL 패턴을 설정하여 요청 경로와 뷰 함수 매핑.
     
     ```python
     from django.urls import path
     from . import views
     
     urlpatterns = [
         path('', views.index)
     ]
     ```
   
   - `views.py`에서 `index` 함수 정의.
     
     ```python
     from django.http imort HttpResponse
     from django.shortcuts import render
     
     def index(request):
         print('클라이언트로부터 요청을 받음')
         return HttpResponse('응답 데이터')
     ```
     
     =>  클라이언트로부터 요청이 들어오면 `urls.py`와 `views.py`에 있는 내용으로 해결하겠다. `/`문자열로 들어왔을 때, `index` 함수가 반응해 응답하겠다.

3. **URL 포함**:
   
   - `mysite/urls.py` 파일에 `exview.urls` 포함.
     
     => **각 앱이 필요로 하는 URL 설정을 각 앱이 다 가지도록 설정. 대표 앱에서 `include`를 이용해 불러와 사용**
     
     ```python
     from django.contrib import admin
     from django.urls import path, include
     
     urlpatterns = [
         path('admin/', admin.site.urls),
         path('ex_view/', include('ex_view.urls')),  # 기본 페이지 더 이상 그려지지 않
     ]
     ```

### 테스트

1. **서버 실행**:
   
   - 브라우저에서 `http://localhost:8000/exview/` 주소로 접속.

2. **결과 확인**:
   
   - `index` 함수가 호출되며, 클라이언트의 요청을 처리하고 "클라이언트로부터 요청을 받음"이라는 문자열이 응답으로 반환됨.

3. **요청 및 응답 확인**:
   
   - 서버에 요청을 보내고 응답 데이터를 브라우저에서 확인.

### 요약

- URL 요청에 따라 Django는 요청 정보를 해석하고, 해당하는 뷰 함수를 호출하여 응답을 반환.

- 각 앱은 독립적으로 URL 설정을 가지고 있으며, 프로젝트의 메인 URL 설정 파일에서 이를 포함하여 관리.

- **요청과 응답의 기본 개념**:
  
  - **요청(Request)**: 클라이언트(브라우저 등)가 서버에 보내는 데이터. 요청에는 헤더(Header)와 바디(Body)가 포함될 수 있습니다.
  - **응답(Response)**: 서버가 클라이언트의 요청에 대해 보내는 데이터. 응답도 헤더와 바디를 포함합니다.

- **헤더와 바디**:
  
  - **헤더**: 요청 또는 응답의 메타데이터를 포함합니다. 예를 들어, 요청의 URL, 요청 메서드(GET, POST 등), 응답 상태 코드(200 OK 등) 등이 있습니다.
  - **바디**: 요청 또는 응답의 실제 데이터가 포함됩니다. 예를 들어, 폼 제출 시 입력된 데이터나 서버에서 클라이언트로 보내는 HTML, JSON 데이터 등이 이에 해당합니다.

- **웹 브라우저에서의 개발자 도구**:
  
  - 개발자 도구의 네트워크 탭을 사용하면 요청과 응답의 세부 사항을 확인할 수 있습니다. 이를 통해 헤더와 바디, 요청 URL, 응답 데이터 등을 볼 수 있습니다.

- **HTTP 메서드**:
  
  - **GET**: 서버에서 데이터를 가져오는 요청.
  - **POST**: 서버에 데이터를 보내는 요청.

- **요청 파라미터와 바디**:
  
  - **요청 파라미터**: URL에 포함된 데이터(쿼리 스트링 등)로, GET 요청에서 주로 사용됩니다.
  - **바디**: POST 요청 등에서 서버로 보내는 데이터가 포함됩니다. 예를 들어, 회원가입이나 로그인 시 입력된 데이터가 바디에 포함됩니다.

- **세션**:
  
  - 클라이언트를 구분하기 위해 서버에서 관리하는 데이터. 쿠키를 통해 클라이언트에 저장되며, 세션을 통해 사용자의 상태를 유지합니다.

- **템플릿**:
  
  - 장고에서는 템플릿을 사용하여 응답 데이터를 동적으로 생성할 수 있습니다. 템플릿을 사용하면 HTML 파일에서 데이터 바인딩 및 동적 콘텐츠 생성을 쉽게 할 수 있습니다.

- **템플릿 폴더 구조**:
  
  - 장고 프로젝트에서 각 앱의 템플릿 폴더를 설정하고, 앱 별로 템플릿 파일을 관리하는 것이 권장됩니다. 이렇게 하면 템플릿 파일의 충돌을 방지하고 관리하기 쉬워집니다.

---

## Variable Routing

: URL 일부에 변수를 포함시키는 것 (변수는 view 함수의 인자로 전달할 수 있음)

- `<pah_converter:variable_name>`
  
  - ex) `path('articles/<int:num>/', views.detail)`

## Path converters

: URL 변수의 타입을 지정 (str, int 등 5가지 타입 지원)

---

## App URL mapping

: 각 앱에 URL을 정의하는 것

- 프로젝트와 각 앱이 URL을 나누어 관리를 편하게 하기 위함

- 프로젝트의 `urls.py` 에는 `include` 함수 이용해서 각 앱으로 보내고, 각 앱의 `urls.py`에서 경로 지정

- <mark>`include()`</mark>
  
  : 프로젝트 내부 앱들의 URL을 참조할 수 있도로 ㄱ매핑하는 함수
  
  - URL의 일치하는 부분까지 잘라내고, 남은 문자열 부분은 후속 처리를 위해 include된 URL로 전달

---

## Naming URL patterns

: URL에 이름을 지정하는 것(`path` 함수의 `name` 인자를 정의해서 사용)

- ex) `path('articles/<int:num>/', view.detail, name='detail')`

- URL을 작성하는 모든 곳에서 변성 (`a` 태그의 `href` 속성 값 뿐만 아니라 `form`의 `action` 속성 등도 포함)
  
  - ex) `<a href="/dinner/">dinner</a>` -> `<a href="{% url 'dinner' %}">dinner</a>`

- <mark>`{% url 'url name' arg1 arg2 %}`</mark>
  
  : 주어진 URL 패턴의 이름과 일치하는 절대 경로 주소를 반환



---

## URL 이름 공간

- 단순히 url 이름만으로는 완벽하게 분리할 수 없어 이름에 성(key)를 붙이는 것

- `app_name = '앱이름'` 

- `url` 태그가 사용하는 모든 곳의 표기 변경
  
  - `{% url 'index' %}` -> `{% url 'articles:index' %}`

---

## cf. 추가 템플릿 경로 지정

- `BASE_DIR`
  
  : `settings`에서 경로지정을 편하게 하기 위해 최상단 지점을 지정해둔 변수
  
  - `BASE_DIR = Path(__file__).resolve().parent.parent`

- 템플릭 기본 경로 외 커스텀 경로 `settings.py`에 추가
  
  - `TEMPLATES`에 `'DIRS' : [ BASE_DIR / 'templates', ]` 추가

- 각 하위 템플릿에서 `extends`경로 수정
  
  - 약속된 경로는 안 쓰므로 `{% extends "articles/base.html" %}` 이고, `base.html`이 커스텀 경로 `templated`에 있다면 `{% extends "base.html" %}`로 수정
