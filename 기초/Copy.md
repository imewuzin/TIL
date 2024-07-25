# 복사

- 파이썬에서는 데이터 분류 ('변경 가능한 데이터 타입'과 '변경 불가능한 데이터 타입')에 따라 복사가 달라짐

- ## 할당 (Assignment)
  
  : 할당 연산자 `=` 를 통한 복사는 해당 객체에 대한 객체 참조를 복사
  
  > ```python
  >   >   original_list = [1, 2, 3]
  > copy_list = original_list
  > print(original_list, copy_list)  # [1, 2, 3] [1, 2, 3]
  > 
  > copy_list[0] = 'hi'
  > print(original_list, copy_list)  # ['hi', 2, 3] ['hi', 2, 3]
  > ```

- ## 얕은 복사
  
  - 슬라이싱을 통해 생성된 객체는 원본 객체와 독립적으로 존재
    
    > ```python
    > a = [1, 2, 3]
    > b = a[:]
    > print(a, b)  # [1, 2, 3] [1, 2, 3]
    > 
    > b[0] = 100
    > print(a, b)  # [1, 2, 3] [100, 2, 3]
    > ```
  
  - 2차원 리스트와 같이 변경 가능한 객체 안에 변경 가능한 객체가 있는 경우
    
    : 내부 객체의 주소는 같이 때문에 함께 변경됨
    
    > ```python
    > a = [1, 2, [1, 2]]
    > b = a[:]
    > print(a, b)  # [1, 2, [1, 2]] [1, 2, [1, 2]]
    > 
    > b[2][0] = 100
    > print(a, b)  # [1, 2, [100, 2]] [1, 2, [100, 2]]
    > ```

- ## 깊은 복사
  
  : 내부에 중첩된 모든 객체까지 새로운 객체 주소를 참조하도록 함
  
  > ```python
  > import copy
  > 
  > original_list = [1, 2, [1, 2]]
  > deep_copied_list = copy.deepcopy(original_list)
  > deep_copied_list[2][0] = 100
  > 
  > print(original_list)  # [1, 2, [1, 2]]
  > print(deep_copied_list)  # [1, 2, [100, 2]]
  > ```
