# Packing & Unpacking

## Packing (패킹)

: 여러 개의 값을 하나의 변수에 묶어서 담는 것

- 변수에 담긴 값들은 **tuple** 형태로 묶임
  
  > ```python
  > packed_values = 1, 2, 3, 4, 5
  > print(packed_values)  # (1, 2, 3, 4, 5)
  > ```

- `*`을 활용한 패킹 (남은 것을 **리스트**로 패킹함)
  
  > ```python
  > numbers = [1, 2, 3, 4, 5]
  > a, *b, c = numbers
  > 
  > print(a)  # 1
  > print(*b) # [2, 3, 4]
  > print(c)  # c
  > 
  > # *b는 남은 요소들을 리스트로 패킹하여 할당
  > ```
  
  > ```python
  > print('hello')  # hello
  > print('you', 'need', 'python')  # you need python
  > 
  > """
  >     print 함수의 구조는 다음과 같다.
  >     print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False
  >     모든 비 키워드 인자는 str()이 하듯이 문자열로 변환된 후 스트림에 쓰이는데,
  >     sep로 구분되고 end를 뒤에 붙인다. (sep와 end는 문자열이어야 함. None일 수도 있는데 기본값을 사용한다는 뜻임)
  >     objects가 주어지지 않으면 print()는 end만 씁니다.
  > """
  > ```

## Unpacking 언패킹

: 패킹된 변수의 값을 개별적인 변수로 분리하여 할당하는 것

- 튜플이나 리스트 등의 객체의 요소들을 개별 변수에 할달
  
  > ```python
  > packed_values = 1, 2, 3, 4, 5
  > a, b, c, d, e = packed_values
  > print(a, b, c, d, e)  # 1 2 3 4 5
  > ```

- `*`을 활용한 언패킹
  
  > ```python
  > def my_function(x, y, z) :
  >     print(x, y, z)
  > 
  > names = ['alice', 'jane', 'peter']
  > my_function(*names)  # alice jane peter
  > ```

- `**`을 활용한 언패킹 (딕셔너리의 키-값 쌍을 함수의 키워드 인자로 언패킹)
  
  > ```python
  > def my_function(x, y, z):
  >         print(x, y, z)
  > 
  > my_dict = {'x': 1, 'y': 2, 'z': 3}
  > my_function(**my_dict)  # 1 2 3
  > ```

## 연산자 정리

- `‘*’`
  
  - 패킹 연산자로 사용될 때, 여러 개의 인자를 하나의 튜플로 묶음
  
  - 언패킹 연산자로 사용될 때, 시퀀스나 반복 가능한 객체를 각각의 요소로 언패킹하여 함수의 인자로 전달

- `‘**’`
  
  - 언패킹 연산자로 사용될 때, 딕셔너리의 키-값 쌍을 언패킹하여 함수의 키워드 인자로 전달
