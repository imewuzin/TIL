# Static files

[Static files](#static-files)

[Media files](#media-files)

[참고](#참고)

---

## Static files

정적 파일

서버 측에서 변경되지 않고 고정적으로 제공되는 파일(이미지, JS, CSS 파일 등)

- **<mark>기본 경로</mark>** : app폴더/static/
  
  1. `articles/static/articles/` 경로에 이미지 파일 배치\
  
  2. static files 경로는 DTL의 `static tag`를 사용해야 
     
     built-in tag가 아니기 때문에 `load tag`를 사용해 import 후 사용 가능
  
  3. STATIC_URL 확인
     
     > 기본 경로 및 추가 경로에 위치한 정적 파일을 참조하기 위한 URL
     > 
     > - 실제 파일이나 디렉토리 경로가 아니며, URL로만 존재
     > 
     > - URL + STATIC_URL + 정적파일 경로

- **<mark>추가 경로</mark>**
  
  STATICFILES_DIRS에 문자열 값으로 추가 경로 설정
  
  > 정적 파일의 기본 경로 외에 추가적인 경로 목록을 정의하는 리스트
  
  1. 임의의 추가 경로 설정
  
  2. 추가 경로에 이미지 파일 배치
  
  3. `static tag`를 사용해 이미지 파일에 대한 경로 제공
  
  4. 이미지를 제공 받기 위해 요청하는 Request URL 확인

- 정적 파일을 제공하려면 요청에 응답하기 위한 URL이 필요

---

## Media files

사용자가 웹에서 업로드하는 정적 파일(user-uploaded)

- **`ImageField()`**
  
  이미지 업로드에 사용하는 모델 필드
  
  - 이미지 객체가 직접 DB에 저장되는 것이 아닌 '이미지 파일의 경로' 문자열이 저장

- **<mark>미디어 파일을 제공하기 전 준비사항</mark>**
  
  1. `setting.py`에 MEDIA_ROOT, MEDIA_URL 설정
  
  2. 작성한 MEDIA_ROOT와 MEDIA_URL 에 대한 URL 지정
     
     > - **`MEDIA_ROOT`**: 미디어 파일들이 위치하는 디렉토리의 절대 경로
     >   
     >    MEDIA_URL을 통해 참조하는 파일의 실제 위치 == `setting.MEDIA_ROOT`
     > 
     > - **`MEDIA_URL`** : MEDIA_ROOT에서 제공되는 미디어 파일에 대한 주소를 생성 (STATIC_URL과 동일한 역할)
     >   
     >   업로드 된 파일의 URL == `setting.MEDIA_URL`

- **<mark>이미지 업로드</mark>**
  
  1. `blank=True` 속성을 작성해 빈 문자열이 저장될 수 있도록 제약 조건 설정
     
     - 게시글 작성 시 이미지 업로드 없이도 작성할 수 있도록 하기 위함
  
  2. migration 진행 (`ImageField`를 사용하려면 반드시 Pillow 라이브러리가 필요)
     
     ```bash
     pip install pillow
     
     python manage.py makemigrations
     python manage.py migrate
     
     pip freeze > requirements.txt
     ```
  
  3. `form`요소의 `enctype` 속성 추가
  
  4. `ModelForm`의 2번째 인자로 요청 받은 파일 데이터 작성
     
     - `ModelForm`의 상위 클래스 `BaseModelForm`의 생성자 함수의 2번째 위치 인자로 파일을 받도록 설정되어 있음
  
  5. 이미지 업로드 input 확인
  
  6. 이미지 업로드 결과 확인 (DB에는 파일 자체가 아닌 '파일 경로'가 저장)

- **<mark>업로드 이미지 제공</mark>**
  
  1. `url`속성을 통해 업로드 파일의 경로 값을 얻을 수 있음
     
     > - `article.image.url` : 업로드 파일의 경로
     > 
     > - `article.image` : 업로드 파일의 파일 이름
  
  2. 업로드 이미지 출력 확인 및 MEDIA_URL 확인
  
  3. 이미지를 업로드하지 않은 게시물은 detail 템플릿을 렌더링 할 수 없음
     
     이미지 데이터가 있는 경우만 이미지를 출력할 수 있도록 처리하기
     
     ```html
     {% if article.image %}
       <img src="{{ article.image.url }}" alt="img">
     {% endif %}
     ```

- **<mark>업로드 이미지 수정</mark>**
  
  1. 수정 페이지 `form` 요소에 `enctype` 속성 추가
  
  2. `update view` 함수에서 업로드 파일에 대한 추가 코드 작성

---

## 참고

- 미디어 파일 추가 경로
  
  `upload_to` argument
  
   : `ImageField()`의 `upload_to` 속성을 사용해 다양한 추가 경로 설정

- BaseModelForm
  
  `request.FILES`가 두 번째 위치 인자인 이유
  
   : `ModelForm`의 상위 클래스 `BaseModelForm`의 생성자 함수 키워드 인자 참고

---
