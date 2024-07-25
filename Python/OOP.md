# 객체 지향 프로그래밍

## 절차 지향 프로그래밍 Procedural Programming

: 프로그램을 '데이터'와 '절차'로 구성하는 방식의 프로그래밍 패러다임

- 특징
  
  - '데이터'와 해당 데이터를 처리하는 '함수(절차)'가 분리되어 있으며, 함수 호출의 흐름이 중요
  
  - 코드의 순차적인 흐름과 함수 호출에 의해 프로그램이 진행
  
  - 데이터를 다시 재사용하기보다 처음부터 끝까지 실행되는 결과물이 중요한 방식

## 소프트웨어 위기 Software Crisis

: 하드웨어의 발전으로 컴퓨터 계산 용량과 문제의 복잡성이 급격히 증가합에 따라 소프트웨어에 발생한 충격

---

## 객체 지향 프로그래밍 Object Oriented Programming

: 데이터와 해당 데이터를 조작하는 메서드를 하나의 객체로 묶어 관리하는 방식의 프로그래밍 패러다임

| 절차 지향                          | 객체 지향                                         |
| ------------------------------ | --------------------------------------------- |
| - 데이터와 해당 데이터를 처리하는 함수(절차)가 분리 | - 데이터와 해당 데이터를 처리하는 메서드(메시지)를 하나의 객체(클래스)로 묶음 |
| - 함수 호출의 흐름이 중요                | - 객체 간 상호작용과 메시지 전달이 중요                       |

- ## 클래스 Class
  
  : 파이썬에서 타입을 표현하는 방법
  
  - 객체를 생성하기 위한 설계도
  
  - 데이터와 기능을 함께 묶는 방법 제공
  
  - `class` 키워드
    
    - 클래스 이름은 파스칼 케이스(Pascal Case) 방식으로 작성
  
  - 구조
    
    - 생성자 메서드
      : 객체를 생성할 때 자동으로 호출되는 특별한 메서드
      
      - `__init__` 이라는 이름의 메서드로 정의되며, 객체의 초기화 담당
      - 생성자 함수를 통해 인스턴스를 생성하고 필요한 초기값을 설정
    
    - 인스턴스 변수
      : 인스턴스(객체)마다 별도로 유지되는 변수
      
      - 인스턴스마다 독립적인 값을 가지며, 인스턴스가 생성될 때마다 초기화됨
    
    - 클래스 변수
      : 클래스 내부에 선언된 변수
      
      - 클래스로 생성된 모든 인스턴스들이 공유하는 변수
    
    - 인스턴스 메서드
      : 각각의 인스턴스에서 호출할 수 있는 메서드
      
      - 인스턴스 변수에 접근하고 수정하는 등의 작업을 수행
        
        > ```python
        > class Person:
        >     blood_color = 'red'           # 클래스 변수
        > 
        >      def __init__(self, name):   # 생성자 메서드
        >          self.name = name        # 인스턴스 변수
        >      def singing(self):           # 인스턴스 메서드
        >          return f'{self.name}가 노래합니다.'
        > 
        > # 인스턴스 생성
        > singer1 = Person('iu)
        > # 메서드 호출
        > print(singer1.singing())  # iu가 노래합니다
        > # 속성(변수) 접근
        > print(singer1.blood_color)  # red
        > ```
  
  - 활용
    
    - `class.class_variable`로 클래스 변수 참조하기
      
      > 예시 - 가수가 몇 명인지 활용하고 싶다면 인스턴스가 생성될 때마다 클래스 변수가 늘어나도록 설정 가능
      > 
      > ```python
      > class Person:
      >     count = 0
      >    def __init__(self, name):
      >        self.name = name
      >        Person.count += 1
      > 
      > person1 = Person('iu')
      > person2 = Person('BTS')
      > print(Person.count)  # 2
      > ```

- ## 객체 Object
  
  : 클래스에서 정의한 것을 토대로 메모리에 할당된 것. '속성(변수)'과 '행동(메서드)'으로 구성된 모든 것
  
  - 하나의 객체(Object)는 특정 타입의 인스턴스(instance)이다
  
  - <mark>객체(Object) = 속성(Attribute) + 기능(Method)</mark>
    
    - 타입(type) : 어떤 연산자(operator)와 조작(method)이 가능한가?
    
    - 속성(attribute) : 어떤 상태(데이터)를 가지는가?
    
    - 조작법(method) : 어떤 행위(함수)를 할 수 있는가?

- ### 인스턴스 Instance
  
  : 클래스의 속성과 행동을 기반으로 생성된 개별 객체
  
  > 클래스와 객체, 인스턴스 예시
  > 
  > - 클래스(가수) - 객체(아이유, BTS...)
  >   
  >   - 아이유는 객체다 (O)
  >   
  >   - 아이유는 인스턴스다 (X)
  >   
  >   - 아이유는 가수의 인스턴스다 (O)
  > 
  > - `name = 'Alice'`
  >   
  >   - 변수 `name`의 타입은 str 클래스이다
  >   
  >   - 변수 `name`은 str 클래스의 인스턴스이다
  >   
  >   -> 우리가 사용해왔던 **데이터 타입은 사실 모두 클래스**였다

---

## 메서드

- ### 인스턴스 메서드 Instance method
  
  : 클래스로부터 생성된 각 인스턴스에서 호출할 수 있는 메서드
  
  > ```python
  > class MyClass:
  > 
  >   def instance_method(self, arg1, ...):
  >     pass
  > ```
  
  - 인스턴스의 상태를 조작하거나 동작을 수행
  
  - 반드시 첫번째 매개변수로 인스턴스 자신(`self`)을 전달 받음
  
  - `self`는 매개변수 이름일 뿐 다른 이름으로 설정 가능하지만 다른 이름을 사용하지 않을 것을 강력히 권장
  
  - `self` 동작 원리
    
    > - upper 메서드 사용해 문자열 'hello'를 대문자로 변경하기
    >   1. `'hello`.upper()
    >   2. 실제 파이썬 내부 동작 `str.upper('hello')`
    >      -> str 클래스가 upper 메서드 호출했고, 그 첫번째 인자로 문자열 인스턴스가 들어간 것
    >      => 인스턴스 메서드의 첫번째 매개변수가 반드시 인스턴스 자기 자신인 이유
    > - `'hello'.upper()`은 `str.upper('hello')`를 객체 지향 방식의 메서드로 호출하는 표현 (단축형 호출)
    > - `'hello'`라는 문자열 객체가 단순히 어딘가의 함수로 들어가는 인자로 활용되는 것이 아닌 객체 스스로 메서드를 호출하여 코드를 동작하는 객체 지향적인 표현인 것

- ### 생성자 메서드 Constructor method
  
  : 인스턴스 객체가 생성될 때 자동으로 호출되는 메서드
  
  > ```python
  > class Peron:
  >   def __init__(self):
  >     pass
  > ```
  
  - 인스턴스 변수들의 초기값을 설정

- ### 클래스 메서드 Class method
  
  : 클래스가 호출하는 메서드
  
  > ```python
  > class MyClass:
  > 
  >   @classmethod
  >   def class_method(cls, arg1, ...):
  >     pass
  > ```
  > 
  > - `cls`는 매개변수 이름일 뿐이며 다른 이름으로 설정 가능하지만 다른 이름을 사용하지 않기를 강력히 권장
  > - 클래스 메서드 안에 클래스 변수 사용할 때 <mark>꼭 `cls` 사용</mark>
  
  - 클래스 변수를 조작하거나 클래스 레벨의 동작을 수행

- ### 정적 메서드 Static method
  
  : 클래스와 인스턴스와 상관없이 독립적으로 동작하는 메서드
  
  > ```python
  > class MyClass:
  > 
  >   @staticmethod
  >   def static_method(arg1, ...):
  >     pass
  > ```
  
  - 주로 클래스와 관련이 있지만, 인스턴스와 상호작용이 필요하지 않은 경우에 사용
  
  - `@staticmethod` 데코레이터를 사용하여 정의
  
  - 호출 시 필수적으로 작성해야 할 매개변수가 없음
  
  - 객체 상태나 클래스 상태를 수정할 수 없으며 단지 기능(행동)만을 위한 메서드로 사용
    
    > 예시 - 단순히 문자열을 조작하는 기능을 제공하는 메서드
    > 
    > ```python
    > class StringUtils:
    >   @staticmethod
    >   def reverse_string(string):
    >     return string[::-1]
    > 
    >   @staticmethod
    >   def capitalize_string(string):
    >     return string.capitalize()
    > 
    > text = 'hello, world'
    > 
    > reversed_text = StringUtils.reverse_string(text)
    > print(reversed_text)  # dlrow, olleh
    > 
    > capitalized_text = StringUtils.capitalize_string(text)
    > print(capitalized_text)  # Hello, world
    > ```

---

## 상속 Inheritance

: 기존 클래스의 속성과 메서드를 물려받아 하위 클래스를 생성하는 것

- 필요한 이유
  
  - 코드 재사용
    
    - 상속을 통해 기존 클래스의 속성과 메서드를 재사용할 수 있음
    
    - 새로운 클래스를 작성할 때 기존 클래스의 기능을 그대로 활용할 수 있으며, 중복된 코드를 줄일 수 있음
  
  - 계층 구조
    
    - 상속을 통해 클래스들 간의 계층 구조를 형성할 수 있음
    
    - 부모 클래스와 자식 클래스 간의 관계를 표현하고, 더 구체적인 클래스를 만들 수 있음
  
  - 유지 보수의 용이성
    
    - 상속을 통해 기존 클래스의 수정이 필요한 경우, 해당 클래스만 수정하면 되므로 유지 보수가 용이해짐
    
    - 코드의 일관성을 유지하고, 수정이 필요한 범위를 최소화할 수 있음
  
  - 예시
    
    > ```python
    > class Person:
    >     def __init__(self, name, age):
    >         self.name = name
    >         self.age = age
    > 
    >     def talk(self):  # 메서드 재사용
    >         print('반갑습니다. {self.name}입니다.')
    > 
    > class Professor(Person):
    >     def __init__(self, name, age, department):
    >         self.name = name
    >         self.age = age
    >         self.department = department
    > 
    > class Professor(Person):
    >     def __init__(self, name, age, gpa):
    >         self.name = name
    >         self.age = age
    >         self.gpa = gpa
    > 
    > p1 = Professor('박교수', 49, '컴퓨터공학과')
    > s1 = Student('김학생', 20, 3.5)
    > 
    > # 부모 Person 클래스의 talk 메서드를 활용
    > p1.talk() # 반갑습니다. 박교수입니다.
    > 
    > # 부모 Person 클래스의 talk 메서드를 활용
    > s1.talk() # 반갑습니다. 김학생입니다.
    > ```

- ### 다중 상속
  
  : 둘 이상의 상위 클래스로부터 여러 행동이나 특징을 상속받을 수 있는 것
  
  - 상속받은 모든 클래스의 요소를 활용 가능함
  
  - 중복도니 속성이나 메서드가 있는 경우 상속 순서에 의해 결정됨
  
  - 예시
    
    > ```py
    > class Person:
    >     def __init__(self, name):
    >         self.name = name
    > 
    >     def greeting(self):
    >         return f'안녕, {self.name}'
    > 
    > 
    > class Mom(Person):
    >     gene = 'XX'
    > 
    >     def swim(self):
    >         return '엄마가 수영'
    > 
    > 
    > class Dad(Person):
    >     gene = 'XY'
    > 
    >     def walk(self):
    >         return '아빠가 걷기'
    > 
    > 
    > class FirstChild(Dad, Mom):
    >     def swim(self):
    >         return '첫째가 수영'
    > 
    >     def cry(self):
    >         return '첫째가 응애'
    > 
    > 
    > baby1 = FirstChild('아가')
    > print(baby1.cry()) # 첫째가 응애
    > print(baby1.swim()) # 첫째가 수영
    > print(baby1.walk()) # 아빠가 걷기
    > print(baby1.gene) # XY
    > ```
    
    - 다이아몬드 문제
      : 두 클래스 B와 C가 A에서 상속되고, 클래스 D가 B와 C 모두에서 상속될 때 모호함
      
      - Q. B와 C가 재정의한 메서드가 A에 있고, D가 이를 재정의하지 않은 경우라면 D는 B의 메서드 중 어떤 버전을 상속하는가? 아니면 C의 메서드 버전을 상속하는가?
        
        > ```python
        > class D(B, C):
        >     pass
        > ```
      
      - 파이썬에서의 해결책
        
        1. <mark>MRO</mark>(Method Resulution Order) 알고리즘을 사용하여 클래스 목록을 생성
        2. 부모 클래스로부터 상속된 속성들의 검색을 **깊이 우선으로, 왼쪽에서 오른쪽으로, 계층 구조에서 겹치는 같은 클래스를 두 번 검색하지 않음**
        3. 속성이 D에서 발견되지 않으면 B에서 찾고, 거기에서도 발견되지 않으면 C에서 찾고, 이런 식으로 진행

- `super()`
  : 부모 클래스 객체를 반환하는 내장 함수
  
  - 다중 상속 시 MRO를 기반으로 현재 클래스가 상속하는 모든 부모 클래스 중 다음에 호출될 메서드를 결정하여 자동으로 호출
  
  - 사용 사례
    
    - 단일 상속 구조
      
      - 명시적으로 이름을 지정하지 않고 부모 클래스를 참조할 수 있으므로 코드를 더 유지 관리하기 쉽게 만들 수 있음
      - 클래스 이름이 변경되거나 부모 클래스가 교체되어도 `super()`를 사용하면 코드 수정이 더 적게 필요
    
    - 다중 상속 구조
      
      - MRO를 따른 메서드 호출
      - 복잡한 다중 상속 구조에서 발생할 수 있는 문제를 방지
  
  - 예시
    
    > - 사용 전
    >   
    >   ```python
    >   class Person:
    >       def __init__(self, name, age, number, email):
    >           self.name = name
    >           self.age = age
    >           self.number = number
    >           self.email = email
    >   
    >   class Student(Person):
    >       def __init__(self, name, age, number, email, student_id):
    >           self.name = name
    >           self.age = age
    >           self.number = number
    >           self.email = email
    >           self.student_id = student_id
    >   ```
    > 
    > - 사용 후
    >   
    >   ```python
    >   class Person:
    >       def __init__(self, name, age, number, email):
    >           self.name = name
    >           self.age = age
    >           self.number = number
    >           self.email = email
    >   
    >   class Student(Person):
    >       def __init__(self, name, age, number, email, student_id):
    >           # Person의 init 메서드 호출
    >           super().__init__(name, age, number, email)
    >           self.student_id = student_id
    >   ```

- MRO (Method Resulution Order) `mro()`
  : 메서드 결정 순서
  
  - 필요한 이유
    
    - 부모 클래스들이 여러 번 액세스되지 않도록 각 클래스에서 지정된 왼쪽에서 오른쪽으로 가는 순서를 보존
    - 각 부모를 오직 한 번만 호출하고, 부모들의 우선순위에 영향을 주지 않으면서 서브 클래스를 만드는 단조적인 구조 형성
    - 프로그래밍 언어의 신뢰성 있고 확장성 있는 클래스를 설계할 수 있도록 도움
    - 클래스 간의 메서드 호출 순서가 예측 가능하게 유지되며, 코드의 재사용성과 유지보수성이 향상
  
  - 예시
    
    > ```python
    >  class A:
    >     def __init__(self):
    >         print('A Constructor')
    > 
    > class B(A):
    >     def __init__(self):
    >         super().__init__()
    >         print('B Constructor')
    > 
    > class C(A):
    >     def __init__(self):
    >         super().__init__()
    >         print('C Constructor')
    > 
    > class D(B, C):
    >     def __init__(self):
    >         super().__init__()
    >         print('D Constructor')
    > 
    > print(D.mro())
    > ```

---

## 참고

- ### 인스턴스와 클래스 간의 이름 공간
  
  - 클래스를 정의하면, 클래스와 해당하는 이름 공간 생성
  - 인스턴스를 만들면, 인스턴스 객체가 생성되고 독립적인 이름 공간 생성
  - 인스턴스에서 특정 속성에 접근하면, 인스턴스 -> 클래스 순으로 탐색
  - 독립적인 이름 공간을 가지는 이점 -> 코드의 가독성, 유지보수성, 재사용성을 높이는데 도움을 줌
    - 각 인스턴스는 독립적인 메모리 공간을 가지며, 클래스와 다른 인스턴스 간에는 사로의 데이터나 상태에 직접적인 접근이 불가능
    - 객체 지향 프로그래밍의 중요한 특성 중 하나로, 클래스와 인스턴스를 모듈화하고 각각의 객체가 독립적으로 동작하도록 보장
    - 이를 통해 클래스와 인스턴스는 다른 객체들과의 상호작용에서 서로 충돌이나 영향을 주시 않으면서 독립적으로 동작할 수 있음

- ### 매직 메서드 Magic method (스페셜 메서드 Special method)
  
  : 인스턴스 메서드. 특정 상황에 자동으로 호출되는 메서드
  
  - Double underscore `__`가 있는 메서드는 특수한 동작을 위해 만들어진 메서드
  
  - ```python
    __str__(self) : 내장함수 print에 의해 호출되어 객체 출력을 문자열 표현으로 변경
    __len__(self), __lf__(self, other), __le__(self, other), __eq__(self, other), __gt__(self, other), __ge__(self, other), __ne__(self, other)
    ```

- ### 데코레이터 Decorator
  
  : 다른 함수의 코드를 유지한 채로 수정하거나 확장하기 위해 사용되는 함수
  
  - 데코레이터 정의
    
    > ```python
    >  def my_decorator(func):
    >    def wrapper():
    >        # 함수 실행 전에 수행할 작업
    >        print('함수 실행 전')
    >        # 원본 함수 호출
    >        result = func()
    >        # 함수 실행 후에 수행할 작업
    >        print('함수 실행 후')
    >  return wrapper
    > ```
  
  - 데코레이터 사용
    
    > ```python
    >  @my_decorator
    >  def my_function():
    >    print('원본 함수 실행')
    >  my_function()
    > 
    >  """
    >  함수 실행 전
    >  원본 함수 실행
    >  함수 실행 후
    >  """
    > ```
