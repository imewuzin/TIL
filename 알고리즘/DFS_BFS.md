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

---

# 너비 우선 탐색 BFS(Breadth First Search)

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
