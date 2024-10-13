# Authentication System

[Cookie & Session](#cookie--session)

[Django Authentication System](#djagno-authentication-system)

[Custom User model](#custom-user-model)

[Login](#login)

[Logout](#logout)

[Template with Authentication data](#template-with-authentication-data)

---

## Cookie & Session

- **HTTP**
  
  : HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 규약
  
  >  웹(WWW)에서 이루어지는 모든 데이터 교환의 기초
  
  - 특징
    
    1. 비 연결 지향(connectionless)
       
       서버는 요청에 대한 응답을 보낸 후 연결을 끊음
    
    2. 무상태(stateless)
       
       연결을 끊는 순간 클라이언트와 서버 간의 통신이 끝나며 상태 정보가 유지되지 않음
       
       > 상태가 없다는 것은
       > 
       > - 장바구니에 담은 상품을 유지할 수 없음
       > 
       > - 로그인 상태를 유지할 수 없음

- **쿠키(Cookie)**
  
  : 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각
  
  > 서버가 제공하여 클라이언트 측에서 저장되는 작은 데이터 파일
  > 
  > 사용자 인증, 추적, 상태 유지 등에 사용되는 데이터 저장 방식
  
  - **동작 예시**
    
    1. 브라우저가 웹 서버에 웹 페이지를 요청
    
    2. 웹 서버는 요청된 페이지와 함께 쿠기를 포함한 응답을 브라우저에게 전송
    
    3. 브라우저는 받은 쿠키를 저장소에 저장, 쿠키의 속성(만료 시간, 도메인, 주소 등)도 함께 저장됨
    
    4. 이후 브라우저가 같은 웹 서버에 웹 페이지를 요청할 때, 저장된 쿠키 중 해당 요청에 적용 가능한 쿠키를 포함하여 함께 전송
    
    5. 웹 서버는 받은 쿠키 정보를 확인하고, 필요에 따라 사용자 식별, 세션 관리 등을 수행
    
    6. 웹 서버는 요청에 대한 응답을 보내며, 필요한 경우 새로운 쿠키를 설정하거나 기존 쿠키를 수정할 수 있음
  
  - **작동 원리와 활용**
    
    1. 쿠키 저장 방식
       
       - 브라우저(클라이언트)는 쿠키를 KEY-VALUE의 데이터 형식으로 저장
       
       - 쿠키에는 이름, 값 외에도 만료 시간, 도메인, 경로 등의 추가 속성이 포함됨
    
    2. 쿠키 전송 과정
       
       - 서버는 HTTP 응답 헤더의 Set-Cookie 필드를 통해 클라이언트에게 쿠키를 전송
       
       - 브라우저는 받은 쿠키를 저장해 두었다가, 동일한 서버에 재요청 시 HTTP 요청 Header의 Cookie 필드에 저장된 쿠키를 함께 전송
    
    3. 쿠키의 주요 용도
       
       - 두 요청이 동일한 브라우저에게 들어왔는지 아닌지를 판단할 때 주로 사용
       
       - 이를 이용해 사용자의 로그인 상태를 유지할 수 있음
       
       - 상태가 없는(stateless) HTTP 프로토콜에서 상태 정보를 기억시켜주는 역할
       
       > 서버에게 "나 로그인된 사용자야!"라는 인증 정보가 담긴 쿠키를 매 요청마다 계속 보내느 것
  
  - **쿠키 사용 목적**
    
    1. 세션 관리 (Session management)
       
       로그인, 아이디 자동완성, 공지 하루 안 보기, 팝업 체크, 장바구니 등 정보 관리
    
    2. 개인화 (Personalization)
       
       사용자 선호 설정(언어 설정, 테마 등) 저장
    
    3. 트래킹 (Tracking)
       
       사용자 행동을 기록 및 분석

- **세션 (Session)**
  
  : 서버 측에서 생성되어 클라이언트와 서버 간의 상태를 유지
  
  : 상태 정보를 저장하는 데이터 저장 방식
  
  > 쿠키에 세션 데이터를 저장하여 매 요청시마다 세션 데이터를 함께 보냄
  
  - 작동 원리
    
    1. 클라이언트가 로그인 요청 후 인증에 성공하면 서버가 session 데이터를 생성 후 저장
    
    2. 생성된 session 데이터에 인증할 수 있는 session id를 발급
    
    3. 발급한 session id를 클라이언트에게 응답 (데이터는 서버에 저장. 열쇠만 주는 것)
    
    4. 클라이언트는 응답받은 session id를 쿠키에 저장
    
    5. 클라이언트가 다시 동일한 서버에 접속하면 요청과 함께 쿠키(session id가 저장된)를 서버에 전달
    
    6. 쿠키는 요청 때마다 서버에 함게 전송되므로 서버에서 session id를 확인해 로그인 되어있다는 것을 계속해서 확인하도록 함
    
    > - 서버 측에서는 세션 데이터를 생성 후 저장하고 이 데이터에 접근할 수 있는 세션 ID를 생성
    > 
    > - 이 ID를 클라이언트 측으로 전달하고, 클라이언트는 쿠키에 이 ID를 저장
    > 
    > - 이후 클라이언트가 같은 서버에 재요청 시마다 저장해 두었던 쿠키도 요청과 함께 전송

- **<mark>쿠키와 세션의 목적</mark>**
  
  클라이언트와 서버 간의 상태 정보를 유지하고 사용자를 식별하기 위해 사용

---

## Django Authentication System

: 사용자 인증과 관련된 기능을 모아 놓은 시스템

- **Authentication (인증)**
  
  : 사용자가 자신이 누구인지 확인하는 것 (신원 확인)

- **사전 준비**
  
  - 두 번째 app `accounts` 생성 및 등록
  
  > auth와 관련된 경로나 키워드들을 django 내부적으로 accounts라는 이름으로 사용하고 있기 때문에 되도록 'accounts'로 지정하는 것을 권장

---

## Custom User model

- **기본 User Model의 한계**
  
  - 우리는 지금까지 별도의 User 클래스 정의 없이 내장된 auth 앱에 작성된 User 클래스를 사용했음
  
  - Django의 기본 User 모델은 username, password 등 제공되는 필드가 매우 제한적
  
  - 추가적인 사용자 정보가 필요하다면 이를 위해 기본 User Model을 변경하기 어려움
    
    - 별도의 설정 없이 사용할 수 있어 간편하지만, 개발자가 직접 수정하기 어려움

- **User Model 대체의 필요성**
  
  - 프로젝트의 특정 요구사항에 맞춰 사용자 모델을 확장할 수 있음
  
  - 예를 들어 이메일을 username으로 사용하거나, 다른 추가 필드를 포함시킬 수 있음

- **Custom User Model로 대체하기**
  
  1. `AbstractUser` 클래스를 상속받는 `User`클래스 작성
     
     > ```python
     > # accouts/models.py
     > from django.contrib.auth.models import AbstractUser
     > from django.db import models
     > 
     > # 커스텀 User 모델로 대체하기 위한 class
     > class User(AbstractUser):
     >     pass
     > ```
2. django 프로젝트에서 사용하는 기본 User 모델을 우리가 작성한 User 모델로 사용할 수 있도록 `AUTH_USER)MODEL` 값을 변경
   
   > ```python
   > # settings.py
   > 
   > AUTH_USER_MODEL = 'accounts.User'
   > ```

3. admin site에 대체한 User 모델 등록
   
   > - ```python
   >   # accounts.admin.py
   >   from .models import User
   >   from django.contrib.auth.admin import UserAdmin
   >   
   >   admin.site.register(User, UserAdmin)
   >   ```
   > 
   > - AUTH_USER_MODEL
   >   
   >   Django 프로젝트 User를 나타내는데 사용하는 모델을 지정하는 속성
   >   
   >   프로젝트 중간에 변경할 수 없음!!
- **프로젝트를 시작하며 반드시 User 모델을 대체해야 한다.**
  
  - Django는 새 프로젝트를 시작하는 경우 비록 기본 User 모델이 충분하더라도 커스텀 User 모델을 설정하는 것을 강력하게 권장하고 있음
  
  - 커스텀 User 모델은 기본 User 모델과 동일하게 작동하면서도 필요한 경우 나중에 맞춤 설정할 수 있기 때문
  
  > 단, User 모델 대체 작업은 프로젝트의 모든 migrations 혹은 첫 migrate를 실행하기 전에 이 작업을 마쳐야 함

---

## Login

: 로그인은 Session을 Create하는 과정

- `AuthenticationForm()`
  
  : 로그인 인증에 사용할 데이터를 입력 받는 built-in form

- `login(request, user)`
  
  : AuthenticationForm을 통해 인증된 사용자를 로그인하는 함수
  
  > ```python
  > # accounts/view.py
  > from django.contrib.auth.forms import AuthenticationForm
  > from django.contrib.auth import login as auth_login
  > 
  > def login(request):
  >     if request.method == 'POST':
  >         form = AuthenticationForm(request, request.POST)
  >         if form.is_valid():
  >             # 만약 인증된 사용자라면 로그인 진행(세션데이터 생성)
  >             auth_login(request, form.get_user())
  >             return redirect('articles:index')
  >     else:
  >         form = AuthenticationForm()
  >     context = {
  >         'form' : form,
  >     }
  >     return render(request, 'accounts/login.html', context)
  > ```

- `get_user()`
  
  : AuthenticationForm의 인스턴스 메서드
  
  > 유효성 검사를 통과했을 경우 로그인 한 사용자 객체를 반환

- **세션 데이터 확인하기**
  
  1. 로그인 후 발급받은 세션 django_session 테이블에서 확인
  
  2. 브라우저에서 확인. 개발자도구 - Appication - Cookies

- **로그인 링크 작성**
  
  - 메인 페이지에 로그인 페이지로 갈 수 있는 링크 작성

---

## Logout

: 로그아웃은 Session을 Delete하는 과정

- `logout(request)`
  
  1. DB에서 현재 요청에 대한 Session Data를 삭제
  
  2. 클라이언트의 쿠키에서도 Session Id를 삭제
  
  > ```python
  > # accounts/views.py
  > from django.contrib.auth import logout as auth_logout
  > 
  > def logout(request):
  >     auth_logout(request)
  >     return redirect('articles:index')
  > ```

---

## Template with Authentication data

템블릿에서 인증 관련 데이터를 출력하는 방법

- **현재 로그인 되어있는 유저 정보 출력하기**
  
  - user라는 context 데이터를 사용할 수 있는 이유는?
    
    - django가 미리 준비한 contet 데이터가 존재하기 때문(context processors)

- **context processors**
  
  - 템블릿이 렌더링 될 때 호출 가능한 컨텍스트 데이터 목록
  
  - 작성된 컨텍스트 데이터는 기본적으로 템플릿에서 사용 가능한 변수로 포함됨
    
    - django에서 자주 사용하느 데이터 목록을 미리 템플리셍 로드 해 둔 것

---

## 회원 가입

: User 객체를 Create 하는 과정

- `UserCreationForm()`
  
  : 회원 가입 시 사용자 입력 데이터를 받는 built-in ModelForm
  
  > ```python
  > # accounts/views.py
  > from .forms import CustomUserCreationForm
  > 
  > def signup(request):
  >     if request.methond == 'POST':
  >         form = CustomUserCreationForm(request.POST)
  >         if form.is_valid():
  >             form.save()
  >             return redirect('articles:index')
  >     else:
  >         form = CustomUserCreationForm()
  >     context = {
  >         'form' : form,
  >     }
  >     return render(request, 'accounts/signup.html', context)
  > ```
  > 
  > ```html
  > <!-- accounts/signup.html -->
  > <h1>회원가입</h1>
  > <form action="{% url 'accounts:signup' %}" method='POST'>
  >   {% csrf_token %}
  >   {{ form.as_p }}
  >   <input type="submit">
  > </form>
  > ```

- 회원가입 로직 에러
  
  - 회원가입 시도 후 에러 페이지 확인
    
    - `Manager isn't available: 'auth.User' has been swapped form 'accounts.User'`
  
  - 회원가입에 사용하는 `UserCreationForm`이 대체한 커스텀 유저 모델이 아닌 과거 Django의 기본 유저 모델로 인해 작성된 클래스이기 때문에
  
  - 커스텀 유저 모델을 사용하려면 다시 작성해야 하는 Form
    
    1. `UserCreationForm`
    
    2. `UserChangeForm`
  
  - Custom User model을 사용할 수 있도록 상속 후 일부분만 재작성
    
    > ```python
    > # accounts/forms.py
    > 
    > from django.contrib.auth import get_user_model
    > from django.contrib.auth.forms import UserCreationForm, UserChangeForm
    > 
    > class CustomUserCreationForm(UserCreationForm):
    >     class Meta(UserCreationForm.Meta):
    >         model = get_user_model()
    > 
    > class CustomUserChangeForm(UserChangeForm):
    >     class Meta(UserChangeForm.Meta):
    >         model = get_user_model()
    > ```
    > 
    > - `get_user_model()
    >   : 현재 프로젝트에서 활성화된 사용자 모델(active user model)을 반환하는 함수
  
  - User 모델을 직접 참조하지 않는 이유
    
    - `get_user_model()`을 사용해 User 모델을 참조하면 커스텀 User 모델을 자동으로 반환해주기 때문에
    
    - Django는 필수적으로 User 클래스를 직접 참조하는 대신 `get_user_model()`을 사용해 참조해야 한다고 강조하고 있음

---

## 회원 탈퇴

: User 객체를 Delete 하는 과정

- `delete()`
  
  > ```python
  > # accounts/view.py
  > 
  > def delete(request):
  >     request.user.delete()
  >     return redirect('articles:index')
  > ```
  > 
  > ```html
  > <!-- accounts/index.html -->
  > 
  > <form action="{% url 'accounts:delete' %}" method="POST">
  >   {% csrf_token %}
  >   <input type="submit" value='회원탈퇴'>
  > </form>
  > ```

---

## 회원정보 수정

: User 객체를 Update 하는 과정

- `UserChangeForm()`
  
  : 회원정보 수정 시 사용자 입력 데이터를 받는 built-in **ModelForm**
  
  > ```python
  > # accounts/view.py
  > from .forms import CustomUserChangeForm
  > 
  > def update(request):
  >     if request.method == 'POST':
  >         form = CustomUserChangeForm(request.POST, instance=request.user)
  >         if form.is_valid():
  >             form.save()
  >             return redirect('articles:index')
  >     else:
  >         form = CustomUserChangeForm(instance=request.user)
  >     context = {
  >         'form' : form,
  >     }
  >     return render(request, 'accounts/update.html', context
  > ```

- `UserChangeForm()` 사용 시 문제점
  
  - User 모델의 모든 정보들(fields)까지 모두 출력됨
  
  - 일반 사용자들이 접근해서는 안 되는 정보는 출력하지 않도록 해야 함
  
  - `CustomUserChangeForm`에서 출력 필드 다시 조정하기

- `CustomUserChangeForm` 출력 필드 재정의
  
  - [User Model의 필드 목록 확인](https://docs.djangoproject.com/en/4.2/ref/contrib/auth/)
  
  - ```python
    # accounts/forms.py
    
    class CustomUserChangeForm(UserChangeForm):
        class Meta(UserChangeForm.Meta):
            model = get_user_model()
            fields = ('first_name', 'last_name', 'email',)
    ```

---

## 비밀번호 변경

: 인증된 사용자의 Session 데이터를 Update 하는 과정

- `PasswordChangeForm()`
  
  : 비밀번호 변경 시 사용자 입력 데이터를 받는 built-in Form
  
  - django는 비밀번호 변경 페이지를 회원정보 수정 form 하단에서 별도 주소로 안내 `/user_pk/password/`
  
  > ```python
  > # accounts/view.py
  > from django.contrib.auth.forms import PasswordChangeForm
  > 
  > def change_password(request, user_pk):
  >     if request.method == 'POST':
  >         form = PasswordChangeForm(request.user, request.POST)
  >         if form.is_valid():
  >             form.save()
  >             return redirect('articles:index')
  >     else:
  >         form = PasswordChangeForm(request.user)
  >     context = {
  >         'form' : form,
  >     }
  >     return render(request, 'accounts/change_password.html', context)
  > ```

- 암호 변경 시 세션 무효화
  
  - 비밀번호가 변경되면 기존 세션과의 회원 인증 정보가 일치하지 않게 되어 버려 로그인 상태가 유지되지 못하고 로그아웃 처리됨
  
  - 비밀번호가 변경되면서 기존 세션과의 회원 인증 정보가 일치하지 않기 때문에
  
  - `update_session_auth_hash(request, user)`
    
    : 암호 변경 시 세션 무효화를 막아주는 함수
    
    - 암호가 변경되면 새로운 password의 Session Data로 기존 session을 자동으로 갱신
    
    > ```python
    > # accounts/view.py
    > from django.contrib.auth import update_session_auth_hash
    > 
    > def change_password(request, user_pk):
    >     if request.method == 'POST':
    >         form = PasswordChangeForm(request.user, request.POST)
    >         if form.is_valid():
    >             user = form.save()
    >             update_session_auth_hash(request, user)
    >             return redirect('articles:index')
    >     else:
    >         form = PasswordChangeForm(request.user)
    >     context = {
    >         'form' : form,
    >     }
    >     return render(request, 'accounts/change_password.html', context)
    > ```

---

## 로그인 사용자에 대해 접근을 제한하는 2가지 방법

1. `is_authenticated` 속성
   
   : 사용자가 인증 되었는지 여부를 알 수 있는 User model의 속성
   
   - 모든 User 인스턴스에 대해 항상 True인 읽기 전용 속성
   
   - 비인증 사용자에 대해서는 항상 False
   
   > ```html
   > <!-- articles/index.html -->
   > {% if request.user.is_authenticated %}
   >     <h3>Hello, {{ user.username }}</h3>
   >     <a href="{% url 'articles:create' %}">NEW</a>
   >     <form action="{% url 'accounts:logout' %}" method="POST">
   >         {% csrf_token %}
   >         <input type='submit' value='Logout'>
   >     </form>
   >     <form action="{% url 'accounts:delete' %}" method="POST">
   >         {% csrf_token %}
   >         <input type='submit' value='회원탈퇴'>
   >      </form>
   >      <a href="{% url 'accounts:update' %}">회원정보 수정</a>
   > {% else %}
   >      <a href="{% url 'accounts:login' %}">Login</a>
   >      <a href="{% url 'accounts:signup' %}">Signup</a>
   > {% endif %}
   > ```
   > 
   > ```python
   > # accounts/views.py
   > 
   > def login(request):
   >     if request.user.is_authenticated:
   >         return redirect('articles:index')
   >     ...
   > 
   > def signup(request):
   >     if request.user.is_authenticated:
   >         return redirect('articles:index')
   >     ...
   > ```

2. `login_required` 데코레이터
   
   : 인증된 사용자에 대해서만 view 함수를 실행시키는 데코레이터
   
   - 비인증 사용자의 경우 `/accounts/login/` 주소로 redirect시킴
   
   - 인증된 사용자만 게시글을 작성/수정/삭제 할 수 있도록 `articles/views.py`에서 `create`, `delete`, `update` 함수 위에 `@login_required` 첨부
   
   - 인증된 사용자만 로그아웃/탈퇴/수정/비밀번호 변경 할 수 있도록 `accounts/view.py`에서 `logout`, `delete`, `update`, `changed_password` 함수 상단에 `@login_required` 첨부

---

#### cf. 쿠키 종류별 Lifetime

1. Session cookie
   
   : 현재 세션이 종료되면 삭제됨. 브라우저 종료와 함께 세션 삭제

2. Persistent cookies
   
   : Expires 속성에 지정된 날짜 혹은 Max-Age 속성에 지정된 기간이 지나면 삭제

#### cf. 쿠키의 보안 장치

- 제한된 정보 : 쿠키에는 보통 중요하지 않은 정보만 저장 (ex. 사용자 ID, 세션 번호)

- 암호화 : 중요한 정보는 서버에서 암호화해서 쿠키에 저장

- 만료 시간 : 쿠키에는 만료 시간을 설정. 시간이 지나면 자동으로 삭제

- 도메인 제한 : 쿠키는 특정 웹사이트에서만 사용할 수 있도록 설정 가능

#### cf. 회원가입 후 로그인까지 이어서 진행하려면?

- 회원가입 성공한 user 객체를 이용해 login 진행
  
  > ```python
  > # accounts/views.py
  > 
  > def signup(request):
  >     if request.methond == 'POST':
  >         form = CustomUserCreationForm(request.POST)
  >         if form.is_valid():
  >             user = form.save()
  >             auth_login(request, user)
  >             return redirect('articles:index')
  >     else:
  >         form = CustomUserCreationForm()
  >     context = {
  >         'form' : form,
  >     }
  >     return render(request, 'accounts/signup.html', context)
  > ```

#### cf. 탈퇴와 함께 기존 사용자의 Session Data 삭제 방법

- 사용자 객체 삭제 이후 로그아웃 함수 호출

- 단, '탈퇴 후 로그아웃'의 순서가 바뀌면 안됨

- 먼저 로그아웃이 진행되면 해당 요청 객체 정보가 없어지기 때문에 탈퇴에 필요한 유저 정보 또한 없어지기 때문에
  
  > ```python
  > # accounts/views.py
  > 
  > def delete(request):
  >     request.user.delete()
  >     auth_logout(request)
  > ```
