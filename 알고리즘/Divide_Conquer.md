# 분할 정복 알고리즘 Divide & Conquerd

- 유래
  
  - 1805년 12월 2일 아우스터리츠 전투에서 나폴리옹이 사용한 전략
  
  - 전력이 우세한 연합군을 공격하기 위해 나폴레옹은 연합군의 중앙부로 쳐들어가 연합군을 둘로 나눔
  
  - 둘로 나뉜 연합군을 한 부분씩 격파함

- 설계 전략
  
  - 분할(Divide) : 해결할 문제를 여러 개의 작은 부분으로 나눔
  
  - 정복(Conquer) : 나눈 작은 문제를 각각 해결
  
  - 통합(Combine) : (필요하다면) 해결된 해답을 모음

- 예시 - 거듭제곱
  
  > ```python
  > def Power(Base, Exponent):
  >   if Base == 0:
  >     return 1
  >   result = 1  # Base^0은 1이므로
  >   for i in range(Exponent):
  >     result *= Base
  >   return result
  > ```
  > 
  > - 분할 정복 기반의 알고리즘 : O(log_2 n)
  > 
  > ```python
  > def Power(Base, Exponent)L
  >   if Exponent == 0 or Base == 0:
  >     return 1
  >   if Exponent % 2 == 0:
  >     NewBase = Power(Base, Exponenet/2)
  >     return NewBase * NewBase
  >   else:
  >     NewBase = Power(Base, (Exponent-1)/2)
  >     return (NewBase * NewBase) * Base
  > ```

---

## 이진 검색 Binary Search

: 자료의 가운데에 있는 항목의 키 값과 비교해 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법

- 목적 키를 찾을 때까지 이진 검색을 순환적으로 반복 수행함으로써 검색 범위를 반으로 줄여가면서 보다 빠르게 검색을 수행함
- 이진 검색을 하기 위해서는 **자료가 정렬된 상태**여야 함
- 검색 과정
  1. 자료의 중앙에 있는 원소 고름
  2. 중앙 원소의 값과 찾고자 하는 목표 값 비교
  3. 목표 값이 중앙 원소의 값보다 작으면 자료의 왼쪽 반에 대해서 새로 검색 수행. 크다면 오른쪽 반에 대해 새로 검색 수행
  4. 찾고자 하는 값 찾을 때까지 1~3 과정 반복
- 알고리즘
  > - 반복 구조
  > ```pseudocode
  > binarySearch(n, S[], key)
  > low <- 0
  > high <- n-1
  > 
  > WHILE low <= hign
  >     mid <- low + (high-low) / 2
  > 
  >        IF s[mid] == key
  >         RETURN mid
  >        ELIF s[mid] > key
  >            high <- mid - 1
  >        ELSE
  >            low <- mid + 1
  > 
  > RETURN -1
  > ```
  > - 재귀 구조
  > ```pseudocode
  > binarySearch(a[], low, high, key)
  > 	IF low > high
  > 		RETURN -1
  > 
  > 	ELSE
  >     	mid <- low + (high-low) / 2
  >  	    IF key == a[mid]
  >		        RETURN mid
  >        	ELIF key < a[mid]
  >         	RETURN binarySearch(a[], low, mid-1, key)
  >         ELSE
  >             RETURN binarySearch(a[], mid+1, high, key)
  > 
  > ```
