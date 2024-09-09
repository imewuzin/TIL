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
  - 동적인 결과를 생성하기 위해 사용됨.

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
     - 템플릿, 데이터베이스 설정, 관리자 사이트 언어 코드, 타임존 설정.

3. **터미널 설정**:
   
   - 명령 프롬프트에서 서버 실행 (`python manage.py runserver`).
   - 서버를 다른 터미널에서 구동해두고 사용.

4. **초기 설정 완료**:
   
   - `python manage.py migrate` 명령으로 데이터베이스 마이그레이션.
   - `python manage.py createsuperuser` 명령으로 관리자 계정 생성.
   - 관리자 사이트에서 로그인 및 모델 관리 가능.

### 앱 생성 및 설정

1. **앱 생성**:
   
   - `python manage.py startapp exview` 명령으로 앱 생성.
   - 생성된 `exview` 폴더에서 작업.

2. **URL 설정**:
   
   - `exview/urls.py` 파일 생성.
   - URL 패턴을 설정하여 요청 경로와 뷰 함수 매핑.
   - `views.py`에서 `index` 함수 정의.

3. **뷰 함수 정의**:
   
   - `index` 함수 생성:
     
     ```python
     from django.http import HttpResponse
     
     def index(request):
         return HttpResponse("클라이언트로부터 요청을 받음")
     ```

4. **URL 포함**:
   
   - `mysite/urls.py` 파일에 `exview.urls` 포함.
     
     ```python
     from django.contrib import admin
     from django.urls import path, include
     
     urlpatterns = [
         path('admin/', admin.site.urls),
         path('exview/', include('exview.urls')),
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
