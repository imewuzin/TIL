# Many to many relationships

: 한 테이블의 0개 이상의 레코드가 다른 테이블의 0개 이상의 레코드와 관련된 경우

- 양 쪽 모두에서 N:1 관계를 가짐

- M:N 관계 주요 사항
  
  - M:N 관계로 맺어진 두 테이블에는 물리적인 변화가 없음
  
  - `ManyToManyField`는 중개 테이블을 자동으로 생성
  
  - `ManyToManyField`는 M:N 관계를 맺는 두 모델 어디에 위치해도 상관 없음
    
    - 대신 필드 작성 위치에 따라 참조와 역참조 방향을 주의할 것
  
  - N:1은 완전한 종속의 관계였지만, M:N은 종속적인 관계가 아니며 '의사에게 진찰받는 환자 & 환자를 진찰하는 의사' 이렇게 2가지 형태 모두 표현 가능

- 의사 - 환자 예시로 모델 생성 연습
  
  1. 예약 모델 생성
     
     - 환자 모델의 외래 키를 삭제하고 별도의 예약 모델을 새로 생성
     
     - 예약 모델은 의사와 환자에 각각 N:1 관계를 가짐
     
     > ```python
     > # hospitals/models.py
     > 
     > # 외래 키 삭제
     > clas Patient(models.Model):
     >     name = models.TextField()
     >     def __str__(self):
     >         return f'{self.pk}번 환자 {self.name}'
     > 
     > # 중개 모델 작성
     > class Reservation(models.Model):
     >     doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
     >     patient = models.ForeignKey(Patiend, on_delete=models.CASCADE)
     >     def __str__(self):
     >         return f'{self.doctor_id}번 의사의 {self.patiend_id}번 환자'
     > ```
  
  2. 예약 데이터 생성
     
     - 데이터베이스 초기화 후 Migrations 진행 및 shell_plus 실행
     
     - 의사와 환자 생성 후 예약 만들기
     
     > ```python
     > doctor1 = Doctor.objects.create(name='allie')
     > patiend1 = Patient.objects.create(name='carol')
     > 
     > Reservation.objects.create(doctor=doctor1, patient=patient1)
     > ```
  
  3. 예약 정보 조회
     
     - 의사와 환자가 예약 모델을 통해 각각 본인의 진료 내역 확인
     
     > ```python
     > # 의사 -> 예약 정보 찾기
     > doctor1.reservation_set.all()
     > <QuerySet [<Reservation: 1번 의사의 1번 환자>]>
     > 
     > # 환자 -> 예약 정보 찾기
     > patiend1.reservation_set.all()
     > <QuerySet [<Reservation: 1번 의사의 1번 환자>]>
     > ```
  
  4. 추가 예약 생성
     
     - 1번 의사에게 새로운 환자 예약 생성
     
     > ```python
     > patient2 = Patiend.objects.create(name='duke')
     > 
     > Reservation.objects.create(doctor=doctor1, patient=patiend2)
     > ```
  
  5. 예약 정보 조회
     
     - 1번 의사의 예약 정보 조회
     
     > ```python
     > # 의사 > 환자 목록
     > doctor1.reservation_set.all()
     > <QuerySet [<Reservation: 1번 의사의 1번 환자>, <Reservation: 1번 의사의 2번 환자>]>
     > ```

---

## `ManyToManyField()`

: M:N 관계 설정 모델 필드

- 의사 - 환자 예시로 모델 생성 연습
  
  1. 환자 모델에 `ManyToManyField` 작성
     
     - 의사 모델에 작성해도 상관없으며 참조/역참조 관계만 잘 기억할 것
     
     > ```python
     > # hospitals/models.py
     > 
     > class Patient(models.Model):
     >     # ManyToManyField 작성
     >     doctors = models.ManyToManyField(Doctor)
     >     name = models.TextField()
     > 
     >     def __str__(self):
     >         return f'{self.pk}번 환자 {self.name}'
     > ```
  
  2. 의사 1명과 환자 2명 생성
     
     > ```python
     > doctor1 = Doctor.objects.create(name='allie')
     > patiend1 = Patient.objects.create(name='carol')
     > patient2 = Patiend.objects.create(name='duke')
     > ```
  
  3. 환자가 예약 생성
     
     > ```python
     > # patient1이 doctor1에게 예약
     > patient1.doctors.add(doctor1)
     > 
     > # patient1 - 자신이 예약한 의사 목록 확인
     > patient1.doctors.all()
     > <QuerySet [<Doctor: 1번 의사 allie>]>
     > 
     > # doctor 1 - 자신의 예약된 환자 목록 확인
     > doctor1.patient_set.all()
     > <QuerySet [<Patient: 1번 환자 carol>]>
     > ```
  
  4. 의사가 예약 생성
     
     > ```python
     > # doctor1이 patient2에게 예약
     > doctor1.patiend_set.add(patient2)
     > 
     > # doctor 1 - 자신의 예약된 환자 목록 확인
     > doctor1.patient_set.all()
     > <QuerySet [<Patient: 1번 환자 carol>, <Patient: 2번 환자 duke>]>
     > 
     > # patient1 - 자신이 예약한 의사 목록 확인
     > patient1.doctors.all()
     > <QuerySet [<Doctor: 1번 의사 allie>]>
     > 
     > # patient2 - 자신이 예약한 의사 목록 확인
     > patient2.doctors.all()
     > <QuerySet [<Doctor: 1번 의사 allie>]>
     > ```
  
  5. 예약 취소하기
     
     - 이전에는 Reservation을 찾아서 지워야 했다면, 이제는 `.remove()`로 삭제 가능
     
     > ```python
     > # doctor1이 patient1 진료 예약 취소
     > doctor1.patient_set.remove(patient1)
     > ```
     > 
     > ```python
     > # patient2가 doctor2 진료 예약 취소
     > patient2.doctors.remove(doctor1)
     > ```

---

## `through` argument

: 중개 테이블에 '추가 데이터'를 사용해 M:N 관계를 형성하려는 경우에 사용

- 의사 - 환자 예시로 모델 생성 연습
  
  1. `Reservation` Class 재작성 및 `through` 설정
     
     - 이제는 예약 정보에 '증상'과 '예약일'이라는 추가 데이터가 생김
     
     > ```python
     > # hospitals/models.py
     > 
     > clas Patient(models.Model):
     >     doctors = models.ManyToManyField(Doctor, through='Reservation')
     >     name = models.TextField()
     >     def __str__(self):
     >         return f'{self.pk}번 환자 {self.name}'
     > 
     > # 중개 모델 작성
     > class Reservation(models.Model):
     >     doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
     >     patient = models.ForeignKey(Patiend, on_delete=models.CASCADE)
     >     symptom = models.TextField()
     >     reserved_at = models.DateTimeField(auto_now_add=True)
     > 
     >     def __str__(self):
     >         return f'{self.doctor_id}번 의사의 {self.patiend_id}번 환자'
     > ```
  
  2. 의사 1명과 환자 2명 생성
     
     > ```python
     > doctor1 = Doctor.objects.create(name='allie')
     > patiend1 = Patient.objects.create(name='carol')
     > patient2 = Patiend.objects.create(name='duke')
     > ```
  
  3. Reservation class를 통한 예약 생성
     
     > ```python
     > reservation1 = Reservation(doctor=doctor1, patient=patient1, sysmtom='headache')
     > reservation1.save()
     > ```
  
  4. Patient 또는 Doctor의 인스턴스를 통한 예약 생성(`through_defaults`)
     
     > ```python
     > patient2.doctors.add(doctor1, through_defaults={'symptom':'flu'})
     > ```
  
  5. 예약 취소하기
     
     - 생성과 마찬가지로 의사와 환자 모두 각각 예약 삭제 가능
     
     > ```python
     > # doctor1이 patient1 진료 예약 취소
     > doctor1.patient_set.remove(patient1)
     > ```
     > 
     > ```python
     > # patient2가 doctor2 진료 예약 취소
     > patient2.doctors.remove(doctor1)
     > ```

---

## `ManyToManyField(to, **options)`

: M:N 관계 설정 시 사용하는 모델 필드

- `ManyToManyField` 특징
  
  - 양방향 관계 : 어느 모델에서든 관련 객체에 접근할 수 있음
  
  - 중복 방지 : 동일한 관계는 한 번만 저장됨

- `ManyToManyField`의 대표 인자 3가지
  
  - `related_name`
    
    : 역참조시 사용하는 manager name을 변경
    
    > ```python
    > class Patient(models.Model):
    >     doctors = models.ManyToManyField(Doctor, related_name='patients')
    >     name = models.TextField()
    > 
    > # 변경 전
    > doctor.patient_set.all()
    > # 변경 후 (변경 후 이전 manager name은 사용 불가)
    > doctor.patients.all()
    > ```
  
  - `symmetrical`
    
    : 관계 설정 시 대칭 유무 설정
    
    - `ManyToManyField`가 동일한 모델을 가리키는 정의에서만 사용
    
    - 기본 값 : `True`
      
      - `True`일 경우
        
        - source 모델의 인스턴스가 target 모델의 인스턴스를 참조하면 자동으로 target 모델 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 함(대칭)
        
        - 즉, 내가 당신의 친구라면 자동으로 당신도 내 친구가 됨
        
        - source 모델 : 관계를 시작하는 모델
        
        - target 모델 : 관계의 대상이 되는 모델
      
      - `False`일 경우
        
        - `True`와 반대(대칭되지 않음)
    
    > ```python
    > # 예시
    > 
    > class Person(models.Model):
    >     friends = models.ManyToManyField('self')
    >     # friends = models.ManyToManyField('self', symmetrical=False)
    > ```
  
  - `through`
    
    : 사용하고자 하는 중개모델을 지정
    
    - 일반적으로 '추가 데이터를 M:N 관계와 연결하려는 경우'에 사용

- M:N 에서의 대표 조작 methods
  
  - `add()`
    
    : 관계 추가. 지정된 객체를 관련 객체 집합에 추가
  
  - `remove()`
    
    : 관계 제거. 관련 객체 집합에서 지정도니 모델 객체를 제거

---

## 좋아요 기능 구현

1. 모델 관계 설정
   
   - Article(M) - User(N) : 0개 이상의 게시글은 0명 이상의 회원과 관련
   
   - Article 클래스에 ManyToManyField 작성
   
   > ```python
   > # articles/models.py
   > 
   > class Article(models.Model):
   >     user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
   >     like_users = models.ManyToManyField(settings.AUTH_USER_MODEL)
   >     title = models.CharField(max_length=10)
   >     content = models.TextField()
   >     created_at = models.DateTimeField(auto_now_add=True)
   >     updated_at = models.DateTimeField(auto_now=True)
   > ```

2. Migration 진행 후 에러 발생 - 역참조 매니저 충돌했기 때문에
   
   - `like_users` 필드 생성 시 자동으로 역참조 매니저 `.article_set`가 생성됨
   
   - 그러나 이전 N:1(Article-User) 관계에서 이미 같은 이름의 매니저를 사용 중
     
     - `user.article_set.all()` : 해당 유저가 작성한 모든 게시글 조회
   
   - 'user가 작성한 글(`user.article_set`)'과 'user가 좋아요를 누른 글(``user.article_set`)'을 구분할 수 없게 된
   
   - user 와 관계된 ForeignKey 혹은 ManyToManyField 둘 중 하나에 `related_name` 작성 필요

3. 모델 관계 설정 - `related_name` 작성 후 Migration 재진행
   
   > ```python
   > # articles/models.py
   > 
   > class Article(models.Model):
   >     user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
   >     like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
   >     title = models.CharField(max_length=10)
   >     content = models.TextField()
   >     created_at = models.DateTimeField(auto_now_add=True)
   >     updated_at = models.DateTimeField(auto_now=True)
   > ```
   
   - User-Article간 사용 가능한 전체 related manager
     - `article.user` : 게시글을 작성한 유저. N:1
     - `user.article_set` : 유저가 작성한 게시글(역참조). N:1
     - `article.like_users` : 게시글을 좋아요한 유저. M:N
     - `user.like_articles` : 유저가 좋아요한 게시글(역참조). M:N

4. 기능 구현
   
   1. url 작성
      
      > ```python
      > # articles/urls.py
      > 
      > urlpatterns = [
      >     ...
      >     path('<int:article_pk>/likes/', views.likes, name='likes'),
      > ]
      > ```
   
   2. view 함수 작성
      
      > ```python
      > # articles/views.py
      > 
      > @login_required
      > def likes(request, article_pk):
      >     article = Article.objects.get(pk=article_pk)
      >     if request.user in article.like_users.all():
      >         article.like_users.remove(request.user)
      >     else:
      >         article.like_users.add(request.user)
      >     return redirect('articles:index')
      > ```
   
   3. index 템플릿에서 각 게시글에 좋아요 버튼 출력
      
      > ```html
      > <!-- articles/index.html -->
      > 
      > {% for article in articles %}
      >     ...
      >     <form action="{% url 'articles:likes' article.pk %}" method="POST">
      >         {% csrf_token %}
      >         {% if request.user in article.like_users.all %}
      >             <input type="submit" value="좋아요 취소">
      >         {% else %}
      >             <input type="submit" value="좋아요">
      >         {% endif %}
      >     </form>
      >     <hr>
      > {% endfor %}
      > ```

---

## 프로필 페이지 구현

1. url 작성
   
   > ```python
   > # accounts/urls.py
   > 
   > urlpatterns = [
   >     ...
   >     path('profile/<username>/', views.profile, name='profile'),
   > ]
   > ```

2. view 함수 작성
   
   > ```python
   > # accounts/views.py
   > from django.contrib.auth import get_user_model
   > 
   > def profile(request, username):
   >     User = get_user_model()
   >     person = User.objects.get(username=username)
   >     context = {
   >         'person' : person,
   >     }
   >     return render(request, 'accounts/profile.html', context)
   > ```

3. profile 템플릿 작성
   
   > ```html
   > <!-- accounts/profile.html -->
   > 
   > <h1>{{ person.username }}님의 프로필</h1>
   > <hr>
   > <h2>{{ person.username }}가 작성한 게시글</h2>
   > {% for article in person.article_set.all %}
   >   <div>{{ article.title }}</div>
   > {% endfor %}
   > <hr>
   > ...
   > <h2>{{person.username }}가 작성한 댓글</h2>
   > {% for comment in person.comment_set.all %}
   >   <div>{{ comment.content }}</div>
   > {% endfor %}
   > <hr>
   > <h2>{{person.username }}가 좋아요한 게시글</h2>
   > {% for article in person.like_articles.all %}
   >   <div>{{ article.title }}</div>
   > {% endfor %}
   > ```

4. 프로필 페이지로 이동할 수 있는 링크 작성
   
   > ```html
   > <!-- articles/index.html -->
   > 
   > <a href="{% url 'accounts:profile' user.username %}">내 프로필</a>
   > <p>작성자 : <a href="{% url 'accounts:profile' article.user.username %}">{{ article.user }}</a></p>
   > ```

---

## 팔로우 기능 구현

1. 모델 관계 설정
   
   - User(M) - User(N) : 0명 이상의 회원은 0명 이상의 회원과 관련
   
   - `ManyToManyField` 작성
     
     - 참조 : 내가 팔로우하는 사람들(팔로잉, `followings`)
     
     - 역참조 : 상대방 입장에서 나는 팔로워 중 한 명(팔로워, `followers`)
   
   > ```python
   > # accounts/models.py
   > 
   > class User(AbstractUser):
   >     followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
   > ```

2. Migration 

3. url 작성
   
   > ```python
   > # accounts/urls.py
   > 
   > urlpatterns = [
   >     ...
   >     path('<int:user_pk>/follow/', views.follow, name='follow'),
   > ]
   > ```
   
   4. view 함수 작성
   
   > ```python
   > # accounts/views.py
   > 
   > @login_required
   > def follow(request, user_pk):
   >     User = get_user_model()
   >     person = User.objects.get(pk=user_pk)
   >     if person != request.user:
   >         if request.user in person.followers.all():
   >             person.followers.remove(request.user)
   >         else:
   >             person.followers.add(request.user)
   >     return redirect('accounts:profile', person.username)
   > ```
   
   5. 프로필 유저의 팔로잉, 팔로워 수 & 팔로우, 언팔로우 버튼 작성
   
   > ```html
   > <!-- accounts/profile.html -->
   > 
   > <div>
   >   <div>
   >     팔로잉 : {{ person.followings.all|length }} / 팔로워 : {{ person.followers.all|length }}
   >   </div>
   >   {% if request.user != person %}
   >     <div>
   >       <form action="{% url 'accounts:follow' person.pk %}" method="POST">
   >         {% csrf_token %}
   >         {% if request.user in person.followers.all %}
   >             <input type="submit" value="Unfollow">
   >         {% else %}
   >             <input type="submit" value="Follow">
   >         {% endif %}
   >       </form>
   >     </div>
   >     {% endif %}
   > </div>
   > ```

---

## Fixtures

: Django가 데이터 베이스로 가져오는 방법을 알고 있는 데이터 모음

- 데이터는 데이터베이스 구조에 맞추어 작성되어 있음

- Fixtures 사용 목적 : 초기 데이터 제공

- 초기 데이터의 필요성
  
  - 프로젝트의 앱을 처음 설정할 때 동일하게 준비된 데이터로 데이터베이스를 미리 채우는 것이 필요한 순간이 있음
  
  - Django에서는 fixtures를 사용해 앱에 초기 데이터(initial data)를 제공

- fixtures 관련 명령어
  
  - `dumpdata` 
    
    : 생성(데이터베이스의 모든 데이터 추출)
    
    - **Fixtures 파일을 직접 만들지 말고 반드시 `dumpdata` 명령어를 사용해 생성할 것**
    
    - `python manage.py dumpdata [app_name[.ModelName] [app_name[.ModelName] ... }} > filename.json`
    
    - 예시
      
      > ```python
      > python manage.py dumpdata --indent 4 articles.article > articles.json
      > ```
  
  - `loaddata`
    
    : 로드(Fixtures 데이터를 데이터베이스로 불러오기)
    
    - Fixtures 파일 기본 경로 : `app_name / fixtures`
      
      - Django는 설치된 모든 app의 디렉토리에서 fixtures 폴더 이후의 경로로 fixtures 파일을 찾아 load
    
    - 순서 주의사항
      
      - 만일 `loaddata`를 한번에 실행하지 않고 별도로 실행한다면 모델 관계에 따라 load 순서가 중요할 수 있음
        
        - `comment`는 `article`에 대한 key 및 `user`에 대한 key가 필요
        
        - `article`은 `user`에 대한 key가 필요
        
        - 즉, 현재 모델 관계에서는 `user` -> ` article` -> `comment` 순으로 data를 load해야 오류가 발생하지 않음
    
    - 예시
      
      > ```git
      > # 해당 위치로 fixture 팡리 이동
      > articles/
      >   fixtures/
      >     articles.json
      >     users.json
      >     comments.json
      > ```
      > 
      > - load 진행 후 데이터가 잘 입력되었는지 확인
      > 
      > ```python
      > python manage.py loaddata users.json articles.json comments.json
      > ```

---

## Improve query

: query 개선하기. 같은 결과를 얻기 위해 DB측에 보내는 query 개수를 점차 줄여 조회하기

- `annotate`
  
  - SQL의 `GROUP BY`를 사용
  
  - 쿼리셋의 각 객체에 계산된 필드 추가
  
  - 집계 함수(`Count`, `Sum` 등)와 함께 자주 사용됨
  
  - 예시 - `Book.objects.annotate(num_authors=Count('authors'))`
    
    > - 의미
    >   
    >   - 결과 객체에 `num_authors`라는 새로운 필드를 추가
    >   
    >   - 이 필드는 각 책과 연관된 저자의 수를 계산
    > 
    > - 결과
    >   
    >   - 결과에는 기존 필드와 함께 `num_authors`필드를 가지게 됨
    >   
    >   - `book.num_authors`로 해당 책의 저자 수에 접근할 수 있게 됨
  
  - 예시 - 게시글 조회하면서 댓글 개수까지 한번에 조회해서 가져오기
    
    > - 11 queries including 10 similar -> 1 query
    >   
    >   ```python
    >   # views.py
    >   ```
    > 
    > def index_1(request):
    > 
    >     # articles = Article.objects.order_by('-pk')
    >     articles = Article.objects.annotate(Count('comment')).order_by('-pk')
    >     context = {
    >         'articles' : articls,
    >     }
    >     return render(request, 'articles/index_1.html', context)
    > 
    > ```
    > ```html
    > <!-- index_1.html -->
    > 
    > <p>댓글개수 : {{ article.comment__count }}</p>
    > ```

- `select_related`
  
  - SQL의 `INNER JOIN`를 사용
  
  - 1:1 또는 N:1 참조 관계에서 사용
    
    - ForeignKey나 OneToOneField관계에 대해 JOIN을 수행
  
  - 단일 쿼리로 관련 객체를 함께 가져와 성능을 향상
  
  - 예시 - `Book.objects.select_related('publisher')`
    
    > - 의미
    >   
    >   - Book 모델과 연관된 Publisher 모델의 데이터를 함께 가져옴
    >   
    >   - ForiegnKey 관계인 'publisher'를 JOIN하여 단일 쿼리만으로 데이터를 조회
    > 
    > - 결과
    >   
    >   - Book 객체를 조회할 때 연관된 Publisher 정보도 함께 로드
    >   
    >   - `book.publisher.name`과 같은 접근이 추가적인 데이터베이스 쿼리 없이 가능
  
  - 예시 - 11 queries including 10 similar and 8 duplicates
    
    > - 문제 원인
    >   - 각 게시글마다 작성한 유저명까지 반복 평가
    > 
    > ```html
    > <!-- index_2.html -->
    > 
    > {% for article in articles %}
    >   <h3>작성자 : {{ article.user.username }}</h3>
    >   <p>제목 : {{ article.title }}</p>
    >   <hr>
    > {% endfor %}
    > ```
    > 
    > - 문제 해결 : 게시글을 조회하면서 유저 정보까지 한번에 조회해서 가져오기
    >   
    >   - 11 queries including 10 similar and 8 duplicates -> 1 query
    > 
    > ```python
    > # views.py
    > 
    > def index_2(request):
    >     # articles = Article.objects.order_by('-pk')
    >     articles = Article.objects.select_related('user').order_by('-pk')
    >     context = {
    >         'articles' : articls,
    >     }
    >     return render(request, 'articles/index_2.html', context)
    > ```

- `prefetch_related`
  
  - SQL이 아닌 Python을 사용한 JOIN을 진행
    
    - 관련 객체들을 미리 가져와 메모리에 저장하여 성능을 향상
  
  - M:N 또는 N:1 역참조 관계에서 사용
    
    - ManyToManyField나 역참조 관계에 대해 별도의 쿼리를 실행
  
  - 예시 - `Book.objects.prefetch_related('authors')`
    
    > - 의미
    >   
    >   - Book과 Author는 ManyToMany 관계로 가정
    >   
    >   - Book 모델과 연관된 모든 Author 모델의 데이터를 미리 가져옴
    >   
    >   - Django가 별도의 쿼리로 Author 데이터를 가져와 관계 설정
    > 
    > - 결과
    >   
    >   - Book 객체들을 조회한 후, 연관된 모든 Author 정보가 미리 로드됨
    >   
    >   - `for author in book.authors.all()`와 같은 반복이 추가적인 데이터베이스 쿼리 없이 실행됨
  
  - 예시 - 11 queries including 10 similar 
    
    > - 문제 원인
    >   - 각 게시글 출력 후 각 게시글의 댓글 목록까지 개별적으로 모두 평가
    > 
    > ```html
    > <!-- index_3.html -->
    > 
    > {% for article in articles %}
    >   <p>제목 : {{ article.title }}</p>
    >   <p>댓글 목록</p>
    >   {% for comment in article.comment_set.all %}
    >     <p>{{ comment.content }}</p>
    >   {% endfor %}
    >   <hr>
    > {% endfor %}
    > ```
    > 
    > - 문제 해결 - 게시글을 조회하면서 참조된 댓글까지 한번에 조회해서 가져오기
    >   - 11 queries including 10 similar -> 2 queries
    > 
    > ```python
    > # views.py
    > 
    > def index_3(request):
    >     # articles = Article.objects.order_by('-pk')
    >     articles = Article.objects.prefetch_related('comment_set').order_by('-pk')
    >     context = {
    >         'articles' : articls,
    >     }
    >     return render(request, 'articles/index_2.html', context)
    > ```

- `select_related` & `prefetch_related`
  
  - 예시 - 111 queries including 110 similar and 100 duplicates
    
    > - 문제 원인
    >   
    >   - '게시글' + '각 게시글의 댓글 목록' + '댓글의 작성자'를 단계적으로 평가
    > 
    > ```html
    > <!-- index_4.html -->
    > 
    > {% for article in articles %}
    >   <p>제목 : {{ article.title }}</p>
    >   <p>댓글 목록</p>
    >   {% for comment in article.comment_set.all %}
    >     <p>{{ comment.user.username }} : {{ comment.content }}</p>
    >   {% endfor %}
    >   <hr>
    > {% endfor %}
    > ```
    > 
    > - 문제 해결 1단계
    >   
    >   - 게시글을 조회하면서 참조된 댓글까지 한번에 조회
    >   
    >   - 111 queries including 110 similar and 100 duplicates -> 102 queries including 100 silimar and 100 duplicates (아직 각 댓글을 조회하면서 각 댓글의 작성자를 중복 조회 중)
    > 
    > ```python
    > # views.py
    > 
    > def index_4(request):
    >     # articles = Article.objects.order_by('-pk')
    >     articles = Article.objects.prefetch_related('comment_set').order_by('-pk')
    >     context = {
    >         'articles' : articls,
    >     }
    >     return render(request, 'articles/index_2.html', context)
    > ```
    > 
    > - 문제 해결 2단계
    >   
    >   - '게시글' + '각 게시글의 댓글 목록' + '댓글의 작성자'를 한번에 조회
    >   
    >   - 102 queries including 100 silimar and 100 duplicates -> 2queries
    > 
    > ```python
    > # views.py
    > 
    > def index_4(request):
    >     # articles = Article.objects.order_by('-pk')
    >     # articles = Article.objects.prefetch_related('comment_set').order_by('-pk')
    >     articles = Article.objects.prefetch_related(
    >         Prefetch('comment_set', queryset=Comment.objects.select_related('uesr'))
    >     ).order_by('-pk')
    >     context = {
    >         'articles' : articls,
    >     }
    >     return render(request, 'articles/index_2.html', context)
    > ```

---

### cf. `exists` method

    : QuerySet에 결과가 하나 이상 존재하는지 여부를 확인하는 메서드

- 결과가 포함되어 있으면 `True`를 반환하고, 결과가 포함되어있지 않으면 `False`를 반환

- 특징
  
  - 데이터베이스에 최소한의 쿼리만 실행하여 효율적
  
  - 전체 QuerySet을 평가하지 않고 결과의 존재 여부만 확인
    
    - 대량의 QuerySet에 있는 특정 객체 검색에 유용

- 적용 예시
  
  > - 적용 전
  >   
  >   ```python
  >   # articles/views.py
  >   
  >   @login_required
  >   def likes(request, article_pk):
  >       article = Article.objects.get(pk=article_pk)
  >       if request.user in article.like_users.all():
  >           article.like_users.remove(request.user)
  >       else:
  >           article.like_users.add(request.user)
  >       return redirect('articles:index')
  >   ```
  > 
  > - 적용 후
  >   
  >   ```python
  >   # articles/views.py
  >   
  >   @login_required
  >   def likes(request, article_pk):
  >       article = Article.objects.get(pk=article_pk)
  >       if article.like_users.filter(pk=request.user.pk).exists():
  >           article.like_users.remove(request.user)
  >       else:
  >           article.like_users.add(request.user)
  >       return redirect('articles:index')
  >   ```

### cf. 모든 모델을 한꺼번에 dump 하기

- ```python
  # 3개의 모델을 하나의 json 파일로
  python manage.py dumpdata --indent 4 articles.article articles.comment accounts.user > data.json
  
  # 모든 모델을 하나의 json 파일로
  python manage.py dumpdata --indent 4 > data.json
  ```

### cf. `loaddata` 시 encoding codec 관련 에러가 발생하는 경우

- 2가지 방법 중 택 1
  
  - `dumpdata`시 추가 옵션 작성
    
    - `python -Xutf8 manage.py dumpdata [생략]`
  
  - 메모장 활용
    
    1. 메모장으로 `json` 파일 열기
    
    2. '다른 이름으로 저장' 클릭
    
    3. 인코딩을 `UTF8`로 선택 후 저장
