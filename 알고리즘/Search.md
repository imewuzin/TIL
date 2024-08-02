# 검색

: 저장되어 있는 자료 중에서 원하는 항목을 찾는 작업. 목적하는 탐색 키를 가진 항목을 찾는 것

- 탐색 키(Search Key) : 자료를 구별하여 인식할 수 있는 키

---

## 순차 검색 Sequential Search

: 일렬로 되어 있는 자료를 순서대로 검색하는 방법

- 가장 간단하고 직관적인 검색 방법

- 배열이나 연결 리스트 등 순차구조로 구현된 자료구조에서 원하는 항목을 찾을 때 유용

- 알고리즘이 단순해 구현이 쉽지만, 검색 대상의 수가 많은 경우에는 수행시간이 급격히 증가해 비효율적

- #### 정렬되어 있지 않은 경우
  
  - 검색 과정
    
    1. 첫번째 원소부터 순서대로 검색 대상과 키 값이 같은 원소가 있는지 비교하며 찾음
    
    2. 키 값이 동일한 원소를 찾으면 그 원소의 인덱스 반환
    
    3. 자료구조의 마지막에 이를 때까지 검색 대상을 찾지 못하면 검색 실패
  
  - 시간 복잡도 : **O(n)**
    
    - 찾고자 하는 원소의 순서에 따라 비교횟수가 결정됨
  
  > ```pseudocode
  > def sequential_search(a, n, key)
  >   i <- 0
  >   while i<n and a[i]!=key:
  >     i <- i+1
  >   if i<n: return i
  >   else: return -1
  > ```

- #### 정렬되어 있는 경우
  
  - 검색 과정
    
    1. 자료가 오름차순으로 정렬된 상태에서 검색을 실시한다고 가정
    
    2. 자료를 순차적으로 검색하며 키 값을 비교하여, 원소의 키 값이 검색 대상의 키 값보다 크면 찾는 원소가 없다는 것이므로 더이상 검색하지 않고 검색 종료
  
  - 시간 복잡도 : O(n)
    
    - 찾고자 하는 원소의 순서에 따라 비교횟수가 결정됨
    
    - 정렬이 되어있으므로, 검색 실패를 반환하는 경우 평균 비교 횟수가 반으로 줄어듦
  
  > ```pseudocode
  > def sequential_search2(a, n, key)
  >   i <- 0
  >   while i<n and a[i]<key:  # 배열을 벗어나지 않은 상태!
  >     i <- i+1
  >   if i<n and a[i]==key:
  >     return i
  >   else:
  >     return -1
  > ```
  
  - <mark>배열을 벗어나지 않으면서 값을 비교할 때는 인덱스 코드가 먼저!</mark>
    
    - `while i<n and a[i]<key` 꼴로 작성해야 함
    
    - `while a[i]<key and i<n` 일 경우
      
      - 단축 평가로 인해 `a[i]<key` 에서 인덱스 오류가 발생

---

## 이진 검색 Binary Search

: 자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법

- 목적 키를 찾을 때까지 이진 검색을 순환적으로 반복 수행함으로써 검색 범위를 반으로 줄여가면서 보다 빠르게 검색을 수행함

- 이진 검색을 하기 위해서는 **자료가 정렬된 상태**여야 함

- 구현
  
  1. 검색 범위의 시작점과 종료점을 이용해 검색을 반복 수행
  
  2. 이진 검색의 경우, 자료에 삽입하거나 삭제가 발생했을 때 배열의 상태를 항상 정렬 상태로 유지하는 추가 작업 필요
  
  > ```pseudocode
  > def binarySearch(a, N, key)
  >   start = 0
  >   end = N-1
  >   while start <= end:
  >     middle = (start + end) // 2
  >     if a[middle] == key:           # 검색 성공
  >       return True
  >     elif a[middle] > key:
  >       end = middle - 1
  >     else:
  >       start = middle + 1
  >   return False                     # 검색 실패
  > ```
  
  - 재귀 함수 이용
    
    > ```pseudocode
    > def binarySearch2(a, low, high, key):
    >   if low > high:               # 검색 성공
    >     return False
    >   else:
    >     middle = (low + high) // 2
    >     if key == a[middle]:
    >       return True
    >     elif key < a[middle]:
    >       return binarySearch2(a, low, middle-1, key)
    >     elif key > a[middle]:
    >       return binarySearch2(a, middle+1, high, key)
    > ```

- ### 인덱스
  
  : Database에서 유래했으며, 테이블에 대한 동작 속도를 높여주는 자료 구조를 일컬음
  
  - 인덱스를 저장하는데 필요한 디스크 공간은 보통 테이블을 저장하는데 필요한 디스크 공간보다 작음. 보통 인덱스는 키-필드만 갖고 있고, 테이블의 다른 세부 항목들은 갖고 있지 않기 때문에.
  - 데이터베이스 인덱스는 이진 탐색 트리 구조로 되어 있음

----

## 해쉬 Hash
