# 에러와 예외

## 버그 bug

: 소프트웨어에서 발생하는 오류 또는 결함

: 프로그램의 예상된 동작과 실제 동작 사이의 불일치

- 기원
  
  - 'Bug'라는 용어는 소프트웨어 이전부터 공학 분야에서 사용되었음
  
  - 컴퓨터 분야에서의 첫 사용은 1947년 프로그래밍 언어의 일종인 코볼 발명자 '그레이스 호퍼'와 그녀의 팀이 컴퓨터의 릴레이에서 실제 나방을 발견한 것이 시초로 할려짐
  
  - 하버드 Mark 2 컴퓨터 회로에 실제 나방이 들어가 합선을 일으켜 비정상적으로 동작한 것을 기록한 것
  
  - 발견된 나방은 컴퓨터 로그북에 테이브로 붙여서 보존되었고, 'First actual case of bug being found (실제 버그가 발견된 첫 사례)'라는 메모가 남겨짐
  
  - '버그'라는 용어는 이전부터 사용되어 왔지만, 이 사건을 계기로 컴퓨터 시스템에서 발생하는 오류 또는 결함을 지칭하는 용어로 널리 사용되기 시작

## 디버깅 Debugging

: 소프트웨어에서 발생하는 버그를 찾아내고 수정하는 작업

: 프로그램의 오작동 원인을 식별하여 수정하는 작업

- 방법
  
  1. `print`함수 사용
     
     - 특정 함수 결과, 반복/조건 결과 등 나눠서 생각
     
     - 코드를 bisection으로 나눠서 생각
  
  2. 개발 환경(text editor, IDE) 등에서 제공하는 기능 활용
     
     - breakpoint, 변수 조회 등
  
  3. `python tutor` 활용 (단순 파이썬 코드인 경우)
  
  4. 뇌 컴파일, 눈 디버깅 등

## 에러 Error

: 프로그램 실행 중 발생하는 예외 상황

- 파이썬의 에러 유형
  
  - ### 문법 에러 Syntex Error
    
    : 프로그램의 구문이 올바르지 않은 경우 발생 (오타, 괄호 및 콜론 누락 등의 문법적 오류)
    
    > - 문법 오류 `Invalid syntax`
    >   
    >   ```python
    >   while  # SyntaxError : invalid syntax
    >   ```
    > 
    > - 잘못된 할당 `assign to literal`
    >   
    >   ```python
    >   5 = 3  # SyntaxError : cannot assign to literal
    >   ```
    > 
    > - End of Line `EOL`
    >   
    >   ```python
    >   print('hello
    >   # SyntaxError : EOL shile scanning string literal
    >   ```
    > 
    > - End of File `EOF`
    >   
    >   ```python
    >   print(
    >   # SyntaxError : unexpected EOF while parsing
    >   ```
  
  - ### 예외 Exception
    
    : 프로그램 실행 중에 감지되는 에러
    
    - #### 내장 예외 Built-in Exceptions
      
      : 예외 상황을 나타내는 예외 클래스등
      
      - 파이썬에서 이미 정의되어 있으며, 특정 예외 상황에 대한 처리를 위해 사용
      
      - [참고 문서]([내장 예외 &#8212; Python 3.12.4 문서](https://docs.python.org/ko/3/library/exceptions.html#ValueError))
      
      - 예시
        
        > - `ZeroDivisionError`
        >   : 나누기 또는 모듈로 연산의 두 번째 인자가 0일 때 발생
        >   
        >   ```py
        >   10/0 # ZeroDivisionError: division by zero
        >   ```
        > 
        > - `NameError`
        >   : 지역 또는 전역 이름을 찾을 수 없을 때 발생
        >   
        >   ```py
        >   print(name_error) # NameError: name 'name_error' is not defined
        >   ```
        > 
        > - `TypeError` 
        >   
        >   - 타입 불일치
        >     
        >     ```py
        >     '2' + 2  # TypeError: can only concatenate str (not "int") to str
        >     ```
        >   
        >   - 인자 누락
        >     
        >     ```py
        >     sum()  # TypeError: sum() takes at least 1 positional argument (0 given)
        >     ```
        >   
        >   - 인자 초과
        >     
        >     ```py
        >     sum(1, 2, 3)  # TypeError: sum() takes at most 2 arguments (3 given)
        >     ```
        >   
        >   - 인자 타입 불일치
        >     
        >     ```py
        >     import random
        >     random.sample(1, 2)
        >     # TypeError: Population must be a sequence.  For dicts or sets, use sorted(d).
        >     ```
        > 
        > - `ValueError` 
        >   : 연산이나 함수에 문제가 없지만 부적절한 값을 가진 인자를 받았고, 상황이 IndexError 처럼 더 구체적인 예외로 설명되지 않는 경우 발생
        >   
        >   ```python
        >   int('1.5') # ValueError: invalid literal for int() with base 10: '3.5'
        >   
        >   range(3).index(6) # ValueError: 6 is not in range
        >   ```
        > 
        > - `IndexError`
        >   : 시퀀스 인덱스가 범위를 벗어날 때 발생
        >   
        >   ```py
        >   empty_list = []
        >   empty_list[2]
        >   # IndexError: list index out of range
        >   ```
        > 
        > - `KeyError`
        >   : 딕셔너리에 해당 키가 존재하지 않는 경우
        >   
        >   ```py
        >   person = {'name': 'Alice'}
        >   person['age']  # KeyError: 'age'
        >   ```
        > 
        > - `ModuleNotFoundError`
        >   : 모듈을 찾을 수 없을 때 발생
        >   
        >   ```py
        >   import hahaha  # ModuleNotFoundError: No module named 'hahaha'
        >   ```
        > 
        > - `ImportError`
        >   : 임포트 하려는 이름을 찾을 수 없을 때 발생
        >   
        >   ```py
        >   from random import hahaha
        >   # ImportError: cannot import name 'hahaha' from 'random'
        >   ```
        > 
        > - `KeyboardInterrupt`
        >   : 사용자가 Control-C 또는 Delete를 누를 때 발생
        >   : 무한루프 시 강제 종료
        >   
        >   ```py
        >   while True: 
        >       continue
        >   
        >   '''
        >   Traceback (most recent call last):
        >     File "...", line 2, in <module>
        >       continue
        >   KeyboardInterrupt
        >   '''
        >   ```
        > 
        > - `IndentationError`
        >   : 잘못된 들여쓰기와 관련된 문법 오류
        >   
        >   ```py
        >   for i in range(10):
        >       print(i) # IndentationError: expected an indented block
        >   ```

---

## 예외 처리

: 예외가 발생했을 때 프로그램이 비정상적으로 종료되지 않고, 적절하게 처리할 수 있도록 하는 방법

- 예외 처리 사용 구문
  
  : 예외 발생 시, 프로그램은 `try` 블록을 빠져나와 해당 예외에 대응하는 `except` 블록으로 이동
  
  - `try` : 예외가 발생할 수 있는 코드 작성
  
  - `except` : 예외가 발생했을 때 실행할 코드 작성
  
  - `else` : 예외가 발생하지 않았을 때 실행할 코드 작성
  
  - `finally` : 예외 발생 여부와 상관없이 항상 실행할 코드 작성
  
  > ```python
  > try:
  >     x = int(input('숫자를 입력하세요: '))
  >     y = 10 / x
  > except ZeroDivisionError:
  >     print('0으로 나눌 수 없습니다.')
  > except ValueError:
  >     print('유효한 숫자가 아닙니다.')
  > else:
  >     print(f'결과: {y}')
  > finally:
  >     print('프로그램이 종료되었습니다.')
  > ```

- 복수 예외 처리 예시
  
  > - 모두 명시
  >   
  >   ```python
  >   try:
  >       num = int(input('100으로 나눌 값을 입력하시오 : '))
  >       print(100 / num)
  >   except (ValueError, ZeroDivisionError):
  >       print('제대로 입력해주세요.')
  >   ```
  > 
  > - 별도 작성
  >   
  >   ```python
  >   try:
  >       num = int(input('100으로 나눌 값을 입력하시오 : '))
  >       print(100 / num)
  >   except ValueError:
  >       print('숫자를 넣어주세요.')
  >   except ZeroDivisionError:
  >       print('0으로 나눌 수 없습니다.')
  >   except:
  >       print('에러가 발생하였습니다.')
  >   ```

- 예외 처리 주의 사항 - 내장 예외의 상속 계층구조 주의
  
  - 아래와 같이 예외를 작성하면 코드는 2번째 `except` 절 이후로 도달하지 못함
    
    > ```python
    > try:
    >     num = int(input('100으로 나눌 값을 입력하시오 : `))
    >     print(100 / num)
    > except BaseException:
    >     print('숫자를 넣어주세요.`)
    > # 하단 블록에 도달하지 못함
    > except ZeroDivisionError:
    >      print('0으로 나눌 수 없습니다.')
    > except:
    >     print('에러가 발생하였습니다.')
    > ```
  
  - 내장 예외 클래스는 상속 계층구조를 가지기 때문에 `except`절로 분기 시 반드시 하위 클래스를 먼저 확인할 수 있도록 작성해야 함
    ![image](https://github-production-user-asset-6210df.s3.amazonaws.com/32388270/250026421-6ce033c9-6715-473f-a6c2-9e2288361a1a.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240725%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240725T011625Z&X-Amz-Expires=300&X-Amz-Signature=b4d5d0a831d159665e945e2362f66621fc015f64a126c31012a78da9b53d6707&X-Amz-SignedHeaders=host&actor_id=156065214&key_id=0&repo_id=329857415)
  
  - [참고 문서](https://docs.python.org/ko/3/library/exceptions.html#exception-hierarchy)

- #### 예외 객체
  
  : 예외가 발생했을 때 예외에 대한 정보를 담고 있는 객체
  
  - `except` 블록에서 예외 객체를 받아 상세한 예외 정보 활용 가능
  
  - `as` 키워드
    
    > ```python
    > my_list = []
    > try:
    >     number = my_list[1]
    > except IndexError as error:
    >     print(f'{error}가 발생했습니다.')
    > 
    > # list index out of range가 발생했습니다.
    > ```

- `try-except` 와 `if-else` 함께 사용 가능
  
  > ```python
  > try:
  >     x = int(input('숫자를 입력하세요: '))
  >     if x < 0:
  >         print('음수는 허용되지 않습니다.')
  >        else:
  >            print('입력한 숫자:', x)
  >    except ValueError:
  >        print('오류 발생')
  > ```

---

## 예외처리와 값 검사에 대한 2가지 접근 방식

### EAFP

  : Easier to Ask for Forgiveness than Permission
  : 예외 처리를 중심으로 코드를 작성하는 접근 방식

- `try - except`
  
  > ```python
  > try:
  >     result = my_dict['key']
  >     print(result
  > except KeyError:
  >        print('Key가 존재하지 않습니다.')
  > ```

### LBYL

  : Look Before You Leap
  : 값 검사를 중심으로 코드를 작성하는 접근 방식

- `if - else`
  
  > ```python
  > if 'key' in my_dict:
  >     result = my_dict['key']
  >        print(result
  > else:
  >     print('Key가 존재하지 않습니다.')
  > ```

| EAFP | LBYL    |
|:--- | --- |
| '일단 실행하고 예외를 처리'  | '실행하기 전에 조건을 검사'    |
| 코드를 실행하고 예외가 발생하며 예외처리를 수행 | 코드 실행 전에 조건문 등을 사용하여 예외 상황을 미리 검사하고, 예외 상황을 피하는 방식 |
| 코드에서 예외가 발생할 수 있는 부분을 미리 예측하여 대비하는 것이 아니라, 예외가 발생한 후에 예외를 처리 | 코드가 좀 더 예측 가능한 동작을 하지만, 코드가 더 길고 복잡해질 수 있음 |
| 예외 상황을 예측하기 어려운 경우에 유용    |     |
