# Django Model

: DB의 테이블을 정의하고 데이트를 조작할 수 있는 기능들을 제공(테이블 구조 설계하는 청사진)

- ```python
  # articles/models.py
  
  class Article(models.Model):  # models.Model은 상위 클래스
      title = models.CharField(max_length=10)
      content = models.TextField()
  ```
  
  | id  | title | content |
  | --- | ----- | ------- |
  |     |       |         |
  |     |       |         |
  
  - 위와 같은 표 생성하는 코드
  
  - `id` 필드는 django가 알아서 만들어줌
  
  - `django.db.models` 모듈의 `Model`이라는 부모 클래스를 상속 받음
  
  - `Model`은 model에 관련된 모든 코드가 이미 작성되어 있는 클래스
    
    - [참고자료](https://github.com/django/django/blob/main/django/db/models/base.py#L460) - `Model`은 2000줄짜리 클래스
  
  - 개발자는 가장 중요한 테이블 구조를 어떻게 설계할지에 대한 코드만 작성하도록 하기 위한 것 (상속을 활용한 프레임워크의 기능 제공)
  
  - 클래스 변수 명 : 테이블의 각 필드(열) 이름 - `title`, `content`
  
  - Model Field : 데이터베이스 테이블의 열(column)을 나타내는 중요한 구성 요소
    
    - 데이터의 유형과 제약 조건 정의 - `CharField(max_length=10)`, `TextField()`

---

## Model Field

: DB 테이블의 필드(열)을 정의하며, 해당 필드에 저장되는 데이터 타입(Field types)과 제약조건(Field options)을 정의

- [Model field reference | Django documentation | Django](https://docs.djangoproject.com/en/5.1/ref/models/fields/)
  
  - 오른쪽 목차 먼저 확인. 아래는 많이 쓰는 필드만 가져온 것
  
  - 우측 하단에 버전 맞춰서 확인할 것

- 구성
  
  - **Field types (필드 유형)**
    
    : 데이터베이스에 저장될 데이터의 종류를 정의 (`models` 모듈의 클래스로 정의되어 있음)
    
    - 문자열 필드
      
      - `CharField` : 제한된 길이의 문자열 저장 (필드의 최대 길이 결정하는 max_length는 필수 옵션)
      
      - `TextField` : 길이 제한이 없는 대용량 텍스트를 저장 (무한대는 아니며 사용하는 시스템에 따라 달라짐)
    
    - 숫자 필드
      
      - `IntegerField`, `FloatField`
    
    - 날짜/시간 필드
      
      - `DateField`, `TimeField`
      
      - `DateTimeField` : 옵션(auto_now - 데이터가 저장될 때마다 자동으로 현재 날짜시간을 저장, auto_now_add - 데이터가 처음 생성될 때만 자동으로 현재 날짜시간을 저장)
    
    - 파일 관련 필드
      
      - `FileField`, `ImageField`
  
  - **Field options (필드 옵션)**
    
    : 필드의 동작과 제약 조건을 정의
    
    - 주요 필드 옵션
      
      - `null` : 데이터베이스에서 NULL 값을 허용할지 여부를 결정(기본값 : False)
      
      - `blank` : form에서 빈 값을 허용할지 여부를 결정 (기본값 : False)
      
      - `default` : 필드의 기본값을 설정
    
    - 제약조건(Constraint)
      
      : 특정 규칙을 강제하기 위해 테이블의 열이나 행에 적용되는 규칙이나 제한 사항

---

## Migrations

: model 클래스의 변경사항(필드 생성, 수정, 삭제 등)을 DB에 최종 반영하는 방법

- 과정
  
  - model class(설계도 초안) -`makemigrations`-> migration 파일(최종 설계도) -`migrate`-> db.sqlite3

- 핵심 명령어
  
  - `python manage.py makemigrations`
    
    : model class를 기반으로 최종 설계도(migration) 작성
  
  - `python manage.py migrate`
    
    : 최종 설계도를 DB에 전달하여 반영

- `migrate` 후 DB 내에 생성된 테이블 확인

- 이미 생성된 테이블에 필드를 추가해야 한다면 추가 모델 필드 작성 `python manage.py makemigrations`
  
  - 이미 기존 테이블이 존재하기 때문에 필드를 추가할 때 필드의 기본값 설정이 필요
    
    1. 현재 대화 유지하면서 직접 기본 값을 입력하는 방법
       
       - 추가하는 필드의 기본 값을 입력해야 함
       
       - 아무것도 입력하지 않고 enter 누르면 Django가 제안하는 기본 값으로 설정됨
       
       - migrations 과정 종료 후, 2번째 migration 파일이 생성됨을 확인
       
       - Django는 설계도를 쌓아가면서 추후 문제가 생겼을 시 복구하거나 되돌릴 수 있도록 함
       
       - `python manage.py migrate` 한 후 테이블 필드 변화 확인
    
    2. 현재 대화에서 나간 후 `models.py`에 기본 값 관련 설정을 하는 방법
  
  => model class에 변경사항이 생겼다면(`model class` 변경), 반드시 새로운 설계도를 생성(`makemigrations`)하고, 이를 DB에 반영(`migrate`)해야 함




