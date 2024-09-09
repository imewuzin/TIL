# Django Tutorial

- [Django documentation | Django documentation | Django](https://docs.djangoproject.com/en/5.1/)

## 1. 설치

- **장고 공식 홈페이지 방문**
  
  - 장고 공식 홈페이지([Django Documentation](https://docs.djangoproject.com))에 접속하여 시작하기 문서 참고
  
  - 언어 선택에서 한글(ko)을 선택하면 한국어로 된 튜토리얼을 볼 수 있음
    
    - URL에서 `en` -> `ko` 가능

- **장고 설치 확인**
  
  - 설치 여부를 확인하기 위해 커맨드 라인에서 `python -m django --version` 명령어를 실행
  - 장고가 설치되어 있지 않다면 `No module named 'django'` 에러가 발생
    - 설치되어 있다면 버전 출력
  - 장고를 설치하려면 `pip install django`
    - 특정 버전을 설치하려면 `pip install django==4.2`
  - 커맨드 라인에 `path` 입력해 `C:\Python\Python311\Scripts\;`(pip.ese, django-admin,exe) `C:\Python\Python311\;`(python.exe 경로) 있는지 확인
  - 커맨드 라인에 `django-admin` 입력 시 명령어 확인 가능

- **장고 프로젝트 생성**
  
  - 장고 프로젝트를 만들기 위해 작업할 디렉토리로 이동
    - ex) `cd C:\django`
  - `django-admin startproject mysite` 명령어 입력
    - `mysite`라는 이름의 프로젝트 생성
    - `django`, `test` 등 python 또는 django에서 사용 중인 이름은 피해야함. 충돌 발생
  - 프로젝트 디렉토리 구조
    - 디렉토리 : `mysite` (Project)
    - 디렉토리 속 서브디렉토리
      - `manage.py` : 커맨드 라인의 유틸리티
      - `mysite` : App
    - `mysite` 서브디렉토리 속 파일
      - `__init__.py` : Python으로 하여금 이 디렉토리를 패키지처럼 다루라고 알려주는 용도의 빈 파일
      - `settings.py` : 현재 Django 프로젝트의 환경 및 구성 저장
      - `urls.py` : 현재 Django project의 URL 선언 저장. Djano로 작성도니 사이트의 목차
      - `asgi.py` : 현재 프로젝트를 서비스하기 위한 ASGI-호환 웹 서버의 진입점
      - `wsgi.py` : 현재 프로젝트를 서비스하기 위한 WSGI 호환 웹 서버의 진입점
  - Project `mysite`는 이름 변경해도 무관. 변경 권장
    - `ren mysite project01(예시)` 혹은 직접 이름 바꾸기

- **프로젝트 실행**
  
  - 프로젝트 폴더로 이동 `cd project 01`
  - `python manage.py runserver` 로 프로젝트 실행
  - 기본적으로 8000번 포트에서 실행되며, 브라우저에서 `http://127.0.0.1:8000/`에 접속하여 장고의 환영 페이지 확인 가능
  - 포트 변경 원할 시 : `py manage.py runserver 8080`
  - 서버의  IP 변경 : `py manage.py runserver 0.0.0.0:8000` (포트와 함께 전달)
  - <mark>`runserver`는 자동 새로고침이 됨</mark>

## 2. 설문조사 앱 만들기

- cf. 프로젝트 vs 앱
  
  - 프로젝트
    
    : 특정 웹 사이트에 대한 구성 및 앱의 모음
    
    - 한 트로젝트에 여러 개의 앱 포함 가능
  
  - 앱
    
    : 블로그 시스템, 공개 기록 데이터 베이스 또는 소규모 의견조사 앱과 같은 작업을 수행하는 웹 어플리케이션
    
    - 여러 프로젝트에 있을 수 있음

- **앱 생성**
  
  - 서버 실행중이므로 `ctrl+break`로 종료
  
  - 추천 : <mark>vscode의 `Pytahon Extension Pack` extension</mark>
  
  - 장고 프로젝트 내에서 앱 생성 : `python manage.py startapp polls` 
    
    - `polls`라는 이름의 앱 생성, 앱 디렉토리에는 `migrations`, `__init__.py`, `admin.py`, `apps.py`, `models.py`, `tests.py`, `views.py` 파일 포함
  
  - `polls` 디렉토리에 `urls.py` 파일 생성
    
    > - `""` URL 요청이 들어왔을 때 `view.index` 함수로 처리하겠다. 이 요청에 이름을 붙여준 게 `name=`
    >   
    >   ```python
    >   from django.urls import path
    >   
    >   from . import views
    >   
    >   urlpatterns = [
    >       path()
    >       path("", views.index, name="index"),
    >   
    >   ]
    >   ```
  
  - `polls` 관련 url은 다 `polls` 내 `urls.py` 에서 처리하기
  
  - 이 처리 내용을 `mysite` 프로젝트에게 알려주기 위한 명령어를 `mysit` 내 `urls.py`에 추가
    
    > ```python
    > from django.contrib import admin
    > from django.urls import path, include
    > 
    > urlpatterns = [
    >     path('polls/', include('polls.urls;)),
    >     path('admin/', admin.site.urls),
    > ]
    > ```
  
  - `include` 추가할 때 : 다른 URL 패턴을 포함할 때마다 항상 사용
    
    - 유일한 예외 : `admin.site.urls`

- index 뷰가 URLconf에 잘 연결되었는지 확인
  
  - `py manage.py runserver`
  
  - 브라우저에서 `http://localhost:8000/polls/` 입력하면 확인 가능
  
  - 기본 페이지는 새로 고침해도 안 나옴. URL이 추가되었기 때문에 기본 페이지 삭제
  
  - `mysite`에 `'polls'` URL에 대한 경로 있는지 확인
    
    -> 있으므로 경로 이동(`polls.urls`로)
    
    -> `polls`의 `urls.py` 보면 `""`대한 경로 있어서 처리

## 3. 데이터 데이스 설치

- `mysite` 내 `settings.py` 열기

- 기본 DB 설정은 SQLite (아무것도 미리 생성할 필요X)
  
  - 이외의 DB 사용시, USER, PASSWORD, HOST 같은 추가 설정 필요
    
    - DB의 대화형 프롬프트 내에서 `CREATE DATABASE database_name;` 으로 DB생성
    
    - `mysite/settings.py` 에 설정된 DB사용자가 `create database` 권한 있는지 확인해야 함

- 시간대 설정 : `TIME_ZONE = 'Asia/Seoul'`

- cf. HTTP는 비연결형 프로토콜(통신 끝나면 연결 끊어버림). 네이버 로그인하고 메일, 블로그 등 볼 때 로그인된 상태로 보여지는데 사실은 불가능함. 이를 가능하게 만들어주려고 존재하는 기술이 세션

- 기본 어플리케이션(`admin`, `auth`, `contenttypes`, `sessions`, `messages`, `staticfiles`) 중 몇몇은 최소 1개 이상의 DB테이블 사용 -> DB에서 테이블 미리 만들어 놓기
  
  - `python manage.py migrate`

- `DB Broswer for SQLite` 에서 `Windows PortableApp` 설치하면 db.sqlite3 파일 속 내용 확인 가능

## 4. 모델 만들기

: 부가적인 메타데이터를 가진 데이터베이스의 구조(layout)

- `models.py`  에서 생성 및 수정 가능 

- `models.ForeignKey` : 참조하는 모델에 해당 키가 있다는 제약 있음
  
  - `on_delete = models.CASCADE` : 참조하는 모델의 키 삭제 시, 해당하는 외래키의 데이터들도 삭제되게 함

- 모델 생수 후, `polls` 의 `settings.py` 에 앱 추가 (PollsConfig 클래스)
  
  > ```python
  > INSTALLED_APPS = [
  >     "polls.apps.PollsConfig",
  >     "django.contrib.admin",
  >     "django.contrib.auth",
  >     "django.contrib.contenttypes",
  >     "django.contrib.sessions",
  >     "django.contrib.messages",
  >     "django.contrib.staticfiles",
  > ]
  > ```

- `python manage.py makemigrations polls` : 모델을 변경시킨 사실(생성, 수정 등)과 이 변경사항을 migration으로 저장하고 싶다는 것을 Django에게 알려줌 (DB 적용은 아직)

- `py manage.py sqlmigrate polls 0001` : 실행된 SQL 명령어들 확인 가능

- `py manage.py migrate` : DB 생성 (이상 있다면 수정 후 생성할 것)

- 

=> `models.py` 에서 모델 변경 시 -> `python manage.py makemigrations` 로 변경 사항에 대한 migration 생성 -> `python manage.py migrate` 로 변경사항 DB에 적용

## 5. API 이용하기

- `py manage.py shell` : Python 쉘 실행
  
  - `manage.py`에 설정된 `DJANGO_SETTINGS_MODULE` 환경 변수 때문에

- 아래 코드 순서대로 실행
  
  ```python
  from polls.models import Choice, Question
  Question.objects.all()  # select * from polls_question;
  from django.utils import timezone
  q = Question(question_text="What's new?", pub_date=timezone.now()
  q.save()
  q.auestion_text = "What's up?"  # 아직 반영 X
  q.save()  # DB에 반영(업데이트)
  Question.objects.all()  # Question객체 1개 생성된 것 확인 가능
  ```

- `models.py`에 str함수 추가하면 보기 편함
  
  - 단, python shell 실행 중에 추가하면 보이지 않음. 다시 실행해야 함

- 추가 코드
  
  ```python
  Question.objects.filter(id=1)  # id가 1인 데이터만 보기
  Question.objects.filter(auestion_text__startswith="what"  # What으로 시작하는 질문을 가진 것들만 보기
  
  from django.utils import timezone
  current_year = timezone.now().year
  Question.objects.get(pub_date__year=current_year) # 현재 년도인 것만 가져오기
  # get은 존재하는 것만 가져올 수 있음. 존재하지 않는 것 가져오려면 DoesNotExist오류 남
  
  q = Question.objects.get(pk=1)  # 지금은 id로 찾는 것과 동일
  q.was_published_recently()  # 최근에 게시되었는지 여부 확인(특정 기간 설정 가능)
  
  q = Question.objects.get(pk=1)
  q.choice_set.all()  # Choice.objects.all()과 범위(전체, 특정 키만 참조하고 있는 값)만 다르고 개념 같음
  
  q.choice_set.create(choice_text="Not much", votes=0)  # 데이터 생성해주는 형식
  q.choice_set.create(choice_text="The sky", votes=0)
  q.choice_set.create(choice_text="Just hacking again", votes=0)
  
  q.choice_set.all()  # <QuerySet [<Choide: Not much>, <Choice: The sky>, <Choice: Just hacking again>]> 출력
  q  # <Question: Whats' up?> 출력
  q.choice_set.count()  # 3 출력
  Choice.objects.all()  # <QuerySet [<Choide: Not much>, <Choice: The sky>, <Choice: Just hacking again>]> 출력 (모든게 다 나옴)
  
  Choice.objects.filter(question__pub_date__year=current_year)  # <QuerySet [<Choide: Not much>, <Choice: The sky>, <Choice: Just hacking again>]> 출력
  c= q.choice_set.filter(choice_text__startswith="Just hacking")
  c.delete()  # 삭제. q.choice_set.filter(choice_text__startswith="Just").delete()해도 됨
  ```

## 6. 관리자 계정 생성

- cmd에서 Django에서 관리자 계정 생성 : `python manage.py createsuperuser`

- 정보 입력
  
  - 사용자 이름: `admin` (직접 입력 가능)
  - 이메일 주소: 실제 이메일 주소를 입력하거나 `admin@example.com`과 같은 형식 사용
  - 비밀번호: 비밀번호는 복잡하게 설정하는 것이 좋지만, 연습을 위해 간단한 비밀번호(예: `1234`)를 사용 가능
    - 비밀번호 복잡도: 비밀번호는 대문자, 소문자, 숫자, 특수문자 및 길이를 포함한 복잡도 요구. 예를 들어, 비밀번호는 최소 8자 이상이어야 하며, 복잡한 조합이 필요.
  - 비밀번호 입력: 비밀번호를 두 번 입력해 확인
    - 비밀번호가 너무 짧거나 단순하면 오류 메시지 표시
    - 복잡한 비밀번호를 입력하거나 `y`를 선택하여 계속 진행가능

- 관리자 사이트 접근
  
  - 서버 실행: `python manage.py runserver` 명령어로 개발 서버 실행(슈퍼유저만 로그인 가능)
  
  - 로그인: 브라우저에서 `/admin` URL로 접근하여 생성한 관리자 계정으로 로그인
  
  - 관리자 사이트에서 사용자와 그룹을 관리 및 기본적으로 제공되는 기능 탐색 가능

- `auth_user` 테이블에서 생성된 사용자 정보 확인 가능
  
  - 비밀번호는 해싱되어 저장(해싱 알고리즘에 관련된 정보도 함께 저장)

- 모델 관리
  
  - 모델 추가
    
    - 관리할 모델을 추가하기 위해 앱의 `admin.py` 파일에 모델 등록
      
      > ```pytyon
      > from django.contrib import admin
      > from .models import Question
      > 
      > admin.site.register(Question)
      > ```
  
  - 변경 사항 저장
    
    : 변경 후 서비스를 리로드하면 관리 사이트에서 모델을 관리 가능
  
  - 질문 관리
    
    : 관리 사이트에서 질문을 추가, 수정 가능
    
    - 각 질문은 `Question` 모델의 필드에 따라 표시
  
  - 필드 이름 변경
    
    : 모델의 필드 이름은 사용자에게 보여질 때 다른 이름으로 설정 가능 (직관적인 관리 가능)
  
  - 히스토리 관리
    
    : 질문의 수정 이력 확인 가능  

## 7. 뷰(View) 만들기 (4개)

- **뷰의 역할**
  
  : 웹 페이지의 콘텐츠를 생성하여 사용자에게 전달하는 역할
  
  - 장고에서는 뷰를 파이썬 함수 또는 클래스 기반으로 정의 가능
  
  - ex) 블로그 앱에서는 블로그의 메인 페이지를 보여주거나, 특정 블로그 게시물의 세부 정보를 보여주는 기능 등을 포함 가능

- URL 설정
  
  - URL 패턴
    
    : Django에서 URL 패턴은 웹 페이지의 주소를 설정하고 이를 통해 어떤 뷰를 호출할지 정의. ex) `polls/34/`
  
  - URL과 뷰 매칭
    
    : 사용자가 특정 URL로 요청을 보내면 Django는 URL 설정에서 해당 패턴과 매칭되는 뷰를 호출

## 8. 템플릿 만들기

- 템플릿
  
  : HTML 파일로, 동적으로 콘텐츠를 생성 가능
  
  - [템플릿 | Django 문서 | Django](https://docs.djangoproject.com/ko/5.1/topics/templates/) 참고
  
  - 템플릿 위치
    
    : Django에서는 앱 내의 `templates` 폴더에 템플릿 파일 배치
    
    - 앱 이름으로 구분하여 템플릿을 저장 템플릿 충돌 방지할 것
  
  - 템플릿 코드 : HTML 방식

- `render()` 함수
  
  - Django에서 제공하는 `shortcuts` 함수 중 하나
  
  - 템플릿과 데이터를 결합하여 HTTP 응답 생성
  
  - 템플릿을 지정하고, 컨텍스트(데이터)를 전달하여 렌더링된 HTML 반환
  
  - ex) `render(request, "polls/index.html", context)`
  
  - 뷰 함수에서 데이터 처리:
    
    - 뷰 함수에서 데이터를 조회한 후, 이를 딕셔너리 형태로 `context`를 만들고 `render` 함수에 전달
    - `render`는 이 데이터를 템플릿에 주입하여 최종 HTML 생성

- `get_object_or_404` 
  
  - 특정 모델 객체를 조회하고, 객체가 없을 경우 404 에러를 발생시키는 단축 함수
  - 이 함수는 `try-except` 블록을 사용하는 대신 간편하게 404 에러 처리 가능
  
  ```python
  from dfnago.shortcuts import get_object_or 404, reder
  from .models import Question
  
  def details(requestj, question_id):
      question = get_object_or_404(Questionj, pk=question_id)
      return render(request, "polls/detail.html", {question" : question})
  ```

- 템플릿에서 데이터 출력
  
  - 변수 값 :  `{{ }}`로 출력
  - 반복문과 조건문을 사용하여 동적으로 콘텐츠를 생성 가능

- 하드코딩된 URL 제거
  
  - 템플릿에서 URL을 직접 하드코딩하는 대신 `{% url 'view_name' %}` 태그를 사용해 URL을 동적으로 생성
  - URL 패턴이 변경되더라도 템플릿에서 URL을 자동으로 업데이트

- 앱 네임 스페이스
  
  : 앱에 이름을 붙여줌으로써 URL 이름이 동일해도 구분 가능하게 변경
  
  - 앱 이름 붙여주기 : `app_name = "polls"` 를 `urls.py` 파일에 추가
  
  - 요청 주소들 변경 : `{ url 'polls:detail' }` 형태로 앱 이름 추가해주기

## 9. form `<form>`

    : 폼은 HTML 태그 `<form>`을 사용하여 사용자로부터 입력 값을 받을 수 있게 함

- 장고에서는 템플릿을 사용하여 폼을 작성하고 처리

- HTML과 템플릿 태그
  
  - HTML 태그 : 사용자에게 직접 보여지는 요소
  
  - 템플릿 태그 : `{% %}` 형식으로 사용되며, 장고가 처리해 HTML로 변환하는 코드

- 폼 태그와 POST 방식
  
  - 폼 태그
    
    : 사용자 입력을 받기 위한 태그
    
    - `method="post"`로 설정하여 POST 방식으로 데이터를 전송(`get` 방식도 있음)
  
  - POST 방식 : 서버에 데이터를 전송할 때 사용. URL에 파라미터를 붙이지 않고 요청의 본문에 데이터를 담아 전송

- 폼 구성 요소
  
  1. 폼 태그
     
     : `<form method="post" action="{% url 'polls:vote' question.id %}">`
     
     - `method="post"`: POST 방식으로 데이터 전송
     - `action`: 폼 데이터를 전송할 URL 지정
  
  2. 필드셋
     
     : `<fieldset>`은 폼 요소를 그룹화
     
     - `<legend>` 태그로 제목 표시
  
  3. 인풋 태그
     
     : `<input type="radio">`는 라디오 버튼 생성
     
     - `name`: 같은 이름의 인풋 태그끼리 그룹화됩니다.
     - `value`: 사용자가 선택한 값이 서버로 전송됩니다.
     - **레이블**: `<label>` 태그로 입력 요소에 대한 설명을 제공합니다.
  
  4. submit 버튼
     
     : `<input type="submit">` 폼 제출 버튼 생성

- 데이터 처리 및 리디렉션
  
  - 서버 처리
    
    : 사용자가 폼을 제출하면 서버에서 데이터를 처리. 이 경우, 선택된 값을 데이터베이스에 저장하고 응답 반환.
  
  - 리디렉션
    
    : 서버에서 데이터 처리가 완료된 후, 브라우저를 다른 URL로 리디렉션 가능
    
    - `HttpResponseRedirect` 사용
    
    - ex) `return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))` : 사용자를 결과 페이지로 리디렉션

- `reverse` 함수
  
  : URL 패턴을 이름으로 역으로 찾아주는 함수
  
  - ex) `reverse('polls:results', args=[question.id])`는 `polls` 앱의 `results` : URL 패턴을 찾고, `question.id`를 URL에 포함

---

## Django의 MTV 패턴 동작 흐름

![MTV pattern](https://github.com/user-attachments/assets/17c1d761-d031-4ff1-9207-26b7016d41b1)

1. 클라이언트로부터 요청을 받으면 URLconf모듈을 이용해 URL 분석

2. URL 분석 결과를 통해 해당 URL에 대한 처리를 담당할 뷰 결정

3. 뷰는 자신의 로직을 실행하며 만일 데이터베이스 처리가 필요하면 모델을 통해 처리하고 결과를 반환 받음

4. 뷰는 자신의 로직 처리가 끝나면 적절한 템플릿을 이용해 클라이언트에 응답할 HTML 파일 생성

5. 뷰는 최종 결과로 HTML 파일을 클라이언트에게 응답 전송

---

# 제너릭 뷰
