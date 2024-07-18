# 제어문 (Control Statement)

: 코드의 실행 흐름을 제어하는 데 사용되는 구문

: 조건에 따라 코드 블록을 실행하거나 반복적으로 코드를 실행

- 종류
  
  - 조건문 : `if`, `elif`, `else`
  
  - 반복문 : `for`, `while`
  
  - 반복문 제어 : `break`, `continue`, `pass`

---

## 조건문 (Conditional Statement)

: 주어진 조건식을 평가하여 해당 조건이 참(`True`)인 경우에만 코드 블록을 실행하거나 건너뜀

- if statement의 기본 구조
  
  ```python
  if 표현식:
      코드 블록
  elif 표현식:
      코드 블록
  else:
      코드블록
  ```

- #### 복수 조건문
  
  : 조건식을 동시에 검사하는 것이 아니라 순차적으로 비교
  
  > ```python
  > dust = 35
  > 
  > if dust > 150:
  >     print('매우 나쁨')
  > elif dust > 80:
  >        print('나쁨')
  > elif dust > 30:
  >        print('보통')
  >    else:
  >        print('좋음')
  > ```

- #### 중첩 조건문
  
  > ```python
  > dust = 480
  > if dust > 150:
  >        print('매우 나쁨')
  >     if dust > 300:      # 중첩 조건문
  >           print('위험해요! 나가지 마세요!')
  > elif dust > 80:
  >    print('나쁨')
  >    elif dust > 30:
  >    print('보통')
  > else:
  >     print('좋음')
  > ```

---

## 반복문 (Loop Statement)

: 주어진 코드 블록을 여러번 반복해서 실행하는 구문

- 적절한 반복문 활용하기
  
  - `for`
  
      - 반복 횟수가 명확하게 정해져 있는 경우에 유용
  
      - 예를 들어 리스트, 튜플, 문자열 등과 같은 시퀀스 형식의 데이터를 처리할 때
  
  - `while`
  
      - 반복 횟수가 불명확하거나 조건에 따라 반복을 종료해야 할 때 유용
  
      - 예를 들어 사용자의 입력을 받아서 특정 조건이 충족될 때까지 반복하는 경우

### `for` statement

  : 특정 작업을 반복적으로 수행

  : 임의의 시퀀스의 항목들을 그 시퀀스에 들어있는 순서대로 반복

> ```python
> for 변수 in 반복 가능한 객체(iterable):
>     코드 블록
> ```

- 원리
  
  - 리스트 내 첫 항목이 반복 변수에 할당되고 코드블록이 실행
  
  - 다음으로 반복 변수에 리스트의 2번째 항목이 할당되고 코드블록이 다시 실행
  
  - ... 마지막으로 반복 변수에 리스트의 마지막 요소가 할당되고 코드블록이 실행

- 순회 종류
  
  > ```python
  > # 문자열 순회
  > country = 'Korea'
  > for char in country:
  >     print(char)
  > 
  > # range 순회
  > for i in range(5):
  >     print(i)
  > 
  > # dict 순회
  > my_dict = {
  >     'x' : 10,
  >     'y' : 20,
  >     'z' : 30,
  >     }
  > for key in my_dict:
  >     print(key)
  >     print(my_dict[key])
  > 
  > # 인덱스로 리스트 순회 : 리스트의 요소가 아닌 인덱스로 접근해 해당 요소들 변경하기
  > numbers = [4, 6, 10, -8, 5]
  > for i in range(len(numbers)):
  >     numbers[i] = numbers[i] * 2
  > print(numbers)  # [8, 12, 20, -26, 10]
  > ```

- 중첩 리스트 순회
  : 안쪽 리스트 요소에 접근하려면 바깥 리스트를 순회하면서 중첩 반복을 사용해 각 안쪽 반복을 순회
  
  > ```python
  > elements = [['A', 'B'], ['c', 'd']]
  > 
  > for elem in elements:
  >     print(elem)
  > """
  > ['A', 'B']
  > ['c', 'd']
  > """
  > ```
  
  > ```python
  > elements = [['A', 'B'], ['c', 'd']]
  > 
  > for elem in elements:
  >     for item in elem:
  >         print(item)
  >   """
  > A
  > B
  > c
  > d
  > """
  > ```

### `while` statement

  : 주어진 조건이 참인 동안 반복해서 실행
  : `==` 조건식이 거짓(`False`)가 될 때까지 반복

> ```python
> while 조건식:
>     코드 블록
> ```

- <mark>반드시 **종료 조건** 필요</mark>

- while문을 사용한 특정 입력 값에 대한 종료 조건 활용하기
  
  > ```python
  > number = int(input('양의 정수를 입력해주세요.: '))
  > 
  > while number <= 0:
  > 
  >        if number < 0:
  >            print('음수를 입력했습니다.')
  >     else:
  >         print('0은 양의 정수가 아닙니다.')
  >     number = int(input('양의 정수를 입력해주세요.: '))
  > 
  > print('잘했습니다!')
  > """
  > 양의 정수를 입력해주세요.: 0
  > 0은 양의 정수가 아닙니다.  
  > 양의 정수를 입력해주세요.: -1
  > 음수를 입력했습니다.       
  > 양의 정수를 입력해주세요.: 1
  > 잘했습니다!
  > """
  > ```

### 반복 제어

  : 반복문 내 일부만 실행하는 것이 필요할 때 사용

- 키워드
  
  - `break` : 반복을 즉시 중지
  
  - `continue` : 다음 반복으로 건너뜀
  
  - `pass` : 아무런 동작도 수행하지 않고 넘어감
    
    > - 코드 작성 중 미완성 부분. 구현해야 할 부분이 나중에 추가될 수 있고, 코드를 컴파일하는 동안 오류가 발생하지 않음
    > 
    > - 조건문에서 아무런 동작을 수행하지 않아야 할 때
    > 
    > - 무한 루프에서 조건이 충족되지 않을 때 `pass`를 사용하여 루프를 진행하는 방법

---

## List Comprehension

: 간결하고 효율적인 리스트 생성 방법

- 구조
  
  ```python
  [expression for 변수 in iterable]
  list(expression for 변수 in iterable)
  
  [expression for 변수 in iterable if 조건식]
  list(expression for 변수 in iterable if 조건식)
  ```

- 사용 방법
  
  > ```python
  > # 사용 전
  > numbers = [1, 2, 3, 4, 5]
  > squared_numbers = []
  > 
  > for num in numbers:
  >     squared_numbers.append(num**2)
  > print(squared_numbers)  # [1, 4, 9, 16, 25]
  > 
  > # 사용 후
  > numbers = [1, 2, 3, 4, 5]
  > squared_numbers = [num**2 for num in numbers]
  > print(squared_numbers)  # [1, 4, 9, 16, 25]
  > ```

- 2차원 배열 생성 예시 (인접행렬)
  
  > ```python
  > data1 = [[0] * (5) for _ in range(5)]
  > 
  > # 또는
  > data2 = [[0 for _ in range(5)] for _ in range(5)]
  > 
  > """
  > [[0, 0, 0, 0, 0],
  >  [0, 0, 0, 0, 0],
  >  [0, 0, 0, 0, 0],
  >  [0, 0, 0, 0, 0],
  >  [0, 0, 0, 0, 0]]
  > """
  > ```

---

## 참고

- `enumerate(iterable, start=0)`
  
  : iterable 객체의 각 요소에 대해 인덱스와 함께  반환하는 내장 함수
  
  - 예시
    
    > ```python
    > 
    > ```
