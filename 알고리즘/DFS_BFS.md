# 깊이 우선 탐색 DFS(Depth First Search)

: 시작 정점의 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와 다른 방향의 정점으로 탐색을 계속 반복해 결국 모든 정점을 방문하는 순회방법

- 가장 마지막에 만났던 갈림길의 정점으로 되돌아가 다시 깊이 우선 탐색을 반복해야 하므로 <mark>후입선출 구조의 스택</mark> 사용

- 알고리즘
  
  1. 시작 정점 v를 결정해 방문
  
  2. 정점 v에 인접한 정점 중에서
     
     - 방문하지 않은 정점 w가 있으면, 정점 v를 스택에 push하고 정점 w 방문. 그리고 w를 v로 해 다시 2번 반복
     
     - 방문하지 않은 정점이 없으면, 탐색의 방향을 바꾸기 위해 스택을 pop해 받은 가장 마지막 방문 정점을 v로 해 다시 2번 반복
  
  3. 스택이 공백이 될 때까지 2번 반복

```pseudocode
visited[], stack[] 초기화
DFS(v)
    시작점 v 방문;
    visited[v] <- true
    while{
        if (v의 인접 정점 중 방문 안 한 정점 w가 있으면)
            push(v);
            v <- w; (w에 방문)
            visite[w] <- true;
        else
            if (스택이 비어 있지 않으면)
                v <- pop(stack)
            else
                break
    }
end DFS()
```

```python
def my_dfs(graph, start_node):
    visited = list() # 방문한 노드를 담을 배열
    stack = list() # 정점과 인접한 방문 예정인 노드를 담을 배열

    stack.append(start_node) # 처음에는 시작 노드 담아주고 시작하기.

    while stack: # 더 이상 방문할 노드가 없을 때까지.
        node = stack.pop() # 방문할 노드를 앞에서 부터 하나싹 꺼내기.

        if node not in visited: # 방문한 노드가 아니라면
            visited.append(node) # visited 배열에 추가
            # stack.extend(graph[node]) # 해당 노드의 자식 노드로 추가
            stack.extend(reversed(graph[node]))

    print("dfs - ", visited)
    return visited

# 함수 실행
my_dfs(myGraph, 'A')
```

- 한 줄로 입력된 정보 읽기
  
  > ```python
  > def DES(s, V):             # s시작정점, V정점 개수(1번부터)
  >     visited = [0] * (V+1)  # 방문한 정점 표시
  >     stack = []             # 스택 생성
  >     print(s)
  >     visited[s] = 1         # 시작정점 방문했다고 표시
  >     v = s
  >     while True:
  >         for w in adjL[v]:  # v에 인접하고, 방문 안한 w가 있으면
  >             if visited[w] == 0:
  >                 stack.append(v)  # push(v) 현재 정점을 push하고
  >                 v = w           # w에 방문
  >                 print(v)
  >                 visited[w] = 1  # w에 방문 표시
  >                 break            # v부터 다시 탐색
  >         else:                    # 남은 인접정점이 없어서 break가 걸리지 않은 경우
  >             if stack:
  >                 v = stack.pop()
  >             else:                 # 되돌아갈 곳이 없거나 남은 갈림길이 없으면 종료
  >                 break
  > '''
  > 1
  > 7 8
  > 1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
  > '''
  > T = int(input())
  > for tc in range(1, T+1):  # 정점 번호에 주의
  >     V, E = map(int, input().split()) 
  >     adjL = [[]for _ in range(V+1)]  # 0번은 사용x. 1번은 1에 인접인 정점이 들어갈 것
  >     arr = list(map(int, input().split()))
  >     for i in range(E):
  >         v1, v2 = arr[i*2], arr[i*2+1]
  >         adjL[v1].append(v2)   # 단방향
  >         adjL[v2].append(v1)   # 양방향
  > ```

- 예시 - 백준 1520번. 내리막 길 (DP + DFS)
  
  > - 여행을 떠난 세준이는 지도를 하나 구하였다. 이 지도는 아래 그림과 같이 직사각형 모양이며 여러 칸으로 나뉘어져 있다. 한 칸은 한 지점을 나타내는데 각 칸에는 그 지점의 높이가 쓰여 있으며, 각 지점 사이의 이동은 지도에서 상하좌우 이웃한 곳끼리만 가능하다.
  >   
  >   ![](https://upload.acmicpc.net/0e11f3db-35d2-4b01-9aa0-9a39252f05be/-/preview/)
  >   
  >   현재 제일 왼쪽 위 칸이 나타내는 지점에 있는 세준이는 제일 오른쪽 아래 칸이 나타내는 지점으로 가려고 한다. 그런데 가능한 힘을 적게 들이고 싶어 항상 높이가 더 낮은 지점으로만 이동하여 목표 지점까지 가고자 한다. 위와 같은 지도에서는 다음과 같은 세 가지 경로가 가능하다.
  >   
  >   ![](https://upload.acmicpc.net/917d0418-35db-4081-9f62-69a2cc78721e/-/preview/) ![](https://upload.acmicpc.net/1ed5b78d-a4a1-49c0-8c23-12a12e2937e1/-/preview/) ![](https://upload.acmicpc.net/e57e7ef0-cc56-4340-ba5f-b22af1789f63/-/preview/)
  >   
  >   지도가 주어질 때 이와 같이 제일 왼쪽 위 지점에서 출발하여 제일 오른쪽 아래 지점까지 항상 내리막길로만 이동하는 경로의 개수를 구하는 프로그램을 작성하시오.
  >   
  >   - 입력
  >   
  >   첫째 줄에는 지도의 세로의 크기 M과 가로의 크기 N이 빈칸을 사이에 두고 주어진다. 이어 다음 M개 줄에 걸쳐 한 줄에 N개씩 위에서부터 차례로 각 지점의 높이가 빈 칸을 사이에 두고 주어진다. M과 N은 각각 500이하의 자연수이고, 각 지점의 높이는 10000이하의 자연수이다.
  >   
  >   - 출력
  >   
  >   첫째 줄에 이동 가능한 경로의 수 H를 출력한다. 모든 입력에 대하여 H는 10억 이하의 음이 아닌 정수이다.
  >   
  >   - 예제 입력 1 복사
  >   
  >   4 5
  >   50 45 37 32 30
  >   35 50 40 20 25
  >   30 30 25 17 28
  >   27 24 22 15 10
  >   
  >   - 예제 출력 1 복사
  >   
  >   3
  >   
  >   ```python
  >   
  >   ```





---

# 너비 우선 탐색 BFS(Breadth First Search)

: 탐색 시작점의 인접한 정점들을 먼저 모두 차례로 방문한 후, 방문했던 정점을 시작점으로 하여 다시 인접한 정점들을 차례로 방문하는 방식

- 인접한 정점들에 대해 탐색을 한 후, 차례로 다시 너비우선탐색을 진행해야 하므로, 선입선출 형태의 자료구조인 큐 활용
- 알고리즘
  - 초기 상태
    - `visited`배열 초기화
    - Q 생성
    - 시작점 `enqueue`
  - 탐색 진행
    - 정점 `dequeue `
    - 방문한 것으로 표시
    - 인접된 정점들 `enqueue`

```python
def BFS(G, v):                        # 그래프 G, 탐색 시작점 v
    visited = [0] * (n+1)             # n : 정점의 개수
    queue = []                        # 큐 생성
    queue.append(v)                   # 시작점 v를 큐에 삽입
    while queue:                      # 큐가 비어있지 않은 경우
        t = queue.pop(0)              # 큐의 첫번째 원소 반환
        if not visited[t]:            # 방문되지 않은 곳이라면
            visited[t] = True         # 방문한 것으로 표시
            visit(t)                  # 정점 t에서 할 일
            for i in G[t]:            # t와 연결된 모든 정점에 대해
                if not visited[i]:    # 방문되지 않은 곳이라면
                    queue.append(i)   # 큐에 넣기
```

```python
def my_bfs(graph, start_node):
    visited = list() # 방문한 노드를 담을 배열
    queue = list() # 방문 예정인 노드를 담을 배열

    queue.append(start_node) # 처음에는 시작 노드 담아주고 시작하기.

    while queue: # 더 이상 방문할 노드가 없을 때까지.
        node = queue.pop(0) # 방문할 노드를 앞에서 부터 하나싹 꺼내기.

        if node not in visited: # 방문한 노드가 아니라면
            visited.append(node) # visited 배열에 추가
            queue.extend(graph[node]) # 해당 노드의 자식 노드로 추가

    print("bfs - ", visited)
    return visited

# 함수 실행
my_bfs(myGraph, 'A')
```
