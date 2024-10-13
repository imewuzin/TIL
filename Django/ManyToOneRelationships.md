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
     
     # 외래 키 데이터 입
     ```
