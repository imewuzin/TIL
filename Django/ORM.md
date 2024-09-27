# Object Relational Mapping

: 객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에 데이터를 변환하는 기술

- Django와 DB 간에 사용하는 언어가 다르기 때문에 소통 불가. Django에 내장된 ORM이 중간에서 이를 해석

---

## QuerySet API

: ORM에서 데이터를 검색, 필터링, 정렬 및 그룹화 하는 데 사용하는 도구

- 관련 문서
  
  - [1](https://docs.djangoproject.com/en/4.2/ref/models/querysets/)
  - [2](https://docs.djangoproject.com/en/4.2/topics/db/queries/)

- API를 사용해 SQL이 아닌 Python 코드로 데이터를 처리

- djang -(QuerySet API)-> ORM -(SQL)-> Database -(SQL)-> ORM -(QuerySet or Instance)-> django

- 구분
  
  - `Article.objects.all()` : Modelclass.Manager.QuerysetAPI
    
    : 전체 게시글을 보여달라는 예시

- Query : Database에 특정한 데이터를 보여 달라는 요청

- QuerysSet
  
  : Database에게서 전달받은 객체 목록(데이터 모음)
  
  - 순회가 가능한 데이터로써 1개 이상의 데이터를 불러와 사용할 수 있음
  
  - Django ORM을 통해 만들어진 자료형
  
  - Database가 단일한 객체를 반환할 때는 QuerySet이 아닌 모델(Class)의 인스턴스로 반환

- QuerySet API
  
  : python의 모델 클래스와 인스턴스를 사용해 DB에 데이터를 저장, 조회, 수정, 삭제하는 것

- CRUD
  
  : 소프트웨어가 가지는 기본적인 데이터 처리 기능
  
  - Create, Read, Update, Delete

- Django shell
  : Django 환경 안에서 실행되는 python shell
  
  - 입력하는 QuerySet API 구문이 Django 프로젝트에 영향을 미침

- 인스턴스 메서드 (Queryset API)
  
  - `all()` : 전체 데이터 조회
  - `save()` : 객체를 데이터베이스에 저장하는 인스턴스 메서드
  - `create()` : 인스턴스 생성 후 바로 저장
  - `filter()` : 주어진 매개변수와 일치하는 객체를 포함하는 QuerySet 반환
  - `get()` : 주어진 매개변수와 일치하는 객체 반환
    - 객체를 찾을 수 없으면 `DoesNotExist` 예외 발생시키고, 둘 이상의 객체를 찾으면 `MultipleObjectsReturned` 예외를 발생시킴
    - primary key와 같이 고유성(uniqueness)을 보장하는 조회에서 사용해야 함
    - ex) `Article.objects.get(pk=1)`, `Article.objects.get(content='django!')`

- create 실습
  
  > - 외부 라이브러리 설치 및 설정
  >   
  >   ```
  >   $ pip install ipython django-extensions
  >   ```
  >   
  >   ```python
  >   #  settings.py
  >   INSTALLED_APPS = [
  >     'articles',
  >     'django_extensions',
  >      ...,
  >   ]
  >   ```
  >   
  >   ```
  >   # requirements.txt 업데이트
  >   $ pip freeze > requirements.txt
  >   ```
  > 
  > - Django shell 만들기
  >   
  >   ```
  >   $ python manage.py shell_plus
  >   ```
  > 
  > - 데이터 객체 생성(3가지 방법)
  >   
  >   1. ```python
  >      # 특정 데이블에 새로운 행을 추가해 데이터 추가
  >      >>> article = Article()  # Article(class)로부터 article(instance) 생성
  >      >>> article.title = 'first'  # 인스턴스 변수(title)에 값 할당
  >      >>> article.content = 'django!'  # 인스턴스 변수(content)에 갑 ㅅ할당
  >      
  >      # save를 하지 않으면 아직 DB에 값이 저장되지 않음
  >      >>> article.save()
  >      >>> article  # <Article: Article object (1)> 출력
  >      >>> article.id  # 1 출력
  >      >>> argicle.pk  # 1 출력  # id 의미. Django가 pk로도 출력할 수 있게 한 것
  >      
  >      # 인스턴스 article을 활용하여 인스턴스 변수 활용하기
  >      >>> article.title  # 'first' 출력
  >      >>> ariticle.content  # 'django!' 출력
  >      >>> article.created_at  # datetime.datetime(2024, 9, 23, 6, 55, 42, ~~ )출력
  >      ```
  >   
  >   2. ```python
  >      # 한 줄로 쓰기
  >      >>> article = Article(title='second', content='django!')
  >      >>> article.save()
  >      ```
  >   
  >   3. ```python
  >      # QuerySet API 중 create() 메서드 활용
  >      
  >      # 위 2가지 방법과 달리 저장 이후 바로 생성된 데이터 반환됨
  >      
  >      > > > Article.objects.create(title='third', content='django!')  # <Article: Article object (3)> 출력
  >      ```

- Update
  : 인스턴스 변수를 변경 후 `save` 메서드 호출
  
  - 실습
    
    > ```
    > # 수정할 인스턴스 조회
    > >>> article = Article.objects.get(pk=1)
    > # 인스턴스 변수를 변경
    > >>> artile.title = 'byebye'
    > # 저장
    > >>> article.save()
    > # 확인
    > >>> article.title  # 'byebye' 출력
    > ```

- Delete
  : 삭제하려는 데이터 조회 후 `delete` 메서드 호출
  
  - 지워진 번호에 대해 재활용하지 않음. 1, 2 생성 후 2 삭제하면 다음 생성되는 번호는 3
  
  - 실습
    
    > ```
    > # 삭제할 인스턴스 조회
    > >>> article = Article.objects.get(pk=1)
    > # delete 메서드 호출(삭제된 객체가 반환)
    > >>> artile.delete()  # (1, {article.Article' : 1}) 출력
    > # 삭제한 데이터는 더이상 조회할 수 없음
    > >>> Article.objects.get(pk=1)  # DoesNotExist 예외 출력
    > ```

---

## CRUD - Create

- Create 로직을 구현하기 위해 필요한 view 함수
  
  - `new` : 사용자 입력 데이터를 받을 페이지를 렌더링
  
  - `create` : 사용자가 입력한 요청 데이터를 받아 DB에 저장

- new 기능 구현
  
  > ```python
  > # articles/urls.py
  > urlpatterns = [
  >     ...
  >     path('new/', views.new, name='new'),
  > ]
  > ```
  > 
  > ```python
  > # articles/viws.py
  > def new(request):
  >     return render(request, 'articles/new.html')
  > ```
  > 
  > ```html
  > <!-- templates/articles/new.html -->
  > <h1>NEW</h1>
  > <form action='#' method='GET'>
  >   <div>
  >     <label for ='title'>Title: /label>
  >     <input type="text" name="title" id="title">
  >   </div>
  >   <div>
  >     <label for='content'>Content: </label>
  >     <textarea name="content" id="content"></textarea>
  >   </div>
  >   <input type="submit">
  > </form>
  > <hr>
  > <a href="{% url 'articles:index' %}">[back]</a>
  > ```

- create 기능 구현
  
  > ```python
  > # articles/urls.py
  > urlpatterns = [
  >     ...
  >     path('create/', views.create, name='create'),
  > ]
  > ```
  > 
  > ```python
  > <!-- templates/articles/create.html -->
  > <h1>게시글이 작성 되었습니다.</h1>
  > ```
  > 
  > ```html
  > # articles/view.py
  > def create(request):
  >   title = request.GET.get('title')
  >   content = request.GET.get('content')
  >   article = Article(title=title, content = content)
  >   article.save()
  >   return render(request, 'articles/create.html')
  > ```
  > 
  > ```html
  > <!-- templates/articles/new.html -->
  > <h1>NEW</h1>
  > <form action="{% url 'articles:create' %}" method='GET'>
  >   <div>
  >     <label for ='title'>Title: /label>
  >     <input type="text" name="title" id="title">
  >   </div>
  >   <div>
  >     <label for='content'>Content: </label>
  >     <textarea name="content" id="content"></textarea>
  >   </div>
  >   <input type="submit">
  > </form>
  > <hr>
  > <a href="{% url 'articles:index' %}">[back]</a>
  > ```

- HTTP

        : 네트워크 상에서 데이터(리소스)를 주고 받기 위한 약속

- HTTP request methods
  
  : 데이터에 대해 수행을 원하는 작업(행동)을 나타내는 것. 서버에게 원하는 작업의 종류를 알려주는 역할
  
  - 클라이언트가 웹 서버에 특정 동작을 요청하기 위해 사용하는 표준 명령어
  
  - 대표 메서드 : `GET`, `POST`
  
  - `GET`
    
    : 서버로부터 데이터를 요청하고 받아오는데(**조회**) 사용
    
    - URL의 쿼리 문자열(Query String)을 통해 데이터를 전송
      
      - ex) `http://127.0.0.1:8000/articles/create/?title=제목&content=내용`
    
    - URL 길이에 제한이 있어 대량의 데이터 전송에는 적합하지 않음
    
    - 요청 URL이 브라우저 히스토리에 남음
    
    - 브라우저는 GET 요청의 응답을 로컬에 저장할 수 있음
    
    - 동일한 URL로 다시 요청할 때, 서버에 접속하지 않고 저장된 결과를 사용
    
    - 페이지 로딩 시간을 크게 단축
    
    - 사용 예시 : 검색 쿼리 전송, 웹 페이지 요청, API에서 데이터 조회
  
  - `POST`
    
    : 서버에 데이터를 제출하여 리소스를 변경(**생성, 수정, 삭제**)하는 데 사용
    
    - HTTP Body를 통해 데이터를 전송
    
    - `GET`에 비해 더 많은 양의 데이터를 전송할 수 있음
    
    - 브라우저 히스토리에 남지 않음
    
    - 일반적으로 서버의 상태를 변경하는 작업을 수행하기 때문에 기본적으로 캐시할 수 없음
    
    - 사용 예시 : 로그인 정보 제출, 파일 업로드, 새 데이터 생성, API에서 데이터 변경 요청

- HTTP response status code
  
  : 서버가 클라이언트의 요청에 대한 처리 결과를 나타내는 3자리 숫자
  
  - 역할
    
    - 클라이언트에게 요청 처리 결과를 명확히 전달
    
    - 문제 발생 시 디버깅에 도움
    
    - 웹 애플리케이션의 동작을 제어하는 데 사용
  
  - `403 Forbidden`
    
    : 서버에 요청이 전달되었지만 ,권한 때문에 거절되었다는 것을 의미
    
    - "CSRF token이 누락되었다"라는 응답
  
  - CSRF (Cross Site Request Forgery)
    
    : 사이트 간 요청 위조
    
    : 사용자가 자신의 의지와 무관하게 공격자가 의도한 행동을 하여 특정 웹 페이지를 보안에 취약하게 하거나 수정, 삭제 등의 작업을 하게 만드는 공격 방법
    
    - CSRF Token 적용
      
      - DTL의 `csrf_token` 태그를 사용해 손쉽게 사용자에게 토큰 값 부여
      
      - 요청 시 토큰 값도 함께 서버로 전송될 수 있도록 하는 것
      
      - ```html
        <!-- templates/articles/new.html -->
        <h1>NEW></h1>
        <form action="{% url 'articles:create' %}" method="POST">
          {% csrf_token %}
          ...
        ```
    
    - 요청 시 CSRF Token을 함께 보내야 하는 이유
      
      - Django 서버는 해당 요청이 DB에 데이터를 하나 생성하는(DB에 영향을 주는) 요청에 대해 "Django가 직접 제공한 페이지에서 데이터를 작성하고 있는 것인지"에 대한 확인 수단이 필요
      
      - 겉모습은 똑같은 위조 사이트나 정상적이지 않은 요청에 대한 방어 수단
    
    - POST일 때만 Token을 확인하는 이유
      
      - POST는 단순 조회를 위한 GET과 달리 특정 리소스에 변경(생성, 수정, 삭제)을 요구하는 의미와 기술적인 부분을 가지고 있기 때문에
      
      - 데이터베이스에 대한 변경사항을 만드는 요청이기 때문에 토큰을 사용해 최소한의 신원 확인을 하는 것

- `redirect()`
  
  : 클라이언트가 인자에 작성된 주소로 다시 요청을 보내도록 하는 함수
  
  - 서버는 데이터 저장 후, 페이지를 응답하는 것이 아닌 사용자를 적절한 기존 페이지로 보내야 함(클라이언트가 GET 요청을 한번 더 보내도록 응답하는 것)
  
  - 동작 원리
    
    ```python
    from django.shortcuts import render, redirect
    
    def create(request):
      ...
      return redirect('articles:detail', article.pk)
    ```
    
    1. `redirect` 응답을 받은 클라이언트는 `detail` url로 다시 요청을 보내게 됨
    
    2. 결과적으로 `detail` view 함수가 호출되어 `detail` view 함수의 반환 결과인 `detail` 페이지를 응답받게 되는 것
    
    3. 결국 사용자는 게시글 작성 후 작성된 게시글의 detail 페이지로 이동하는 것으로 느낌
       
       > 1. [POST] 게시글 작성 요청 (+ 입력 데이터)
       > 
       > 2. create view 함수 호출
       > 
       > 3. redirect 응답(detail 주소로 요청을 보내라)
       > 
       > 4. [GET] detail 페이지 요청
       > 
       > 5. detail view 함수 호출
       > 
       >  6. detail 페이지 응답



---

## CRUD - Update

- Update 로직을 구현하기 위해 필요한 view 함수
  
  - edit : 사용자 입력 데이터를 받을 페이지를 렌더링
  
  - update : 사용자가 입력한 데이터를 받아 DB에 저장

- edit 기능 구현
  
  > ```python
  > # articles/urls.py
  > 
  > urlpatterns = [
  >     ...
  >     path('<int:pk>/edit/', views.edit, name='edit'),
  > ]
  > ```
  > 
  > ```python
  > # articles/views.py
  > 
  > def edit(request, pk):
  >     article = Article.objects.get(pk=pk)
  >     context = {
  >         'article' : article,
  >     }
  >     return render(request, 'articles/edit.html', context)
  > ```
  > 
  > - 수정 시 이전 데이터가 출력될 수 있도록 작성하기
  > 
  > ```html
  > <!-- articles/edit.html -->
  > 
  > <h1>EDIT</h1>
  > <form action='#' method="POST">
  >   {% csrf_token %}
  >   <div>
  >     <label for="title">Title: </label>
  >     <input type="text" name="title" id="title" value="{{ article.title}}">
  >   </div>
  >   <div>
  >     <label for="content">Content: </label>
  >     <textarea name="content" id="content">{{ article.content }}</textarea>
  >   </div>
  >   <input type="submit">
  > </form>
  > <hr>
  > <a href="{% url 'articles:index' %}">[back]</a>
  > ```
  > 
  > - edit 페이지로 이동하기 위한 하이퍼링크 작성
  > 
  > ```html
  > <!-- articles/detail.html -->
  > 
  > <body>
  >   <a href="{ url 'articles:edit' article.pk %}">EDIT</a><br>
  >   <form action="{% url 'articles:delete' article.pk %}" method="POST">
  >     {% csrf_token %}
  >     <input type="submit" value="DELETE">
  >   </form>
  >   <a href="{% url 'articles:index %}">[back]</a>
  > </body
  > ```

---

## CRUD - Delete

- 기능 구현
  
  > ```python
  > # articles/url.py
  > 
  > urlpatterns = [
  >     ...
  >     path('<int:pk>/delete/', view.delete, name='delete'),
  > ]
  > ```
  > 
  > ```python
  > # articles/view.py
  > 
  > def delete(request, pk):
  >     article = Article.objects.get(pk=pk)
  >     article.delete()
  >     return redirect('articles:index')
  > ```
  > 
  > ```html
  > <!-- articles/detail.html -->
  > 
  > <body>
  >   <h2>DETAIL</h2>
  >   ...
  >   <hr>
  >   <form action="{% url 'articles:delete' article.pk %}" method="POST">
  >     {% csrf_token %}
  >     <input type="submit" value="DELETE">
  >   </form>
  >   <a href="{% url 'articles:index' %}">[back]</a>
  > </body>
  > ```



 

### cf. ORM, QuerySet API를 사용하는 이유

- 데이터베이스 추상화
  : 개발자는 특정 데이터베이스 시스템에 종속되지 않고 일관된 방식으로 데이터를 다룰 수 있음
- 생산성 향상
  : 복잡한 SQL 쿼리를 직접 작성하는 대신 Python 코드로 데이터베이스 작업을 수행할 수 있음
- 객체 지향적 접근
  : 데이터베이스 테이블을 Python 객체로 다룰 수 있어 객체 지향 프로그래밍의 이점을 활용할 수 있음
