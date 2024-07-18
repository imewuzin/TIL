# 모듈

: 한 파일로 묶인 변수와 함수의 모음. 특정한 기능을 하는 코드가 작성된 파이썬 파일 `.py`

## 모듈 활용

- ### 가져오는 방법
  
  - `import`문 사용
    
    : 모듈 이름까지 명시
    
    - 추천. `from`방법은 내가 만든 함수인지 모듈 함수인지, 어떤 함수인지 알기 힘듦.
    
    > ```python
    > import math
    > print(math.sqrt(4)
    > ```
  
  - `from`절 활용
    
    : 모듈 이름 명시 X
    
    > ```python
    > from math import sqrt
    > print(sqrt(4))
    > ```

- ### 사용하기
  
  - `. (dot)` 연산자
    
    : 점의 왼쪽 객체에서 점의 오른쪽 이름을 찾으라는 의미
    
    > ```python
    > # 모듈명.변수명
    > print(math.pi)
    > #모듈명.함수명
    > print(math(sqrt(4))
    > ```

- ### 주의사항
  
  - 서로 다른 모듈이 같은 이름의 함수를 제공할 경우 문제 발생
  - 마지막에 `import`된 이름으로 대체됨
  
  > ```python
  > from math import pi, sqrt
  > from my_math import sqrt
  > # my_math의 sqrt만 쓸 수 있음
  > 
  > # 모듈 내 모든 요소를 한번에 import하는 * 표기 권장 X
  > ```

- ### `as` 키워드
  
  : `as` 키워드를 사용해 별칭(alias) 부여
  
  - 두 개 이상의 모듈에서 동일한 이름의 변수, 함수, 클래스 등을 가져올 때 발생하는 이름 충돌 해결
    
    > ```python
    > from math import sqrt
    > from my_math import sqrt as my_sqrt
    > ```

- ### 사용자 정의 모듈
  
  1. 모듈 작성
  
  2. 불러오길 바라는 파일에서 `import` 후 원하는 함수 호출

---

## 파이썬 표준 라이프러리 (PSL)

: 파이썬 언어와 함께 제공되는 다양한 모듈과 패키지의 모음

- 모듈 모여서 패키지, 패키지들이 모이면 라이브러리

- [파이썬 표준 라이브러리]([파이썬 표준 라이브러리 &#8212; Python 3.12.4 문서](https://docs.python.org/ko/3/library/index.html))

### 패키지

  : 연관된 모듈들을 하나의 디렉토리에 모아놓은 것

- 사용 목적
  
  - 모듈들의 이름공간을 구분하여 충돌을 방지
  
  - 모듈들을 효율적으로 관리하고 재사용할 수 있도록 돕는 역할

- 사용하는 방법
  
  > `my_package` 폴더 속 `math` 폴더와 `statistics` 폴더
  > 
  > `math` 폴더 속 `my_math.py` 파일, `statistics` 폴더 속 `tools.py` 파일
  > 
  > - 패키지 : `my_package`, `math`, `statistics`
  > 
  > - 모듈 : `my_math`, `tools`
  > 
  > ```python
  > # sample.py
  > from my_package.math import my_math
  > from my_package.stastics import tools
  > 
  > 
  > print(my_math.add(1, 2))
  > print(tools.mod(1, 2))  
  > ```

- #### PSL 내부 패키지
  
  : 설치 없이 바로 `import`하여 사용

- #### 외부 패키지
  
  : `pip`를 사용하여 설치 후 `import` 필요

- #### pip 파이썬 패키지 관리자
  
  - 외부 패키지들을 설치하도록 도와주는 파이썬 패키지 관리 시스템 (인터넷 필요)
  
  - [PyPl (Python Package Index)](https://pypi.org/) 에 저장된 외부 패키지들을 설치

- #### 설치
  
  - *global*에 설치됨. 실제로는 프로젝트끼리 서로 영향을 주면 안되기 때문에 global에 잘 설치하지 않음. 가상 환경 내 설치함. (아직 가상환경을 모르므로 global에 설치됨)
  
  - 최신 버전 / 특정 버전 / 최소 버전을 명시하여 설치할 수 있음 (최신 버전이 무조건 좋은 것은 아님)
    
    > ```python
    > $ pip install SomePackage
    > $ pip install Somepackage===1.0.5
    > $ pip install SomePackage>=1.0.4
    > ```
  
  - <mark>`request` 외부 패키지</mark>
    
    > 외부에서 응답을 받아 사용하는 패키지
    > 
    > 응답은 `dictionary` 형식
    > 
    > Chrome 확장 프로그램 `JSON Formatter' 설치하면 보기 편함
    > 
    > ```python
    > # 설치
    > $ pip install requests
    > import requests
    > 
    > 
    > url = 'https://random-data-api.com/api/v2/users'
    > response = requests.get(url).json() 
    > # requests.get(url)로 요청 보내고 응답을 받은 뒤,
    > # json()이 응답을 dictionary 형식으로 바꿔줌
    > 
    > print(response)
    > ```

---

## 참고

- 내장 함수 `help`
  
  : 모듈에 무엇이 들어있는지 확인 가능
  
  > ```python
  > help(math)
  > ```
