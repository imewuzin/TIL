# 플로이드 워셜 알고리즘 Floyd-Warshall

: 모든 지점에서 다른 모든 지점까지의 최단 경로를 모두 구하는 알고리즘

- 소스 코드가 다익스트라에 비해 매우 짧아 구현이 쉬움

- 다익스트라와의 차이
  
  - 다익스트라
    
    - 단계마다 최단 거리를 가지는 노드를 하나씩 반복적으로 선택
    
            ->  해당 노드를 거쳐가는 경로를 확인하며 최단 거리 테이블 갱신하는 방식
    
    - 한 지점 -> 다른 지점까지의 최단 거리이기 때문에 1차원 리스트에 저장
    
    - 그리디 알고리즘에 속한다고 볼 수 있음
  
  - 플로이드 워셜
    
    - 단계마다 '거쳐가는 노드' 기준으로 알고리즘 수행
    
    - BUT, 매 단계마다 방문하지 않은 노드 중 최단 거리를 갖는 노드 찾을 필요 X
    
    - 2차원 테이블에 최단 거리 정보 저장(모든 지점 -> 다른 모든 지점 최단 거리 저장해야 하기 때문에)
    
    - DP 알고리즘에 속함. 노드의 개수가 N개일 때, N번 만큼의 단계를 반복하며 '점화식에 맞게' 2차원 리스트 갱신하기 때문에
    
    - 점화식 : `Dab = min(Dab, Dak + Dkb)
      
      ![floyd-warshall](https://github.com/user-attachments/assets/7403f5c4-8ef1-4f73-a2f4-e0a3d4195f0b)

- 시간 복잡도 : `O(N^3)`

- 알고리즘
  
  ```python
  import sys
  
  input = sys.stdin.readline
  INF = int(1e9)
  
  # 노드의 개수(n)과 간선의 개수(m) 입력
  n = int(input())
  m = int(input())
  
  # 2차원 리스트 (그래프 표현) 만들고, 무한대로 초기화
  graph = [[INF] * (n + 1) for _ in range(n + 1)]
  
  # 자기 자신에서 자기 자신으로 가는 비용은 0으로 초기화
  for a in range(1, n + 1):
      for b in range(1, n + 1):
          if a == b:
              graph[a][b] = 0
  
  # 각 간선에 대한 정보를 입력받아, 그 값으로 초기화
  for _ in range(m):
      # A -> B로 가는 비용을 C라고 설정
      a, b, c = map(int, input().split())
      graph[a][b] = c
  
  # 점화식에 따라 플로이드 워셜 알고리즘을 수행
  for k in range(1, n + 1):
      for a in range(1, n + 1):
          for b in range(1, n + 1):
              graph[a][b] = min(graph[a][b], graph[a][k] + graph[k][b])
  
  # 수행된 결과를 출력
  for a in range(1, n + 1):
      for b in range(1, n + 1):
          if graph[a][b] == INF:
              print('INFINITY', end=' ')
          else:
              print(graph[a][b], end=' ')
      print()
  
  # sample input
  # 4
  # 7
  # 1 2 4
  # 1 4 6
  # 2 1 3
  # 2 3 7
  # 3 1 5
  # 3 4 4
  # 4 3 2
  ```

- 예시 - 백준 11403 경로 찾기
  
  > ## 문제
  > 
  > 가중치 없는 방향 그래프 G가 주어졌을 때, 모든 정점 (i, j)에 대해서, i에서 j로 가는 길이가 양수인 경로가 있는지 없는지 구하는 프로그램을 작성하시오.
  > 
  > ## 입력
  > 
  > 첫째 줄에 정점의 개수 N (1 ≤ N ≤ 100)이 주어진다. 둘째 줄부터 N개 줄에는 그래프의 인접 행렬이 주어진다. i번째 줄의 j번째 숫자가 1인 경우에는 i에서 j로 가는 간선이 존재한다는 뜻이고, 0인 경우는 없다는 뜻이다. i번째 줄의 i번째 숫자는 항상 0이다.
  > 
  > ## 출력
  > 
  > 총 N개의 줄에 걸쳐서 문제의 정답을 인접행렬 형식으로 출력한다. 정점 i에서 j로 가는 길이가 양수인 경로가 있으면 i번째 줄의 j번째 숫자를 1로, 없으면 0으로 출력해야 한다.
  > 
  > ## 예제 입력 1 복사
  > 
  > 3
  > 0 1 0
  > 0 0 1
  > 1 0 0
  > 
  > ## 예제 출력 1 복사
  > 
  > 1 1 1
  > 1 1 1
  > 1 1 1
  > 
  > ## 예제 입력 2 복사
  > 
  > 7
  > 0 0 0 1 0 0 0
  > 0 0 0 0 0 0 1
  > 0 0 0 0 0 0 0
  > 0 0 0 0 1 1 0
  > 1 0 0 0 0 0 0
  > 0 0 0 0 0 0 1
  > 0 0 1 0 0 0 0
  > 
  > ## 예제 출력 2 복사
  > 
  > 1 0 1 1 1 1 1
  > 0 0 1 0 0 0 1
  > 0 0 0 0 0 0 0
  > 1 0 1 1 1 1 1
  > 1 0 1 1 1 1 1
  > 0 0 1 0 0 0 1
  > 0 0 1 0 0 0 0
  > 
  > ```python
  > import sys
  > input = sys.stdin.readline
  > 
  > N = int(input())
  > graph = [list(map(int, input().split())) for _ in range(N)]
  > 
  > for k in range(N):
  >     for a in range(N):
  >         for b in range(N):
  >             if graph[a][k] and graph[k][b]:
  >                 graph[a][b] = 1
  > 
  > for i in range(N):
  >     print(*graph[i])
  > ```


