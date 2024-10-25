# REST API

- API (Application Programming Interface)
  
  : 두 소프트웨어가 서로 통신할 수 있게 하는 메커니즘
  
  - 클라이언트-서버처럼 서로 다른 프로그램에서 요청과 응답을 받을 수 있도록 만든 체계
  
  - API 역할
    
    : 복잡한 코드를 추상화하여 대신 사용할 수 있는 몇 가지 더 쉬운 구문을 제공

- Web API
  
  : 웹 서버 또는 웹 브라우저를 위한 API
  
  - 현대 웹 개발은 하나부터 열까지 직접 개발하기보다 여러 Open API들을 활용하는 추세
  
  - 대표적인 Third Party Open API 서비스 목록
    
    : Youtube API, Google Map API, Naver Papago API, Kakao Map API

- REST (Representational State Transfer)
  
  : API Server를 개발하기 위한 일종의 소프트웨어 설계 방법론. 규칙 X
  
  - API Server를 설계하는 구조가 서로 다르니 이렇게 맞춰서 설계하는 게 어때?
  
  - REST 원리를 따르는 시스템을 **RESTful** 하다고 부름
  
  - '자원을 정의'하고 '자원에 대한 주소를 지정'하는 전반적인 방법을 서술
    
    - '각각 API 서버 구조를 작성하는 모습이 너무 다르니 어느정도 약속을 만들어서 다같이 통일해서 쓰자!'

- REST API
  
  : REST라는 설계 디자인 약속을 지켜 구현한 API
  
  - REST에서 자원을 정의하고 주소를 지정하는 방법
    
    - 자원의 식별 : `URI`
    
    - 자원의 행위 : `HTTP Methods`
    
    - 자원의 표현 : `JSON` 데이터 (궁극적으로 표현되는 데이터 결과물)

---

## 통합 자원 식별자 URI (Uniform Resource Identifier)

: 인터넷에서 리소스(자원)를 식별하는 문자열

- 가장 일반적인 URI는 웹 주소로 알려진 `URL`

- 통합 자원 위치 URL (Uniform Resource Locator)
  
  : 웹에서 주어진 리소스의 주소
  
  - 네트워크 상에 리소스가 어디 있는지를 알려주기 위한 약속
  
  - `http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument`
    
    - Scheme (or Protocol) - `http`
      
      : 브라우저가 리소스를 요청하는데 사용해야 하는 규약
      
      - URL의 첫 부분은 브라우저가 어떤 규약을 사용하는지를 나타냄
      
      - 기본적으로 웹은 `http(s)`를 요구
      
      - 메일을 열기 위한 `mailto:`, 파일을 전송하기 위한 `ftp:` 등 다른 프로토콜도 존재
    
    - Authority
      
      - Domain Name - `www.example.com`
        
        : 요청 중인 웹 서버를 나타냄
        
        - 어떤 웹 서버가 요구되는지를 가리키며 직접 IP 주소를 사용하는 것도 가능하지만, 사람이 외우기 어렵기 때문에 주로 Domain Name으로 사용
        
        - ex) 도메인 google.com의 IP 주소는 142.251.42.142
      
      - Port - `80`
        
        : 웹 서버의 리소스에 접근하는데 사용되는 기술적인 문(Gate)
        
        - HTTP 프로토콜의 표준 포트 (`HTTP` - 80, `HTTPS` - 443)
        
        - 표준 포트만 작성 시 생략 가능
    
    - Path - `/path/to/myfile.html`
      
      : 웹 서버의 리소스 경로
      
      - 초기에는 실제 파일이 위치한 물리 적 위치를 나타냈지만, 오늘날은 실제 위치가 아닌 추상화된 형태의 구조를 표현
      
      - ex) `/articles/create/` 라는 주소가 실제 `articles` 폴더 안에 `create` 폴더 안을 나타내는 것은 아님
    
    - Parameters - `?key1=value1&key2=value2`
      
      : 웹 서버에 제공하는 추가적인 데이터
      
      - `&`기호로 구분되는 key-value 쌍 목록
      
      - 서버는 리소스를 응답하기 전에 이러한 파라미터를 사용하여 추가 작업을 수행할 수 있음
    
    - Anchor - `#SomewhereInTheDocument`
      
      : 일종의 '북마크'를 나타내며 브라우저에 해당 지점에 있는 콘텐츠를 표시
      
      - `#` (fragment identifier, 부분 식별자) 이후 부분은 서버에 전송되지 않음
      
      - `https://docs.djangoproject.com/en/4.2/intro/install/#quick-install-guide` 요청에서 `#quick-install-guide`는 서버에 전달되지 않고 브라우저에게 해당 지점으로 이동할 수 있도록 함

---

## HTTP Request Methods

: 리소스에 대한 행위(수행하고자 하는 동작)를 정의

- `HTTP verbs`라고도 함

- 대표 HTTP Request Methods
  
  - `GET` : 서버에 리소스의 표현을 요청. GET을 사용하는 요청은 데이터만 검색해야 함
  
  - `POST` : 데이터를 지정된 리소스에 제출. 서버의 상태를 변경
  
  - `PUT` : 요청한 주소의 리소스를 수정
  
  - `DELETE` : 지정된 리소스를 삭제

## HTTP response status codes

: 특정 HTTP 요청이 성공적으로 완료되었는지 여부를 나타냄

- 5가지 응답 그룹으로 분류
  
  - `Informational responses (100-199)`
  
  - `Successful responses (200-299)`
  
  - `Redirection messages (300-399)`
  
  - `Client error responses (400-499)`
  
  - `Server error responses (500-599)`

---

## 자원의 표현

: 그동안 서버가 응답(자원을 표현)했던 것

- 지금까지 Django 서버는 사용자에게 페이지(`html`)만 응답하고 있었음

- 하지만 서버가 응답할 수 있는 것은 페이지뿐만 아니라 다양한 데이터 타입을 응답할 수 있음

- REST API는 이 중에서도 `JSON` 타입으로 응답하는 것을 권장

- 응답 데이터 타입의 변화
  
  1. 페이지(html)만을 응답하는 서버
  
  2. 이제는 JSON 데이터를 응답하는 REST API 서버로의 변환
  
  3. Django는 더 이상 Template 부분에 대한 역할을 담당하지 않게 되며, Front-end와 Back-end가 분리되어 구성됨
  
  4. 이제부터 Django를 사용해 RESTful API 서버를 구축할 것

- `JSON` 데이터 응답
  
  - 사전준비
    
    - migrate 이후 준비된 fixtures 파일을 load하여 실습용 초기 데이터 입력
      
      `python manage.py loaddata articles.json`
    
    - `http://127.0.0.1:8000/api/v1/articles/` 요청 후 응답 확인
  
  - python으로 json 데이터 처리하기 - 준비된 `python-request-sample.py` 확인
    
    ```python
    import requests
    from pprint import pprint
    
    response = requests.get('http://127.0.0.1:8000/api/v1/articles/')
    
    # json을 python 타입으로 변환
    result = response.json()
    
    print(type(result))
    # pprint(result)
    # pprint(result[0])
    # pprint(result[0].get('title'))
    ```

---

## DRF with Single Model

### DRF Django REST framework

: Django에서 Restful API 서버를 쉽게 구축할 수 있도록 도와주는 오픈소스 라이브러리

- 프로젝트 준비
  
  1. migrate 이후 준비된 fixtures 파일을 load하여 실습용 초기 데이터 입력
     
     `python manage.py loaddata articles.json`

- [Postman](https://www.postman.com/downloads) 설치
  
  : API 개발 및 테스트를 위한 서비스
  
  - 요청 데이터 구성, 응답 확인, 환경 설정, 자동화 테스트 등 다양한 기능을 제공

### 직렬화 Serialization

: 여러 시스템에서 활용하기 위해 데이터 구조나 객체 상태를 나중에 재구성할 수 있는 포맷으로 변환하는 과정

: 어떠한 언어나 환경에서도 나중에 다시 쉽게 사용할 수 있는 포맷으로 변환하는 과정

- Serializer
  
  : Serialization을 진행하여 Serialized data를 반환해주는 클래스

- ModelSerializer
  
  : Django 모델과 연결된 Serializer 클래스
  
  - 일반 Serializer와 달리 사용자 입력 데이터를 받아 자동으로 모델 필드에 맞추어 Serialization을 진행
  
  - 사용 예시 - `Article` 모델을 토대로 직렬화를 수행하는 `ArticleSerializer`정의
    
    - 게시글 데이터 목록 제공
    
    > ```python
    > # articles/serializers.py
    > 
    > from rest_framework import serializers
    > from .models import Article
    > 
    > class ArticleListSerializer(serializers.ModelSerializer):
    >     class Meta:
    >         model = Article
    >         fields = '__all__'
    > ```

### CRUD with ModelSerializer

- URL과 HTTP requests methods 설계
  
  |               | `GET`   | `POST` | `PUT`   | `DELETE` |
  | ------------- | ------- | ------ | ------- | -------- |
  | `articles/`   | 전체 글 조회 | 글 작성   |         |          |
  | `articles/1/` | 1번 글 조회 |        | 1번 글 수정 | 1번 글 삭제  |

- GET method - 조회
  
  - 게시글 데이터 목록 조회하기
  1. 게시글 데이터 목록을 제공하는 `ArticleListSerializer` 정의
  
  > ```python
  > # articles/serializers.py
  > 
  > from rest_framework import serializers
  > from .models import Article
  > 
  > class ArticleListSerializer(serializers.ModelSerializer):
  >     class Meta:
  >         model = Article
  >         fields = ('id', 'title', 'content',)
  > ```
  
  2. url 및 view 함수 작성
     
     - ModelSerializer의 인자 및 속성
       
       - `many` 옵션 : Serialize 대상이 QuerySet인 경우 입력
       
       - `data` 속성 : Serialized data 객체에서 실제 데이터를 추출
  
  > ```python
  > # articles/urls.py
  > 
  > urlpatterns = [
  >     path('articles/', views.article_list),
  > ]
  > ```
  > 
  > ```python
  > # articles/views.py
  > 
  > from rest_framework.response import Response
  > from rest_framework.decorators import api_view
  > 
  > from .models import Article
  > from .serializers import ArticleListSerializer
  > 
  > @api_view(['GET'])
  > def article_list(request):
  >     articles = Article.objects.all()
  >     serializer = ArticleListSerializer(articles, many=True)
  >     return Response(serializer.data)
  > ```
  
  3. 과거 view 함수와의 응답 데이터 비교
     
     - 과거) HTML에 출력되도록 페이지와 함께 응답했던 view 함수
       
       > ```python
       > def index(request):
       >     articles = Article.objects.all()
       >     context = {
       >         'articles' : articles,
       >     }
       >     return render(request, 'articles/index.html' context)
       > ```
     
     - 현재) JSON 데이터로 serialization하여 페이지 없이 응답하는 view 함수
       
       > ```python
       > @api_view(['GET'])
       > def article_list(request):
       >     articles = Article.objects.all()
       >     serializer = ArticleListSerializer(article, many=True)
       >     return Response(serializer.data)
       > ```
     
     - `api_view` decorator
       
       - DRF view 함수에서는 필수로 작성되며 view 함수를 실행하기 전 HTTP 메서드를 확인
       
       - 기본적으로 GET 메서드만 허용되며 다른 메서드 요청에 대해서는 `405 Method Not Allowed`로 응답
       
       - DRF view 함수가 응답해야 하는 HTTP 메서드 목록을 작성

- GET method - Detail
  
  : 단일 게시글 데이터 조회하기
  
  1. 각 게시글의 상세 정보를 제공하는 `ArticleSerializer` 정의
  
  > ```python
  > # articles/serializers.py
  > 
  > class ArticleSerializer(serializers.ModelSerializer):
  >     class Meta:
  >         model = Article
  >         fields = '__all__'
  > ```
  
  2. url 및 view 함수 작성
  
  > ```python
  > # articles/urls.py
  > 
  > urlpatterns = [
  >     path('articles/<int:article_pk>/', views.article_detail),
  > ]
  > ```
  > 
  > ```python
  > # articles/views.py
  > 
  > from .serializers import ArticleListSerializer, ArticleSerializer
  > 
  > @api_view(['GET'])
  > def article_detail(request, article_pk):
  >     article = Article.objects.get(pk=article_pk)
  >     serializer = ArticleSerializer(article)
  >     return Response(serializer.data)
  > ```

- POST method - 생성
  
  : 게시글 데이터 생성하기
  
  - 데이터 생성이 성공했을 경우 `201 Created` 응답
  
  - 데이터 생성이 실패했을 경우 `400 Bad request` 응답
  1. `article_list` view 함수 구조 변경(method에 따른 분기 처리)
  
  > ```python
  > # articles/views.py
  > 
  > from rest_framework import status
  > 
  > @api_view(['GET', 'POST'])
  > def article_list(request):
  >     if request.method == 'GET':
  >         articles = Article.objects.all()
  >         serializer = ArticleListSerializer(articles, many=True)
  >         return Response(serializer.data)
  >     elif request.method == 'POST':
  >         serializer = ArticleSerializer(data=request.data)
  >         if serialier.is_valid():
  >             serializer.save()
  >             return Response(serializer.data, status=status.HTTP_201_CREATED)
  >         return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
  > ```
  
  2. 응답 확인
  - POST 응답 확인 : http://127.0.0.1:8000/api/v1/articles/
  - GET 새로 생성된 게시글 확인 : http://127.0.0.1:8000/api/v1/articles/21/

- DELETE method - 삭제
  : 게시글 데이터 삭제하기
  
  > ```python
  > # articles/views.py
  > 
  > @api_view(['GET', 'DELETE'])
  > def article_detail(request, article_pk):
  >     articles = Article.objects.get(pk=article_pk)
  >     if request.method == 'GET':
  >         serializer = ArticleSerializer(article)
  >         return Response(serializer.data)
  >     elif request.method == 'DELETE':
  >         article.delete()
  >         return Response(status=status.HTTP_204_NO_CONTENT)
  > ```
  > 
  > 2. 응답 확인
  
  - DELETE 응답 확인 : http://127.0.0.1:8000/api/v1/articles/21/

- PUT method - 수정
  : 게시글 데이터 수정하기
  
  - 요청에 대한 데이터 수정이 성공했을 경우는 `200 OK` 응답
  
  - `partial` argument
    : '부분 업데이트'를 허용하기 위한 인자
    
    - 예를 들어 `partial`인자 값이 `False`일 경우, 게시글 `title`만을 수정하려고 해도 반드시 `content`값도 요청 시 함께 전송해야 함
    
    - 기본적으로 `serializer`는 모든 필수 필드에 대한 값을 전달받기 때문
      
      - 즉, 수정하지 않는 다른 필드 데이터도 모두 전송해야하며 그렇지 않으면 유효성 검사에서 오류가 발생
        
        > ```python
        > # articles/views.py
        > 
        > @api_view(['GET', 'DELETE', 'PUT'])
        > def article_detail(request, article_pk):
        >     articles = Article.objects.get(pk=article_pk)
        >     if request.method == 'GET':
        >         serializer = ArticleSerializer(article)
        >         return Response(serializer.data)
        >     elif request.method == 'DELETE':
        >         article.delete()
        >         return Response(status=status.HTTP_204_NO_CONTENT)
        >     elif request.method == 'PUT':
        >         serializer = ArticleSerializer(article, data=request.data, partial=True)
        >         # serializer = ArticleSerializer(instance=article, data=request.data, partial=True)
        >         if serializer.is_valid():
        >             serializer.save()
        >             return Response(seralizer.data)
        >         return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
        > ```
  2. 응답 확인
  - PUT 응답 확인 : http://127.0.0.1:8000/api/v1/articles/1/
  - GET 수정된 데이터 확인 : http://127.0.0.1:8000/api/v1/articles/1/

---

## DRF with N:1 Relation

1. Comment 클래스 정의 및 데이터베이스 초기화
   
   > ```python
   > # articles/models.py
   > 
   > classs Comment(models.Model):
   >     article = models.ForeignKey(Article, on_delete=models.CASCADE)
   >     content = models.TextField()
   >     created_at = models.DateTimeField(auto_now_add=True)
   >     updated_at = models.DateTimeField(auto_now=True)
   > ```

2. Migrations 및 fixtures 데이터 로드
   
   > ```python
   > python manage.py makemigrations
   > python manage.py migrate
   > python manage.py loaddata articles.json comments.json
   > ```

3. URL 및 HTTP request method 구성
   
   | URL                    | GET      | POST  | PUT      | DELETE   |
   | ---------------------- | -------- | ----- | -------- | -------- |
   | `comments/`            | 댓글 목록 조회 |       |          |          |
   | `comments/1/`          | 단일 댓글 조회 |       | 단일 댓글 수정 | 단일 댓글 삭제 |
   | `articles/1/comments/` |          | 댓글 생성 |          |          |

4. GET method - List
   
   > 1. 댓글 목록 조회를 위한 `CommentSerializer` 정의
   >    
   >    ```python
   >    # articles/serializers.py
   >    
   >    from .models import Article, Comment
   >    
   >    class CommentSerializer(serializers.ModelSerializer):
   >        class Meta:
   >            model = Comment
   >            fields = '__all__'
   >    ```
   > 
   > 2. url 작성
   >    
   >    ```python
   >    urlpatterns = [
   >        ...,
   >        path('comments/', views.comment_list),
   >    ]
   >    ```
   > 
   > 3. view 함수 작성
   >    
   >    ```python
   >    # articles/views.py
   >    
   >    from .models import Article, Comment
   >    from .serializers import ArticleListSerializer, ArticleSerializer, CommentSerializer
   >    
   >    @api_view(['GET'])
   >    def comment_list(request):
   >        comments = Comment.objects.all()
   >        serializer = CommentSerializer(comments, many=True)
   >        return Response(serializer.data)
   >    ```

5. GET method - Detail
   
   > 1. 단일 댓글 조회를 위한 url 및 view 함수 작성
   >    
   >    ```python
   >    urlpatterns = [
   >        ...,
   >        path('comments/<int:comment_pk>/', views.comment_detail),
   >    ]
   >    ```
   >    
   >    ```python
   >    # articles/views.py
   >    
   >    @api_view(['GET'])
   >    def comment_detail(request, comment_pk):
   >        comment = Comment.objects.get(pk=comment_pk)
   >        serializer = CommentSerializer(comment)
   >        return Response(serializer.data)
   >    ```

6. POST method
   
   > 1. 단일 댓글 생성을 위한 url 작성
   >    
   >    ```python
   >    urlpatterns = [
   >        ...,
   >        path('comments/<int:article_pk>/comments/', views.comment_create),
   >    ]
   >    ```
   > 
   > 2. serializer 인스턴스의 `saver()`메서드는 특정 Serializer 인스턴스를 저장하는 과정에서 추가 데이터를 받을 수 있음
   >    
   >    ```python
   >    # articles/views.py
   >    
   >    @api_view(['POST'])
   >    def comment_create(request, article_pk):
   >        article = Article.objects.get(pk=article_pk)
   >        serializer = CommentSerializer(data=request.data)
   >        if serializer.is_valid(raiser_exception=True):
   >            serializer.save(article=article)
   >            return Response(serializer.data, status=status.HTTP_201_CREATED)
   >    ```
   > 
   > 3. 상태코드 400 응답 확인
   >    
   >    - `CommentSerializer`에서 외래 키에 해당하는 `article` field 또한 사용자로부터 입력받도록 설정되어 있기 때문에 서버 측에서는 누락되었다고 판단한 것 -> 유효성 검사 목록에서 제외 필요
   >    
   >    - `article` field를 읽기 전용 필드로 설정하기
   >    
   >    - 읽기 전용 필드(`read_only_fields`)
   >      
   >      : 데이터를 전송 받은 시점에 '유효성 검사에서 제외시키고, 데이터 조회 시에는 출력'하는 필드
   >      
   >      ```python
   >      # articles/serializers.py
   >      
   >      from .models import Article, Comment
   >      
   >      class CommentSerializer(serializers.ModelSerializer):
   >          class Meta:
   >              model = Comment
   >              fields = '__all__'
   >              read_only_fields = ('article', )
   >      ```

7. DELETE & PUT method
   
   > 1. 단일 댓글 삭제 및 수정을 위한 view 함수 작성
   >    
   >    ```python
   >    # articles/views.py
   >    
   >    @api_view(['GET', 'DELETE', 'PUT'])
   >    def comment_detail(request, comment_pk):
   >        comment = Comment.objects.get(pk=comment_pk)
   >        if request.method == 'GET':
   >            serializer = CommentSerializer(comment)
   >            return Response(serializer.data)
   >        elif request.method == 'DELETE':
   >            comment.delete()
   >            return Responser(status=status.HTTP_204_NO_CONTENT)
   >        elif request.method == 'PUT':
   >            serializer = CommentSerializer(comment, data=request.data)
   >            if serializer.is_valid(raise_exception=True):
   >                serializer.save()
   >                return Response(serailizer.data)
   >    ```
   
   ### 응답 데이터 재구성
   
   1. 댓글 조회 시 게시글 출력 내역 변경
      
      - 댓글 조회 시 게시글 번호만 제공해주는 것이 아닌 '게시글 제목'까지 제공하기
      
      - 필요한 데이터를 만들기 위한 Serializer는 내부에서 추가 선언이 가능
      
      > ```python
      > # articles/serializers.py
      > 
      > from .models import Article, Comment
      > 
      > class CommentSerializer(serializers.ModelSerializer):
      >     class ArticleTitleSerializer(serializers.ModelSerializer):
      >         class Meta:
      >             model = Article
      >             fields = ('title',)
      >     article = ArticleTitleSerializer(read_only=True)
      > 
      >     class Meta:
      >         model = Comment
      >         fields = '__all__'
      >         # read_only_fields = ('article', )
      > ```

### 역참조 데이터 구성

- Article -> Coment 간 역참조 관계를 활용한 JSON 데이터 재구성

- 2가지 사항에 대한 데이터 재구성하기
  
  - 단일 게시글 조회 시 해당 게시글에 작성된 댓글 목록도 함께 붙여서 응답
  
  - 단일 게시글 조회 시 해당 게시글에 작성된 댓글 개수도 함께 붙여서 응답

- 단일 게시글 + 댓글 목록
  
  - `Nested relationships` (역참조 매니저 활용)
    
    - 모델 관계 상으로 참조하는 대상은 참조되는 대상의 표현에 포함되거나 중첩될 수 있음
    
    - 이러한 중첩된 관계는 serializers를 필드로 사용하여 표현 가능
    
    > ```python
    > # articles/serializers.py
    > 
    > class ArticleSerializer(serializers.ModelSerializer):
    >     class CommentDetailSerializer(serializers.ModelSerializer):
    >         class Meta:
    >             model = Comment
    >             fields = ('id', 'content',)
    >     comment_set = CommentDetailSerializer(many=True, read_only=True)
    > 
    >     class Meta:
    >         model = Article
    >         fields = '__all__'
    > ```

- 단일 게시글 + 댓글 개수
  
  - 댓글 개수에 해당하는 새로운 필드 생성
    
    > ```python
    > # articles/serializers.py
    > 
    > class ArticleSerializer(serializers.ModelSerializer):
    >     class CommentDetailSerializer(serializers.ModelSerializer):
    >         class Meta:
    >             model = Comment
    >             fields = ('id', 'content',)
    >     comment_set = CommentDetailSerializer(many=True, read_only=True)
    >     comment_count = serializers.IntegerField(source='comment_set.count', read_only=True)
    > 
    >     class Meta:
    >         model = Article
    >         fields = '__all__'
    > ```
    > 
    > - `source` arguments
    >   
    >   : 필드를 채우는 데 사용할 속성의 이름
    >   
    >   - 점 표기법(dotted notation)을 사용하여 속성을 탐색할 수 있음

- 읽기 전용 필드
  
  - 읽기 전용 필드를 사용하는 이유
    
    - 사용자에게 입력으로는 받지 않지만 제공은 해야 하는 경우
    
    - 새로운 필드 값을 만들어 제공해야 하는 경우
  
  - 특징 및 주의사항
    
    - 유효성 검사에서 제외됨
    
    - 단, 유효성 검사에서 제외된다고 해서 반드시 '생성'로직에서만 사용이 국한되는 것은 아님
  
  - `read_only_fields` 속성과 `read_only` 인자의 사용처
    
    - `read_only_fields` : 기존 외래 키 필드 값을 그대로 응답 데이터에 제공하기 위해 지정하는 경우
    
    - `read_only` : 기존 외래 키 필드 값을 결과를 다른 값으로 덮어쓰는 경우. 새로운 응답 데이터 값을 제공하는 경우
  
  - !주의! 읽기 전용 필드 지정 이슈
    
    - 특정 필드를 override 혹은 추가한 경우 `read_only_fields`는 동작하지 않음
    
    - 이런 경우 새로운 필드에 `read_only` 키워드 인자로 작성해야 함
    
    - 즉, 아래와 같은 경우는 동작하지 않음
      
      ```python
      class ArticleSErializer(serializers.ModelSerializer):
          ...
          comment_set = CommentDetailSerializer(many=True)
          comment_count = serializers.IntegerField(source='comment_set.count')
      
          class Meta:
              model = Article
              fields = '__all__'
              read_only_fields = ('comment_set', 'comment_count',)
      ```

---

## API 문서화

- OAS (OpenAPI Specification)
  
  : RESTful API를 설명하고 시각화하는 표준화된 방법
  
  - API에 대한 세부사항을 기술할 수 있는 공식 표준

- Swagger, Redoc
  
  : OAS 기반 API에 대한 문서를 생성하는데 도움을 주는 오픈소스 프레임워크

- 문서화 활동
  
  - `drf-spectacular` 라이브러리
    
    : DRF 위한 OpenAPI 3.0 구조 생성을 도와주는 라이브러리
    
    - 설치 및 등록
      
      > `pip install drf-spectacular`
      > 
      > ```python
      > # settings.py
      > 
      > INSTALLED_APPS = [
      >     ...,
      >     'drf_spectacular',
      >     ...,
      > ]
      > ```
    
    - 관련 설정 코드 입력 (OpenAPI 구조 자동 생성 코드)
      
      > ```python
      > # settings.py
      > 
      > REST_FRAMEWORK = {
      >     # YOUR SETTINGS
      >     'DEFAULT_SCHEMA_CLASS' : 'drf_spectacular.openapi.AutoSchema',
      > }
      > ```
    
    - swagger, redoc 페이지 제공을 위한 url 작성
      
      > ```python
      > # drf/ursl.py
      > from drf_spectacular.views import SpectacularAPIView, SpectacularRedocView, SpectacularSwaggerView
      > 
      > urlpatterns = [
      >     ...,
      >     path('api/schema/', SpectacularAPIView.as_view(), name='schema'),
      >     path('api/schema/swagger-ui/', SpectacularSwaggerView.as_view(url_name='schema'), name='swagger-ui'),
      >     path('api/schema/redoc/', SpectacularRedocView.as_view(url_name='schema'), name='redoc'),
      > ]
      > ```
      > 
      > - `http://127.0.0.1:9000/api/schema/swagger-ui/` 페이지 확인
      > 
      > - `http://127.0.0.1:9000/api/schema/redoc/` 페이지 확인

- '설계 우선' 접근법
  
  - OAS의 핵심 이점
  
  - API를 먼저 설계하고 명세를 작성한 후, 이를 기반으로 코드를 구현하는 방식
  
  - API의 일관성을 유지하고, API 사용자는 더 쉽게 API를 이해하고 사용할 수 있음
  
  - 또한, OAS를 사용하면 API가 어떻게 작동하는지를 시각적으로 보여주는 문서를 생성할 수 있으며, 이는 API를 이해하고 테스트하는데 매우 유용
  
  - 이런 목적으로 사용되는 도구가 `Swagger-UI` 또는 `ReDoc`

---

#### cf. `raise_exception`

  : `is_valid()`의 선택 인자

- 유효성 검사를 통과하지 못할 경우 `ValidatinoError` 예외를 발생시킴

- DRF에서 제공하는 기본 예외 처리기에 의해 자동으로 처리되며 기본적으로 `HTTP 400` 응답을 반환
  
  > ```python
  > # articles/views.py
  > 
  > @api_view(['GET', 'POST'])
  > def article_list(request):
  >     if request.method == 'GET':
  >         articles = Article.objects.all()
  >         serializer = ArticleListSerializer(articles, many=True)
  >         return Response(serializer.data)
  >     elif request.method == 'POST':
  >         serializer = ArticleSerializer(data=request.data)
  >         if serialier.is_valid(raise_exception=True):
  >             serializer.save()
  >             return Response(serializer.data, status=status.HTTP_201_CREATED)
  >         # return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
  > ```

#### cf. 올바르게 404 응답하기

- Django shortcuts functions
  
  - `render()`
  
  - `redirect()`
  
  - `get_object_or_404()`
    
    : 모델 manager objects에서 `get()`을 호출하지만, 해당 객체가 없을 땐 기존 `DoesNotExist`(상태 코드 500) 예외 대신 `HTTP404`를 `raise`함
    
    - `from django.shortcuts import get_object_or_404`
    
    - `Article.objects.get(pk=article_pk)` 형태의 코드를 모두 `get_object_or_404(Article, pk=article_pk)` 형태로 변경
  
  - `get_list_or_404()`
    
    : 모델 manager objects에서 `filter()`의 결과를 반환하고, 해당 객체 목록이 없을 땐 `HTTP404`를 `raise`함
    
    - `from django.shortcuts import get_list_or_404`
    
    - `Article.objects.all()` 형태의 코드를 모두 `get_list_or_404(Article)` 형태로 변경
  
  - `get_~` 을 사용하는 이유
    
    - 클라이언트에게 '서버에 오류가 발생하여 요청을 수행할 수 없다(500)'라는 원인이 정확하지 않은 에러를 제공하기보다는, 적절한 예외 처리를 통해 클라이언트에게 보다 정확한 에러 현황을 전달하는 것도 매우 중요한 개발 요소 중 하나이기 때문에

#### cf. 복잡한 ORM 활용 시 권장 방식

- 복잡한 query나 로직은 View 함수에서 진행
  
  - 여러 모델을 조인하거나 복잡한 집계가 필요한 경우 View 함수에서 처리
  
  - 필요한 경우 View 함수에서 `select_related()`나 `prefetch_related()`를 사용해 query를 최적화

- Serializer는 기본적인 데이터 변환을 담당
  
  - Serializer만으로는 복잡한 query를 처리하기 어려움
