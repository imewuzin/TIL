# 백트래킹 Backtracking

: 해를 찾는 도중에 막히면(해가 아니면) 되돌아가 다시 해를 찾아 가는 기법

- 최적화(optimization) 문제와 결정(decision) 문제 해결 가능
  
  - 결정 문제
    
     : 문제의 조건을 ㅁ나족하는 해가 존재하는지의 여부를 'yes' 또는 'no'가 답하는 문제
    
    - 미로 찾기, n-Queen 문제, Map coloring, 부분 집합의 합(Subset Sum) 문제 등

- 백트래킹과 깊이우선탐색(DFS)의 차이
  
  - 어떤 노드에서 출발하는 경로가 해결책으로 이어질 것 같지 않으면 더 이상 그 경로를 따라가지 않음으로써 시도의 횟수를 줄임 (Prunning 가지치기)
  
  - 깊이우선탐색이 모든 경로를 추적하는데 비해 백트래킹은 **불필요한 경로를 조기에 차단**
  
  - 깊이우선탐색을 가하기에는 경우의 수가 너무나 많음. 즉, `N!`가지의 경우의 수를 가진 문제에 대해 깊이우선탐색을 가하면 당연히 처리 불가능한 문제
  
  - 백트래킹 알고리즘을 적용하면 일반적으로 경우의 수가 줄어들지만 이 역시 최악의 경우에는 여전히 지수함수 시간(Exponential Time)을 요하므로 처리 불가능

- 백트래킹 기법
  
  - 어떤 노드의 유망성을 점검한 후, 유망(promising)하지 않다고 결정되면 그 노드의 부모로 되돌아가(backtracking) 다음 자식 노드로 감
  
  - 어떤 노드를 방문했을 때, 그 노드를 포함한 경로가 해답이 될 수 없으면 그 노드는 유망하지 않다고 하며, 반대로 해답의 가능성이 있으면 유망하다고 함
  
  - 가지치기(pruning) : 유망하지 않은 노드가 포함되는 경로는 더 이상 고려하지 않음
  
  ```python
  def checknode(v):  # node
    if promising(v):
      if there is a solution at v:
        write the solution
      else:
        for u in each child of v:
          checknode(u)
  ```

- 백트래킹 절차
  
  1. 상태 공간 트리의 깊이 우선 검색 실시
  
  2. 각 노드가 유망한지 점검
  
  3. 만일 그 노드가 유망하지 않으면, 그 노드의 부모 노드로 돌아가 검색을 계속함

- 예시 - 미로 찾기
  
  > - 입구와 출구가 주어진 미로에서 입구부터 출구가지의 경로를 찾는 문제
  > 
  > - 이동할 수 있는 방향을 4방향으로 제한
  >    ![미로찾기1](https://github.com/user-attachments/assets/eb7c61a0-f85e-46a2-beb1-234e14f66957)
  > 1. 입구에서 직진    
  >    ![미로찾기2](https://github.com/user-attachments/assets/a06a3229-8189-4a02-aa77-d6e6a45d257e)
  > 2. 스택을 이용해 지나온 경로를 역으로 되돌아감
  >    ![미로찾기3](https://github.com/user-attachments/assets/f3a073d5-b144-484c-b5fd-4c705984fc21)
  > 3. 스택을 이용해 다시 경로를 찾음
  >     ![미로찾기4](https://github.com/user-attachments/assets/4ea069e5-6b8b-49cf-ac9f-f2570bb6121a)

- 예시 - 부분 집합
  
  > - 어떤 집합의 공집합과 자기자신을 포함한 모든 부분집합을 powerset이라 하며, 구하고자 하는 어떤 집합의 원소 개수가 n일 때, 부분집합의 개수는 2*n개 이다
  > 
  > - 각 원소가 부분집합에 포함되었는지를 loop 이용해 확인하고 부분집합 생성
  > 
  > ```python
  > bit = [0, 0, 0, 0]
  > for i in range(2):
  >   bit[0] = i           # 0번째 원소
  >   for j in range(2):
  >     bit[1]=j           # 1번째 원소
  >     for k in range(2):
  >       bit[2] = k       # 2번째 원소
  >       for l in range(2):
  >         bit[3] = l     # 3번째 원소
  >         print(bit)     # 생성된 부분집합 출력
  > ```
  > 
  > ```python
  > def backtrack(a, k, n):  # a주어진 배열, k 결정할 원소, n 원소 개수
  >   c = [0] * MAXCANDIDATES
  >   if k == n:
  >     process_solution(a, k)  # 답이면 원하는 작업 함
  >   else:
  >     ncandidates = construct_candidates(a, k, n, c)
  >     for i in range(ncandidates):
  >       a[k] = c[i]
  >       backtrack(a, k+1, n)
  > 
  > def construct_candidates(a, k, n, c):
  >   c[0] = True
  >   c[1] = False
  >   return 2
  > 
  > def process_solution(a, k):
  >   for i in range(k):
  >     if a[i]:
  >       print(num[i], end='')
  >   print()
  > 
  > MAXCANDIDATES = 2
  > NMAX = 4
  > a = [0] * NMAX
  > num =[1, 2, 3, 4]
  > backtrack(a, 0, 3)
  > ```
  > 
  > - 가지치기
  >   
  >   - 집합 `{1, 2, 3}`의 원소에 대해 각 부분집합에서의 포함 여부를 트리로 표현
  >   
  >   - i 원소의 포함 여부를 결정하면 i까지의 부분집합의 합 s_i를 결정할 수 있음
  >   
  >   - s_i-1이 찾고자 하는 부분집합의 합보다 크면 남은 원소를 고려할 필요가 없음
  >   
  >   - A[i] 원소를 부분집합의 원소로 고려하는 재귀 함수(A는 서로 다른 자연수의 집합)
  >     
  >     ```python
  >     # i-1 원소까지 고려한 합 s, 찾으려는 합 
  >     f(i, N, s, t)
  >       if s == t   # i-1 원소까지의 합이 찾는 값인 경우
  >       elif i == N # 모든 원소에 대한 고려가 끝난 경우
  >       elif s > t  # 남은 원소를 고려할 필요가 없는 경우
  >       else        # 남은 원소가 있고, s < t 인 경우
  >         subset[i] = 1
  >         f(i+1, N, s+A[i], t)  # i원소 포함
  >         subset[i] = 0
  >         f(i+1, N, s, t)       # i원소 미포함
  >     ```
  >   
  >   - 추가 고려 사항 : 남은 원소의 합을 다 더해도 찾는 값 T 미만인 경우 중단

- 예시 - 순열
  
  > - 동일한 숫자가 포함되지 않았을 때, 각 자리 수 별 loop 이용 시
  >   
  >   ```python
  >   for i1 in range(1,4):
  >   for i2 in range(1,4):
  >     if i2 != i1:
  >       for i3 in range(1,4):
  >         if i3 != i1 and i3 != i2:
  >           print(i1, i2, i3)
  >   ```
  > 
  > - 백트래킹 이용해 `{1, 2, 3, ..., NMAX}` 에 대한 순열 구하기
  >   
  >   ```python
  >   def backtrack(a, k, n):
  >   c = [0] * MAXCANDIDATES
  >   if k == n:
  >     for i in range(0, k):
  >       print(a[i], end=" ")
  >     print()
  >   else:
  >     ncandidates = construct_candidates(a, d, n, c)
  >     for i in range(ncandidates):
  >       a[k] = c[i]
  >       backtrack(a, k+1, n)
  >   
  >   def construct_candidates(a, k, n, c):
  >     in_perm = [False] * (NMAX + 1)
  >     for i in range(k):
  >       in_perm[a[i]] = True
  >     ncandidates = 0
  >     for i in range(1, NMAX + 1)
  >       if in_perm[i] == False:
  >         c[ncandidates] = i
  >         ncandidates += 1
  >     return ncandidates
  >   
  >   MAXCANDIDATES = 3
  >    NMAX = 3
  >    a = [0] * NMAX
  >    backtrack(a, 0, 3)
  >   ```
  > 
  >  
