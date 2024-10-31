# Many to one relationships N:1 or 1:N

: 한 테이블의 0개 이상의 레코드와 다른 테이블의 레코드 한 개와 관련도니 관계

- #### ForeignKey(to, on_delete)
  
  : 한 모델이 다른 모델을 참조하는 관계를 설정하는 필드
  
  - N:1 관계 표현
  
  - 데이터베이스에서 외래 키로 구현
  
  - `to` : 참조하는 모델 class 이름
  
  - `on_delete` : 외래 키가 참조하는 객체(1)가 사라졌을 때, 외래 키를 가진 객체(N)를 어떻게 처리할 지 정의하는 설정 (데이터 무결성)
    
    - `CASCADE` : 참조 된 객체(부모 객체)가 삭제될 때 이를 참조하는 모든 객체도 삭제되도록 지정
    
    - [참고문서](https://docs.djangoproject.com/en/4.2/ref/models/fields/#arguments)

- 댓글 모델 정의
  
  - `ForeignKey` 클래스의 인스턴스 이름은 참조하는 모델 클래스 이름의 단수형으로 작성하는 것을 원장
  
  - 외래 키는 `ForeignKey` 클래스를 작성하는 위치와 관계없이 테이블의 마지막 필드로 생성됨
  
  > ```sql
  > # articles/models.py
  > 
  > class Comment(models.Model):
  >     article = models.ForeignKey(Article, on_delete=models.CASCADE)
  >     content = models.CharField(max_length=200)
  >     created_at = models.DateTimeField(auto_now_add=True)
  >     updated_at = models.DateTimeField(auto_now=True)
  > ```

- Migration 이후 댓글 테이블 확인
  
  - 만들어지는 필드 이름 : `참조대상 클래스 이름 + _ + 클래스 이름`
  
  - 참조하는 클래스 이름의 소문자(단수형)로 작성하는 것이 권장되었던 이유

- 댓글 생성
  
  1. shell_plus 실행 및 게시글 작성
     
     ```git
     # shell_plus 실행
     python manage.py shell_plus
     
     # 게시글 생성
     Article.objects.create(title='title', content='content')
     ```
  
  2. 댓글 생성
     
     ```git
     # Comment 클래스의 인스턴스 comment 생성
     comment = Comment()
     
     # 인스턴스 변수 저장
     comment.content = 'first comment'
     
     # DB에 댓글 저장
     comment.save()
     
     # 에러 발생
     django.db.utils.IntegrityError: NOT NULL constraint failed:
     articles_comment.article_id
     # ariticles_comment 테이블의 ForeignKeyFirld, article_id 값이 저장 시 누락되었기 때문
     ```
  
  3. 외래 키 입력
     
     ```git
     # 게시글 조회
     article = Article.objects.get(pk=1)
     
     # 외래 키 데이터 입력
     comment.article = article
     # 또는 comment.article_id = article.pk 표현으로
     # pk 값을 직접 외래 키 컬럼에 넣어줄 수도 있지만 권장하지 않음
     
     # 댓글 저장 및 확인
     comment.save()
     ```
  
  4. comment 인스턴스를 통한 article 값 참조하기
     
     ```git
     comment.pk
     => 1
     
     comment.content
     => 'first comment'
     
     # 클래스 변수명인 article로 조회 시 해당 참조하는 게시글 객체를 조회할 수 있음
     comment.article
     => <Article: Article object (1)>
     
     # article_pk는 존재하지 않는 필드이기 때문에 사용 불가
     comment.article_id
     => 1
     
     # 1번 댓글이 작성된 게시물의 pk 조회
     comment.article.pk
     => 1
     
     # 1번 댓글이 작성된 게시물의 content 조회
     => 'content'
     ```
  
  5. 두번째 댓글 생성
     
     ```git
     comment = Comment(content='second comment', article=article)
     comment.save()
     
     comment.pk
     => 2
     
     comment
     => <Comment: Comment object (2)>
     
     comment.article.pk
     => 1
     ```

---

## 역참조

: N:1 관계에서 1에서 N을 참조하거나 조회하는 것 (1 -> N)

- 모델 간의 관계에서 관계를 정의한 모델이 아닌, 관계의 대상이 되는 모델에서 연결된 객체들에 접근하는 방식

- N은 외래 키를 가지고 있어 물리적으로 참조가 가능하지만, 1은 N에 대한 참조 방법이 존재하지 않아 별도의 역참조 키워드가 필요

- `모델 인스턴스.역참조 이름(related manager).QuerySet API` 형식
  
  - ex) `article.comment_set.all()` : 특정 게시글에 작성된 댓글 전체를 조회하는 요청

- related manager
  
  : N:1 혹은 M:N 관계에서 역참조 시에 사용하는 매니저
  
  - `objects` 매니저를 통해 QuerySet API를 사용했던 것처럼 related manager를 통해 QuerySet API를 사용할 수 있게 됨
  
  - 이름 규칙
    
    - N:1 관계에서 생성되는 realated manager의 이름은 `모델명_set` 형태로 자동 생성됨. 관계를 직접 정의하지 않은 모델에서 연결된 객체들을 조회할 수 있게 함
    
    - 특정 댓글의 게시글 참조(Comment -> Article) : `comment.article`
    
    - 특정 게시글의 댓글 목록 참조(Article -> Comment) : `article.comment_set.all()`

---

## 댓글 구현

### Create

1. 사용자로부터 댓글 데이터를 입력 받기 위한 `CommentForm` 정의
   
   ```python
   # articles/forms.py
   
   from .models import Article, Comment
   
   class CommentForm(forms.ModelForm):
       class Meta:
           model = Comment
           fields = ('content',)
   ```

2. detail view 함수에서 `CommentForm`을 사용해 detail 페이지에 렌더링
   
   ```python
   # articles/views.py
   form .forms import ArticleForm, CommentForm
   
   def detail(request, pk):
       article = Article.objects.get(pk=pk)
       comment_form = CommentForm()
       context = {
           'article' : article,
           'comment_form' : comment_form,
       }
       return render(request, 'articles/detail.html', context)
   ```
   
   ```html
    <!-- articles/detail.html -->
   
    <form action='{% url "articles:comments_create" article.pk %}'' method='POST'>
        {% csrf_token %}
        {{ comment_form }}
          <input type='submit'>
    </form>
   ```

3. 댓글의 외래 키 데이터에 필요한 정보 detail페이지의 URL에서 받아오기  
   
   ```python
   # articles/urls.py
   
   urlpatterns = [
       ...,
       path('<int:pk>/comments/', views.comments_create, name='comments_create'),
   ]
   ```

4. `comments_create` view 함수 정의 -> url로 받은 pk 인자를 게시글을 조회하는데 사용
   
   - `save`의 `commit` 인자를 활용해 외래 키 데이터 추가 입력
   
   - `save(commit=False)` : DB에 저장 요청을 보내지 않고 인스턴스만 반환
   
   ```python
   # articles/views.py
   
   def comments_create(request, pk):
       article = Article.objects.get(pk=pk)
       comment_form = CommentForm(request.POST)
       if comment_form.is_valid():
           comment = comment_form.save(commit=False)
           comment.article = article
           comment.save()
           return redirect('articles:detail', aritlce.pk)
       context = {
           'article' : article,
           'comment_form' : comment_form,
       }
       return render(request, 'articles/detail.html', context)
   ```

### Read

1. detail view 함수에서 전체 댓글 데이터 조회
   
   ```python
   # articles/views.py
   form .models import Article, Comment
   
   def detail(request, pk):
       article = Article.objects.get(pk=pk)
       comment_form = CommentForm()
       comments = article.comment_set.all()
       context = {
           'article' : article,
           'comment_form' : comment_form,
           'comments' : comments,
       }
       return render(request, 'articles/detail.html', context)
   ```

2. 전체 댓글 출력
   
   ```html
   <!-- articles/detail.html -->
   <h4>댓글 목록</h4>
   <ul>
     {% for comment in comments %}
       <li>{{ comment.content }}</li>
     {% endfor %}
   </ul>
   ```

### Delete

1. 댓글 삭제 url 작성
   
   ```python
   # articles/urls.py
   
   app_name = 'articles'
   urlpatterns = [
       ...,
       path('<int:article_pk>/comments/<int:comment_pk>/delete/', views.comments_delete, name='comments_delete'),
   ]
   ```

2. 댓글 삭제 view 함수 정의
   
   ```python
   # articles/views.py
   from .models import Article, Comment
   
   def comments_delete(request, article_pk, comment_pk):
       comment = Comment.objects.get(pk=comment_pk)
       comment.delete()
       return redirect('articles:detail', article_pk)
   ```

3. 댓글 삭제 버튼 작성
   
   ```html
   <!-- articles/detail.html -->
   <ul>
     {% for comment in comments %}
       <li>
         {{ comment.content }}
            <form action="{% url 'articles:comments_delete' article.pk comment.pk %}" method="POST">
           {% csrf_token %}
           <input type="submit" value="DELETE">
         </form>
       </li>
     {% endfor %}
   </ul>
   ```

---

## Article & User

1. User 외래 키 정의
   
   ```python
   # articles/models.py
   from django.conf import settings
   
   class Article(models.Model):
       user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
       title = models.CharField(max_length=10)
       content = models.TextField()
       created_at = models.DateTimeField(auto_now_add=True)
       updated_at = models.DateTimeFirld(auto_now=True)
   ```
   
   - User 모델을 참조하는 2가지 방법
     
     - User모델은 직접 참조하기 않음
     
     |       | `get_user_model()`      | `settings.AUTH_USER_MODEL` |
     | ----- | ----------------------- | -------------------------- |
     | 반환 값  | User Object(객체)         | accounts.User(문자열)         |
     | 사용 위치 | models.py 가 아닌 다른 모든 위치 | models.py                  |

2. Migration
   
   - 기존에 테이블이 있는 상황에서 필드를 추가하려하기 때문에 발생하는 과정
   
   - 기본적으로 모든 필드에는 `NOT NULL~ 제약 조건이 있기 때문에 데이터가 없이는 새로운 필드가 추가되지 못함
   
   - `1`을 입력하고 Enter 진행
   
   - 추가하는 외래 키 필드에 어떤 데이터를 넣은 것인지 직접 입력해야 함
   
   - 마찬가지로 `1` 입력하고 Enter 진행(기존에 작성된 게시글이 있다면 모두 1번 회원이 작성한 것으로 처리됨)

---

## 게시글 구현

### Create

1. `ArticleForm` 출력 필드 수정
   
   - User 모델에 대한 외래 키 데이터 입력을 위해 불필요한 input이 출력되기 때문에 
   
   ```python
   # articles/forms.py
   
   class ArticleForm(forms.ModelForm):
       class Meta:
           model = Article
           fields = ('title', 'content',)
   ```

2. `user_id` 필드 데이터가 누락되었기 때문에 게시글 작성 시 에러 발생

3. 게시글 작성 시 작성자 정보가 함께 저장될 수 있도록 `save`의 `commit` 옵션 활용
   
   ```python
   # articles/view.py
   
   @login_required
   def created(request):
       if request.method == 'POST':
           form = ArticleForm(request.POST)
           if form.is_valid():
               article = form.save(commit=False)
               article.user = request.user
               article.save()
               return redirect('articles:detail', article.pk)
       else:
           ...
   ```

### Read

1. 각 게시글의 작성자 이름 출력
   
   ```html
   <!-- articles/index.html -->
   
   {% for article in articles %}
       <p>작성자 : {{ article.user }}</p>
       <p>글 번호 : {{ article.pk }}</p>
       <a href="{% url 'articles:detail' article.pk %}">
         <p>글 제목 : {{ article.title }}</p>
       </a>
       <p>글 내용 : {{ article.content }}</p>
       <hr>
   {% endfor %}
   ```
   
   ### Update
- 게시글 수정 요청 사용자와 게시글 작성 사용자를 비교하여 본인의 게시글만 수정할 수 있도록 하기

---

#### cf. 데이터 무결성

    : 데이터베이스에 저장된 데이터의 정확성, 일관성, 유효성을 유지하는 것

    : 데이터베이스에 저장된 데이터 값의 정확성을 보장하는 것

- 정확성
  
  - 데이터의 신뢰성 확보
  
  - 시스템 안정성
  
  - 보안 강화

#### cf. 댓글이 없는 경우 대체 콘텐츠 출력

- DTL의 `for empty` 태그 활용

> ```html
> <!-- articles/detail.html -->
> <ul>
>   {% for comment in comments %}
>     <li>
>       {{ comment.content }}
>          <form action="{% url 'articles:comments_delete' article.pk comment.pk %}" method="POST">
>         {% csrf_token %}
>         <input type="submit" value="DELETE">
>       </form>
>     </li>
>   {% empty %}
>     <p>댓글이 없어요..<
>   {% endfor %}
> </ul>
> ```

#### cf. 댓글 개수 출력하기

- DTL filter - `length` 사용
  
  > ```html
  > {{ comments|length }}
  > {{ article.comment_set.all|length }}
  > ```

- QuerySet API - `count()` 사용
  
  > ```html
  > {{ article.comment_set.count }}
  > ```
