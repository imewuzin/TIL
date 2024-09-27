# Form

: 사용자 입력 데이터를 수집하고, 처리 및 유효성 검사를 수행하기 위한 도구

- 유효성 검사를 단순화하고 자동화할 수 있는 기능 제공

- 유효성 검사
  
  : 수집한 데이터가 정확하고 유효한지 확인하는 과정
  
  - 입력 값, 형식, 중복, 범위, 보안 등 많은 것들을 고려해야 하기 때문에 구현이 어려움
  
  - 이런 과정과 기능을 직접 개발하는 것이 아닌  Django가 제공하는 Form을 사용
  
  - `is_valid()` : 여러 유효성 검사를 실행하고, 데이터가 유효한지 여부를 `Boolean`으로 반환
  
  - 공백 데이터가 유효하지 않은 이유 : 별도로 명시하지 않았지만 모델 필드에는 기본적으로 빈 값은 허용하지 않는 제약조건이 설정되어 있음

- Form class 정의
  
  > ```python
  > # articles/forms.py
  > 
  > from django import forms
  > 
  > class ArticleForm(forms.Form):
  >     title = forms.CharField(max_length=10)
  >     content = forms.CharField()  # 모델의 TextField 존재X
  > ```
  > 
  > - view 함수 `new` 변경
  > 
  > ```python
  > # articles/views.py
  > 
  > from .forms import ArticleForm
  > 
  > def new(request):
  >     form = ArticleForm()
  >     context = {
  >         'form' : form,
  >     }
  >     return render(request, 'articles/new.html', context)
  > ```
  > 
  > - new 페이지에서 form 인스턴스 출력
  > 
  > ```html
  > <!-- articles/new.html -->
  > 
  > <h1>NEW</h1> 
  > <form action="{% url 'articles:create' %}" method="POST">
  >     {% csrf_token %}
  >     {{ form }}
  >     <input type="submit">
  > </form>
  > ```

- Form rendering options
  
  - `form.as_p` : label, input 쌍을 특정 HTML 태그로 감싸는 옵션

- #### Widgets
  
  : HTML `input` element의 표현을 담당. 단순히 `input` 요소의 속성 및 출력되는 부분을 변경하는 것
  
  - [참고자료](httpS://docs.djangoproject.com/ko/4.2/ref/forms/widgets/#built-in-widgets)
  
  > ```python
  > # ariticles/form.py
  > 
  > from django import forms
  > 
  > class ArticleForm(forms.Form):
  >     title = forms.CharField(max_length=10)
  >     content = forms.CharField(widget=froms.Textarea)
  > ```
  
  - 응용
    
    > ```python
    > # articles/forms.py
    > 
    > class ArticleForm(forms.ModelForm):
    >     title = forms.CharField(
    >         label = '제목',
    >           widget = forms.TextInput(
    >               attrs={
    >                   'class' : 'my-title',
    >                   'placeholder' : 'Enter the title',
    >                     'maxlength' : 10,
    >                }
    >             ),
    >      )
    >     content = forms.CharField(
    >         label = '내용',
    >           widget = forms.TextInput(
    >               attrs={
    >                   'class' : 'my-title',
    >                   'placeholder' : 'Enter the content',
    >                     'rows' : 5,
    >                     'cols' : 50,
    >                }
    >             ),
    >               error_messages = {'required' : '내용을 입력해주세요.'},
    >      )    
    > 
    >     class Meta:
    >           model = Article
    >           fields = '__all__'
    > ```

## Django Model Form

: Model과 연결된 Form을 자동으로 생성해주는 기능을 제공(`Form` + `Model`)

- Form : 사용자 입력 데이터를 DB에 저장하지 않을 때 (ex. 검색, 로그인 등)

- ModelForm : 사용자 입력 데이터를 DB에 저장해야할 때 (ex. 게시글 작성, 회원가입)

- ModelForm class 정의
  
  > ```python
  > # articles/forms.py
  > 
  > from django import forms
  > from .models import Article
  > 
  > class ArticleForm(forms.ModelForm):
  >     class Meta:
  >         model = Article
  >         fields = '__all__'
  > ```

- #### Meta Class
  
  : ModelForm의 정보를 작성하는 곳
  
  - 속성
    
    - `fields`
    
    - `exclude` : 모델에서 포함하지 않을 필드 지정 가능
      
      - `fields` 위치에 ex) `exclude = ('title',)
  
  - 주의사항
    
    - Django에서 ModelForm에 대한 추가 정보나 속성을 작성하는 클래스 구조를 Meta 클래스로 작성했을 뿐이며, 파이썬의 inner class와 같은 문법적인 관점으로 접근하지 말 것
  
  - ModelForm 적용한 create 로직
    
    > ```python
    > # articles/views.py
    > 
    > from .forms import ArticleForm
    > 
    > def create(request):
    >     form = ArticleForm(request.POST)
    >     if form.is_valid():  # 유효성 검사
    >         article = form.save()
    >         return redirect('articles:detail', article.pk)
    >     context = {
    >         'form' : form,
    >     }
    >     return render(request, 'articles/new.html', context)
    > ```
  
  - ModelForm을 적용한 edit 로직
    
    > ```python
    > # articles/views.py
    > 
    > def edit(request, pk):
    >     article = Article.Objects.get(pk=pk)
    >      form = ArticleForm(instance=article)
    >     context = {
    >           'article' : article,
    >         'form' : form,
    >     }
    >     return render(request, 'articles/edit.html', context)
    > ```
    > 
    > ```html
    > <!-- articles/edit.html -->
    > 
    > <h1>EDIT</h1> 
    > <form action="{% url 'articles:update' article.pk %}" method="POST">
    >     {% csrf_token %}
    >     {{ form.as_p }}
    >     <input type="submit">
    > </form>
    > ```
  
  - 메서드
    
    - `save()` 
      : 데이터베이스 객체를 만들고 저장하는 ModelForm의 인스턴스 메서드
      
      - 키워드 인자 `instance` 여부를 통해 생성할 지, 수정할 지를 결정 
        
        > ```python
        > # CREATE
        > 
        > form = ArticleForm(request.POST)
        > form.save()
        > 
        > # UPDATE
        > 
        > form = ArticleForm(request.POST, instance=article)
        > form.save()
        > ```

---

## HTTP 요청 다루기

- `new` & `create` 의 `view` 함수 간 공통점과 차이점
  
  - 공통점 : 데이터 생성을 구현하기 위함
  
  - 차이점 : `new`는 `GET` methond 요청만을 처리, `create`는 `POST` method 요청만을 처리
  
  - HTTP request method 차이점을 활용해 동일한 목적을 가지는 2개의 `view` 함수를 하나로 구조화

- `new` & `create` 함수 결합
  
  - PUT, DELETE 등 다른 메서드도 존재하기 때문에 `method == 'GET'`으로 구분하지 않고 `else`문 사용
  
  > ```python
  > def create(request):
  >     if request.method == 'POST':  # 기존 create 함수 부분
  >         form = ArticleForm(request.POST)
  >         if form.is_valid():
  >             article = form.save()
  >             return redirect('article:detail', article.pk)
  >     else:  # 기존 new 부분
  >         form = ArticleForm()
  >     context = {   # 공통 부분
  >         'form' : form,
  >     }
  >     return render(request, 'article/new.html', context)
  > ```

- `edit` & `update` 함수 결합
  
  > ```python
  > def update(request, pk):
  >     article = Article.objects.get(pk=pk)
  >     if request.method == 'POST':  # 기존 update 함수 부분
  >         form = ArticleForm(request.POST, instance=article)
  >         if form.is_valid():
  >             form.save()
  >             return redirect('article:detail', article.pk)
  >     else:  # 기존 edit 부분
  >         form = ArticleForm(instance=article)
  >     context = {   # 공통 부분
  >         'article' : article,
  >         'form' : form,
  >     }
  >     return render(request, 'article/update.html', context)
  > ```

---

### cf. ModelFrom의 키워드 인자 구성
