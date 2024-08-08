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
