## **리버스 함수**

: Django에서 URL의 이름을 역으로 찾아주는 함수

- 사용 목적 : URL을 하드코딩하지 않고 동적으로 생성하기 위해

- **기본 사용법**: `reverse('view_name')`, `reverse('app_name:view_name')`
  
  - 파라미터가 필요할 경우 `args` 로 명시해주기
    - ex) `reverse('exview:get2', args=(11 ,22 ,'hell'))`

- <mark>경로를 설정할 때는 직접 설정해주기보다 템플릿 문법과 리버스 함수 사용하는 것 추천</mark>
  
  - ex) `<a href="{% url 'exview:get2' '100' '200' 'hi' %}">get2</a>`

- 구현
  
  1. **URL 이름 설정**
     
     - URL 설정 파일 (`urls.py`)에서 각 경로에 이름 붙이기
     
     - ex) `get1`과 `get2` 경로에 이름 붙이기
       
       ```python
       from django.urls import path 
       from . import views 
       
       urlpatterns = [ 
           path('get1/', views.get1, name='get1'), 
           path('get2/<int:n1>/<int:n2>/<str:n3>/', 
           views.get2, name='get2'), 
       ]
       ```
  
  2. **리버스 함수의 동작 확인**
     
     - `reverse('get1')`은 `/get1/` URL을 반환
     
     - `reverse('get2', args=[10, 20, 'hello'])`는 `/get2/10/20/hello/` URL을 반환
       
       ```python
       print(reverse('get1')) # /get1/
       print(reverse('get2', args=[10, 20, 'hello'])) # /get2/10/20/hello/`
       ```

- **URL 리버스와 템플릿**
  
  - 템플릿에서 URL을 동적으로 생성할 때도 URL 리버스 사용 가능
  
  - 템플릿 문법에서 URL 태그를 사용해 경로를 동적으로 생성
    
    ```python
    <a href="{% url 'get1' %}">Get 1</a>
    <a href="{% url 'get2' 10 20 'hello' %}">Get 2</a>`
    ```

- **URL 리버스의 유용성**
  
  - 경로를 하드코딩하지 않고 URL 리버스를 사용하면, URL 설정이 변경되더라도 코드에서 자동으로 반영됨
  
  - 경로가 변경되면 URL 리버스가 자동으로 업데이트하여 하드코딩의 문제를 해결



- 템플릿에서는 다음과 같이 사용할 수 있습니다:
